# rainfly123.github.io
A tool for proxy RTSP/HLS/HTTP+TS/RTP live stream to HTTP+TS  or UDP  multicast /unicast streams
## IPTVER
这是一款直播流代理工具， 可以把RTSP、HLS、HTTP-TS、RTP流拉取到本地， 然后对外提供HTTP-TS 播放格式，也可以支持UDP （单播、组播输出）
某种程度上可以替换掉udpxy，但是功能比他多，欢迎使用。
## 安装

[下载页面]
暂不公开

首先通过以上网址，下载安装包。
1.  `tar xvf iptver.tar.gz` 解压缩包
2.  `vim conf.yaml`  编辑频道配置文件
3. `./iptver  或者后台运行 nohup ./iptver &` 
4. rain.log 是日志文件

## 关于配置

```
#channels config file
key: a965c8b327878f44992589877ba9f18fe25c9b7ee1b934ee167cc1228e717182
channels:

        - 
         type: rtsp
         name: cctv1
         url: rtsp://127.0.0.1:8554/abcd
         sdev: lo
         dst: 10.200.69.202:1234
         dev: eth0
        - 
         type: hls
         name: cctv2
         url: http://aliplay.17.cn/live/5ca329bd828bb609ea30ccfc.m3u8
         sdev: eth0
         dst: 10.200.69.202:1235
         dev: eth0

        - 
         type: httpts
         name: cctv3
         url: http://aliplay.ye.cn/17e/5ca329bd828bb609ea30ccf
         sdev: eth0
         dst: 10.8.14.177:1236
         dev: eth0

        - 
         type: rtp
         name: cctv4
         url: rtp://22.2.13.44:2332
         sdev: eth0
         dst: 10.8.14.177:1236
         dev: eth0


```

 ### 配置文件说明
 - key 授权码， 如果没有经过授权，程序不具备频道自恢复能力， 运行几天后随时有可能自动退出。 
 - type 频道输入格式 rtsp/rtp/httpts/hls 四中其一
 - name 频道名称，全局唯一，不可重复，可以是中文
 - url 频道输入地址
 - sdev 频道的数据流入网卡名称， 比如eth0、enps1 ，可以通过ifconfig 查看
 - dst 该频道转发的目的地址， 可以说UDP单播、组播。如果不关心可以设置成127.0.0.1:61234
 - dev 频道转发的流出网卡
 
## API
目前比较简单：
 - 查看频道码率 curl http://serverip:8080/[channel name]/status
   注意更改server ip 和 channel name，比如：
 curl http://192.168.3.233:8080/cctv1/status 
 查看cctv1的输入码率，192.168.3.233 是运行iptver的服务器IP地址。
 - 查看全部频道配置 curl  http://serverip:8080/
 - 观看频道直播，打开VLC 播放器，点击’打开网络串流‘，输入频道流地址，比如http://serverip:8080/[channel name]/view 
 - http://serverip:8080/chan/add 增加频道, POST 方式提交，内容是JSON，参数参照配置文件
 - curl http://serverip:8080/[channel name]/del 删除频道
 
## 关于授权
 原则采用单机授权方式， 每台机器一个授权码，
执行以下命令：
```bash
dmidecode -t 4 | grep ID |sort -u |awk -F': ' '{print $2}'
```
可获取CPU信息，把信息发送到18910158363@163.com  24小时内回复授权码。

收到的授权码写进conf.yaml 的key 字段即可。
这是我的微信：Rainfly003 ，有问题可以添加我的微信联系。 
或 [威堡IPTV系统解决方案专家](http://www.wephd.net/)


## QA
1. 如何关闭组播输出？
   很简单 dst 字段留空 即可
2. 上游频道服务中断，需要重启iptver吗？
    不需要， 它本身具有频道自恢复能力，每30秒尝试重新链接上游频道
3. 增加频道会自动启动吗？
   如果参数都没问题， 添加频道成功会马上启动频道， 同理删除频道，马上停止频道
