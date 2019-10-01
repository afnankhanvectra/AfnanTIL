## Introduction

According to Wikipedia "HTTP Live Streaming (also known as HLS) is an HTTP-based adaptive bitrate streaming communications protocol implemented by Apple Inc." 

HLS works for different bandwidth and provide the content according to user bandwidth.  Main function of HLS is to divide the main media file ( audio or video),into chunks and provide the chunk according to bandwidth.


## How it is work

1. Divide the content file into different small chunks. I use FFMPEG for this
2. Create m3u8 file that has all address of new created chunks. It must have staring address of chunk file
3. Create different chunks file with different bitrate
4. Create main m3u8 file that have address of different bitrates m3us’s address  and have information what file should run on which bitrate.
5. Call main m3u8 file for content

Lets Check every step with example


You must FFMPEG for creation of chunks file. If you don’t already install on your system, install it by using home-brew

          brew install ffmpeg


Now chunk the  file. Copy the audio or video file in folder and make empty folder by name 32, 64, 128, 324
 Open the folder location in terminal and run below commands

    ffmpeg -i Juz1.m4a -profile:v high  -level 4.0    -b:a 32k -g 20 -keyint_min 20  -start_number 0 -hls_time 6 -hls_list_size 0 -f hls  32/32.m3u8 \
    -profile:v high  -level 4.0  -b:a 64k -g 12 -keyint_min 12  -start_number 0 -hls_time 6 -hls_list_size 0 -f hls 64/64.m3u8 \
    -profile:v high  -level 4.0  -b:a 128k -g 12 -keyint_min 12  -start_number 0 -hls_time 6 -hls_list_size 0 -f hls 128/128.m3u8 \
    -profile:v high  -level 4.0  -b:a 324k -g 12 -keyint_min 12  -start_number 0 -hls_time 6 -hls_list_size 0 -f hls 324/324.m3u8

This file will make different chunk file run different bitrates in that folders . 
You can change chunk size by changing “-g 12 -keyint_min 12” parameters and bitrate by change 
 -b:a 128k  parameter

Every folder must contain a **m3u8** file that contains the chunk file address. 

To check whether this file is working on not , installl “HLS extension“ on chrome and upload somewhere this folder
And run this “m3u8”. File . It should  play


#### Create main m3u8 file
   Now create m3u8 file for main file
Create main m3u8 file
And divide the bandwidth according to  bandwidth


Create main file for bandwidth

Create main.m3u8 file and copy below code in that

     #EXTM3U
    #EXT-X-STREAM-INF:AVERAGE-BANDWIDTH=326522,BANDWIDTH=327581,CODECS="mp4a.40.2",CLOSED-CAPTIONS=NONE
    324K/324.m3u8
    #EXT-X-STREAM-INF:AVERAGE-BANDWIDTH=34525,BANDWIDTH=36451,CODECS="mp4a.40.2",CLOSED-CAPTIONS=NONE
    32K/32.m3u8
    #EXT-X-STREAM-INF:AVERAGE-BANDWIDTH=66522,BANDWIDTH=67860,CODECS="mp4a.40.2",CLOSED-CAPTIONS=NONE
     64K/64.m3u8
    #EXT-X-STREAM-INF:AVERAGE-BANDWIDTH=130522,BANDWIDTH=131467,CODECS="mp4a.40.2",CLOSED-CAPTIONS=NONE
     128K/128.m3u8

and upload all folder
Remember  "128K/128.m3u8" and all these are folders and path

Now run the  main.m3u8 file . It will play audio or video according to bandwidth. It will run one chunk of file and before end the file , it will check for bandwidth , if bandwidth has been changed then run next file from different bandwidth else  load from same bandwidth. So chunk size should be optimistic

<https://en.wikipedia.org/wiki/HTTP_Live_Streaming>
