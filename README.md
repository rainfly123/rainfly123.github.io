# IPTVER


## Introduction  
This is a simple iptv system tool, which is designed by Golang, It supports a few of input mode.
Such as: HTTP-TS/RTSP/HLS/RTP(UDP Mulitcast or unicast)/ RAW-TS
Output suport HTTP-TS/UDP/

## Install
1. tar xvf iptver.tar.gz 
2. edit conf.yaml
3. ./iptver  or nohup ./iptver & (yun in background)

## Aboud config

```
#channels config file
key: 19030a7be39db2fbde3560ce324fbd459fef52851ba6dcb3bf0016ab008b62a9
channels:

        - 
         type: rtsp
         name: China
         url: rtsp://183.59.156.50/PLTV/88888905/224/3221227270/10000100000000060000000001141346_0.smil?0&accountinfo=%7E%7EV2.0%7EUwOl-Gl8Z1MFFJlY1q_RCg%7EzJeEdQQK6GK7r2Q5jZjxjKYhZiHklGs8wg1XyXUSKdk5d8f2s2AYjzO2-qcyDeaExan3Pra4cuU_Whoju6BKGf666OGX7-GKwJrfpZjwyfM~ExtInfoWNHSPSTb+3AG0FnUkYLPMw=-1%2C0%2C1%2C%2C%2C2%2C%2C%2C%2C2%2CEND&GuardEncType=2
         sdev: ppp3
         dst: 239.15.11.3:1234
         dev: tap0
        - 
         type: rtsp
         name: cctv2
         url: rtsp://183.59.156.50/PLTV/88888905/224/3221227634/10000100000000060000000004662503_0.smil?0&accountinfo=%7E%7EV2.0%7Ew08QX--mxmtiRia3ZBMEZA%7EzJeEdQQK6GK7r2Q5jZjxjKYhZiHklGs8wg1XyXUSKdk5d8f2s2AYjzO2-qcyDeaExan3Pra4cuU_Whoju6BKGf666OGX7-GKwJrfpZjwyfM~ExtInfoWNHSPSTb+3AG0FnUkYLPMw=-1%2C0%2C1%2C%2C%2C2%2C%2C%2C%2C2%2CEND&GuardEncType=2
         sdev: ppp2
         dst: 239.15.11.2:1234
         dev: tap0
        - 
         type: rtp
         name: cctv3
         url: rtp://239.77.0.170:5146
         sdev: ppp1
         dst: 239.15.11.4:1234
         dev: tap0
```
key is authrised key, if it is wrong, iptver can't have channel recovery function, maybe return after days  
run the following command:
```
dmidecode -t 4 | grep ID |sort -u |awk -F': ' '{print $2}'
```
then send out to 18910158363@163.com , I will send you the key, which should be writen in the conf.yaml

1. type  input stream's mode, may be one of rtsp/rtp/httpts/hls/
2. name  channel's name , must be unique.
3. url   channel's input url
4. sdev  is the netcard name , where the live stream flow in your computer, check it by run 'ifconfig'
5. dst   is the output address, udp unicast or multicast
6. dev   is the netcard name, through it the stream flow out

## API 
```
Check bitrate:      curl http://serverip:8080/[channel name]/status 
Check all channels:  curl  http://serverip:8080/
Open with VLC:  http://serverip:8080/[channel name]/view 
```
