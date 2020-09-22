# WifiP2P总结

1. WIFIP2P连接成功后，要注意，只有非群主才能向群主发送Socket建立，并能成功发送数据。
   \~~~~~~~~~
   原因：当成功建立连接后，从WIFIP2PInfo中只能获取GroupOwner的IpAddress，并没有API可以提供其他Device的IpAdress。这个会导致上层应用无法获得对端Ip地址，而无法传递数据等。由于在不同的场景下，任何设备都有可能是GroupOwner，而我们除了GroupOwner的Ip Address外，还希望能获取对端以及Group中所有成员的Ip Address，包括自己的Ip Address.

2. WIFIP2P 建连后（connect方法），如果连接之前没有创建一个组，系统会自动创建一个组，并且随机分配谁是GroupOwner即谁是组长，这也关系到谁是客户端谁是服务器.

   问题：这有个坑，非常大的几率 他会一直锁定一个Device 为群主，不管是A 连接B ，还是B 连接 A, 还是A 连接C。所以一定要确定 wifiinfo 中
   wifiP2pInfo.groupFormed——–一定要为true ，这样wifiinfo才包含有用信息
   wifiP2pInfo.isGroupOwner——–一定要为false，是非群主，见1.才有可能发送信息。

   如何解决：API 也有方法提供
   调用 wifiP2pManager.createGroup(channel, new WifiP2pManager.ActionListener() {}，在未连接p2p 之前，在你想要当做服务器的（AP）的机子上提前 createGroup，并在开始连接。就可以结局随机组长的问题。

3. WifiP2pDevice 的意思是【本机设备信息】，所以千万不要误以为是获取到的是另外一台设备的设备信息。
   其中常用有：
   wifiP2pDevice.deviceName 本设备名字
   wifiP2pDevice.deviceAddress 本设备的MAC地址
   wifiP2pDevice.status 设备连接状态
   \~~~~~~~~~
   重点！！！ 千万不要以为 wifiP2pDevice.status
   （0是连接 ，1是邀请中，3是未连接，但是可用，4是不可用）
   为0的时候就是连接了，问题在于：p2p连接了，status 一定为0，但是status 为0 ，p2p不一定真正的连接了。这种情况在连接【智能设备】的时候容易出现，我就在此被坑了。真正的wifip2p 连接成功，必须要 在
   WifiP2pManager.WIFI_P2P_CONNECTION_CHANGED_ACTION
   广播中获得 为true
   intent.getParcelableExtra(WifiP2pManager.EXTRA_NETWORK_INFO).isConnected()
   并且通过
   mWifiP2pManager.requestConnectionInfo(?,?);的回调监听，拿到wifiinfo的实体 ，且不为空。 才算的上你真正的加入p2p组。

4. 至此，wifip2p 连接问题 的问题点基本已经解决。真正连接后p2p 后就要建立Socket连接。一般是没有问题的，有问题也只有2点

   1.）文件路径没有获取到，我们通过intent 获取到的 一般是 Uri
   content://com.android.providers.media.documents/document/image%3A63
   而我们想要的是文件绝对路径
   /data/hw_init/version/region_comm/china/media/Pre-loaded/Pictures/01-04.jpg
   这样才能 通过 File file = new File(path); 文件。
   所以注意这一点路径转换过程 是否为空
   ~~
   2.）socket 服务器等待方 serverSocket.accept();
   其实也只是 要保证是否在等待。
   楼主使用的是Githup的Demo ，其中serverSocket.accept(); 是在intentservice 的
   onHandleIntent 中开启的，这个intentservice 的 开启有2种方式，startService 和
   bindService 一般我们喜欢使用bindService ，毕竟数据接收后需要处理，通知UI 线程。
   但是bingService 的生命周期 是不走 onHandleIntent. 而 startService 是走的。具体看我另外一篇博客对Service 和IntentService 的 解释。（下面是链接）
   https://blog.csdn.net/qq_31332467/article/details/82430638

5. 在开发过程中发现，WIFIP2P连接，必须双向设备都处于discoverPeers状态中，如果一方不在该状态中，则另一方根本拿不到设备列表，拿不到就谈不了最基本的连接

