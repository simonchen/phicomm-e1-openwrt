# phicomm-e1-openwrt
无需刷机，更改中继信号的固定带宽
斐讯自带的固件其实是OpenWRT官改的版本，可以很方便设置无线2.4/5G信号的中继，但是没有提供选项强制设置带宽，比如无线5G信号的中继带宽我想强制40MHZ，每次设置后，自动都是80MHZ，懂的人都知道，5G信号穿墙能力很弱，并不是带宽越大越好，保持40MHZ的带宽能让网速更稳定，避免断流。

要想设置固定的带宽，只能想想其它办法了，



1. 首先，确保关闭中继，恢复到原出厂设置，然后，连上网线，运行Telnet激活：(192.168.8.1 是e1默认的地址 )

RoutAck.exe 192.168.8.1

*RoutAck.exe在附图中提供的网盘中下载

<img src="pan.png" />

2. 激活telnet以后，你可以设置好中继， 然后，运行telnet命令访问e1路由，这时候你要去你的上一级中继路由里看下e1的中断后的i/p地址（比如192.168.1.121)

现在我们可以断开网线，直接连wifi，用telnet访问e1路由，这样比连网线更方便操作。

telnet 192.168.1.121

进去后，运行命令

vi /etc/config/wireless

按下'i'键进入编辑模式，以5G信号中继为例，找到config wifi-device 'mt7612e', 在下面找到option bw '2' , 在前面加个'#'号， 目的是失效掉这个参数，

然后，添加一行

option htmode 'HT40+'

按ESC键完成编辑, 再同时按shift+:（冒号) ， 输入wq， 回车完成保存。



*我在Openwrt wiki上找半天也没找到这个option bw 参数意思，估计是bandwidth的缩写，是官改定制的，默认让带宽翻倍，比如前一个要中继的路由40MHZ带宽，翻倍后e1的带宽就是80MHZ

<img src="wireless.png" />

3. 重启e1无线网络

在telnet模式下运行

/etc/init.d/network restart



重启大约20秒钟，你可以在其它的wifi检查软件中查看是不是5G信号是不是变成了固定的40MHZ带宽了，如果不是，回头再检查下/etc/config/wireless是不是改错了，每次保存更改后，都要重启e1无线network 。

