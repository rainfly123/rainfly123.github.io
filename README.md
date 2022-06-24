# IPTVER


## Introduction  
This is a simple iptv system tool, which is designed by Golang, It supports a few of input mode.
Such as: HTTP-TS/RTSP/HLS/RTP(UDP Mulitcast or unicast) or UDP RAW-TS.
Output suport HTTP-TS/UDP(unicast or multicast) , iptver is a substitute of udpxy.

## Install
1. tar xvf iptver.tar.gz 
2. edit your conf.yaml use your editor
3. ./iptver  or nohup ./iptver & (run in background)

## About config

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
key is authrised key, if it is wrong, iptver can't have channel recovery function, maybe return after days  
run the following command:
```
dmidecode -t 4 | grep ID |sort -u |awk -F': ' '{print $2}'
```
then send out to 18910158363@163.com , I will send you the key, which should be writen in the conf.yaml

1. type:  input stream's mode, may be one of rtsp/rtp/httpts/hls
2. name:  channel's name , must be unique.
3. url:   channel's input url
4. sdev:  is the netcard name , through which the live stream flow in your computer, check it by run 'ifconfig'
5. dst：  is the output address, udp unicast or multicast
6. dev：  is the netcard name, through which the live stream will flow out

## API 
```
Check bitrate:      curl http://serverip:8080/[channel name]/status 
Check all channels:  curl  http://serverip:8080/
Open with VLC:  http://serverip:8080/[channel name]/view 
```
