# ffmpeg
useful ffmpeg commands for producers every life

Alle wavs zu mp3
```
find *.wav -exec ffmpeg -i {} -map 0:a:0 -b:a 320k {}_2ndX_320kbps.mp3 \;
```

Video zu mp4 aac H264
```
ffmpeg -i input.XXX -vcodec libx264 -acodec aac output.mp4 (0.2 x original)
```

Video zu mp4 aac H264 720p
```
ffmpeg -i input.XXX -vf scale=-1:720 -c:v libx264 -crf 0 -preset veryslow -c:a copy MyMovie_720p.mkv
```


Video zu dnxhd (schneller als h264 - 2xoriginal)
```
ffmpeg -i input.XXX -vf "scale=1280:720,format=yuv422p" -b:v 60M -c:v dnxhd -crf 23 -preset veryslow -c:a aac output.MOV
ffmpeg -i input.XXX -vf "scale=1920:1080,format=yuv422p" -b:v 36M -c:v dnxhd -crf 35 -preset veryslow -c:a aac output.MOV
```
FB360
```
ffmpeg -i INPUTFILE -map 0:v -an -c:v dnxhd -pix_fmt yuv422p -trellis 0 -profile:v dnxhr_lb -y OUTPUTFILE.mov
```

crf: The range of the quantizer scale is 0-51: where 0 is lossless, 23 is default, and 51 is worst possible. 
A lower value is a higher quality and a subjectively sane range is 18-28. Consider 18 to be visually lossless or nearly so: 
it should look the same or nearly the same as the input but it isn't technically lossless.


PRORES (0.7 x original)
```
ffmpeg -i input.avi -c:v prores_ks -profile:v 0 -c:a pcm_s16le output.mov
```
profile:
-1: auto (default)
0: proxy ≈ 45Mbps YUV 4:2:2
1: lt ≈ 102Mbps YUV 4:2:2
2: standard ≈ 147Mbps YUV 4:2:2
3: hq ≈ 220Mbps YUV 4:2:2
4: 4444≈ 330Mbps YUVA 4:4:4:4
5: 4444xq ≈ 500Mbps YUVA 4:4:4:4

Ausschnitt codieren:
```
ffmpeg -i C0011.MP4 -ss 00:02:40 -t 00:07:25 -c:v libx264 -async 1 -c:a aac output.mov
```
Audio aus Video extrahieren (Original Codec; -vn heißt ohne Video)
```
ffmpeg -i 1st_mvt_take_5.m4v -vn -acodec copy 1st_mvt_take_5.aac
```
```
ffmpeg -formats | grep PCM
s24le           PCM signed 24-bit little-endian
```

Video nehmen, Ton wegschmeißen, neuer Ton dazu
```
ffmpeg -i input.mp4 -i input.wav -c:v copy -c:a aac -map 0:v:0 -map 1:a:0 output.mp4
```
```
ffmpeg -i AA031201.MXF -c:v prores_ks -profile:v 0 -c:a copy test.mov
```
Convert 96k/24 zu 48k/16
```
find *.wav -exec ffmpeg -i {} -map 0:a:0 -sample_fmt s16 -ar 48000 4thX_{} \;
```
Youtube Video zu mp3
```
youtube-dl -i --extract-audio --audio-format mp3 --audio-quality 0 LINK
```
