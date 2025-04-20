# Batch Generation of Test Video Assets Using FFmpeg

* For users requiring multiple test video assets, perhaps for integration within an Online Video Platform (OVP), this document outlines an efficient method to generate a series of visually distinct videos using a single FFmpeg command.

~~~```bash
for i in <span class="math-inline">\(seq 1 12\); do ffmpeg \-f lavfi \-i "color\=c\=0x</span>(openssl rand -hex 3):s=1280x720:d=180" -vf "drawtext=fontfile=/path/to/font.ttf:text='myVideo':fontcolor=white:fontsize=72:x=(w-text_w)/2:y=(h-text_h)/2,drawtext=fontfile=/path/to/font.ttf:text='%{eif\:180-t\:d\:2}':fontcolor=white:fontsize=144:x=(w-text_w)/2:y=(h-text_h)/2+100" -c:v libx264 -crf 23 -preset medium -c:a aac -b:a 128k "myVideo_$i.mp4"; done
~~~

The underlying logic of this script, detailed in the sequence diagram in Figure 1, involves iteratively creating videos with random background colours and a simple countdown.

![image](https://github.com/user-attachments/assets/3e5314fa-1ae2-4d7a-aef4-12c7cb1043b5)

Figure 1. FFmpeg Batch Countdown Video Script Sequence.

* For scenarios requiring a single video generation, or where a more refined countdown display and the inclusion of colour bars are desired, the subsequent FFmpeg command provides an enhanced solution. This involves generating a test source pattern with colour bars and overlaying a more detailed minute:second countdown.

~~~```bash
ffmpeg -f lavfi -i testsrc=duration=180:size=1920x1080:rate=30 \
-vf "drawtext=fontfile=/path/to/font.ttf:text='myVideo':fontcolor=black:fontsize=100:x=(w-text_w)/2:y=(h-text_h)/2-50,drawtext=fontfile=/path/to/font.ttf:text='%{eif\\:trunc((180-t)/60)\\:d}\\:%{eif\\:mod(180-t\\,60)\\:d\\:2}':fontcolor=black:fontsize=72:x=(w-text_w)/2:y=(h-text_h)/2+50,format=yuv420p" \
-c:v libx264 -crf 18 -preset veryfast -t 180 myVideo.mp4
~~~

The process for this single video generation, illustrated in Figure 2, involves creating a video with colour bars, a static title, and a minute:second countdown timer.

![image](https://github.com/user-attachments/assets/a1426af1-9634-4f35-9c6f-3bcecae2ef5b)

Figure 2. FFmpeg Single Test Video Asset Generation.
