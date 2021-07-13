# stream

## Linux:

# stream desktop:

```
ffmpeg -f x11grab -s 1920x1080 -framerate 25 -i :0.0 -c:v libx264 -preset fast -pix_fmt yuv420p -s 1280x800 -threads 0 -f flv rtmp://82.146.55.176:1935/hls/stream
```

# stream camera:

```
ffmpeg -re -i /dev/video0 -c:v libx264 -profile:v baseline -c:a libfaac -ar 44100 -ac 2 -f flv -pix_fmt yuv420p rtmp://82.146.55.176:1935/hls/stream
```
