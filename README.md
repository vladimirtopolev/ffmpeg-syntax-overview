# 0. Extract metadata of video
```
ffprobe \
    -v quiet \
    -print_format json \
    -show_format \
    -show_streams \
    ./media/cmd00/input.mp4 \
    > meta.json
```
or
```
ffmpeg -i ./media/cmd00/input.mp4 
```


# 1. Simple filter
## 1.1 Crop video with dimension 640x360 into video with dimension 202x360 (cropped part should be centered relatively the main video):
```
ffmpeg \
    -i ./media/cmd01/input.mp4 \
    -vf "crop=x=219:y=0:w=202:h=360" \
     ./cmd01_out_01.mp4
```
You may also use the short version of command and skip parameters name, but you need
follow the right order of parameters according to doc:
```
ffmpeg \
    -i ./media/cmd01/input.mp4 \
    -vf "crop=202:360:219:0" \
     ./cmd01_out_01.mp4
```
## 1.2 Crop a landscape video into a portrait video with aspect ration 9x16 (cropped part should be centered relatively the main video):
```
ffmpeg \
    -i ./media/cmd01/input.mp4 \
    -vf "crop=x=(in_w-ow)/2:y=0:w=in_h*9/16:h=in_h" \
    ./cmd01_out_02.mp4
```

# 2. Filter chain
At first we need to crop landscape video into partrait with aspect ratio 9x16 and after horizontally flip it
```
ffmpeg \
    -i ./media/cmd02/input.mp4 \
    -vf "crop=x=(in_w-ow)/2:y=0:w=in_h*9/16:h=in_hhflip" \
    ./cmd02_out.mp4
```

# 3. Filter Graphs
```
ffmpeg \
    -i ./media/cmd03/laptop_stream.mov \
    -i ./media/cmd03/stage_stream.mp4 \
    -i ./media/cmd03/logo.jpg \
    -filter_complex "[0][1:v]overlay=x=W-w:y=H-h:shortest=1[screenWithStage];[screenWithStage][2]overlay" \
    ./cmd03_out.mp4
```

