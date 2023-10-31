# Images and Video

## Use ffmpeg to reduce a video file size.

    # get bit size
    $ ffmpeg -i some_file.mp4
    # Reduce bitrate ...
    $ ffmpeg -i some_file.mp4 -b 800k output.mp4
    # Reduce `constant rate factor` CRF -better quality-
    $ ffmpeg -i some_file.mp4 -vcodec libx264 -crf 20 output.mp4

Reduce the resolution of the video to 720p

    $ ffmpeg -i input.mov -vcodec libx264 -b:v 1000k -filter:v "scale=720:-1" output.mp4

Reduce the resolution of the video to 480p

    $ ffmpeg -i input.mov -vcodec libx264 -b:v 1000k -filter:v "scale=480:-1" output.mp4

## Use ffmpeg to crop video

Crop 500px from top and bottom, used to crop a vertical video to horizontal

    $ ffmpeg -i in.mp4 -filter:v "crop=in_w:in_h-1000" out.mp4

Preview crop video with `ffplay`

    $ ffplay -i in.mp4 -vf "crop=in_w:in_h-1000"

## Find jpgs larger than 110k

    # linux
    $ find -iname "*.jpg" -type f -size +110k -exec ls -lh {} \; | awk '{print $9 "|| Size: " $5}'

Use `mogrify` to reduce size

    # linux
    $ find -iname "*.jpg" -type f -size +110k -exec mogrify -strip -resize 800x -quality 75 {} \;

