# ffmpeg常用命令

用来下载m3u8文件

`ffmpeg -i index.m3u8 out.ts`

指定文件类型白名单下载

`ffmpeg -protocol_whitelist "file,https,crypto,tcp,tls" -i index.m3u8 -c copy out.ts`



截图：从第三秒开始 `-ss`表示开始的时间

`ffmpeg -i video_path -y -f mjpeg -ss 3 -t 1  output_path.jpg`



这将每秒依据cutVideo.mp4生成一个图片命名为foo-001.jpeg ,foo-002.jpeg以此类推,图片尺寸是1680*1050。

```
ffmpeg -i cutVideo.mp4 -r 1 -s 1680*1050 -f image2 foo-%03d.jpeg
```

比如一个视频源的码率太高了，有10Mbps，文件太大，想把文件弄小一点，但是又不破坏分辨率。  `ffmpeg -i input.mp4 -b:v 2000k output.mp4`  上面把码率从原码率转成2Mbps码率，这样其实也间接让文件变小了。目测接近一半。  



有时候，下载了某个网站的视频，但是有logo很烦，咋办？有办法，用ffmpeg的delogo过滤器。 
语法：-vf delogo=x:y:w:h[:t[:show]] 
x:y 离左上角的坐标 
w:h logo的宽和高 
t: 矩形边缘的厚度默认值4 
show：若设置为1有一个绿色的矩形，默认值0。

`ffmpeg -i input.mp4 -vf delogo=0:0:220:90:100:1 output.mp4` 