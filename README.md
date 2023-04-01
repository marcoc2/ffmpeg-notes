# FFmpeg_notes
FFmpeg command list

".vid" = Any video format supported
".aud" = Any audio format supported

## Resize
ffmpeg -i input.vid -vf scale=$w:$h -vf format=yuv420p output.vid

## Insert audio on video
**$ ffmpeg -i a_input.aud -i v_input.vid -codec copy -shortest v_output.vid**

_Comments: Seems not to work if you put the audio file as the second input parameter_

## Remove audio from video
**$ ffmpeg -i input.vid -c copy -an output_noaudio.vid**

## Extract audio from video
**$ ffmpeg -i input.vid -c copy -vn -acodec copy audio.aac**


## Concatenate video files
**$ ffmpeg -f concat -safe 0 -i v_input1.vid -i v_input2.vid -c copy v_output.vid**

## Crop video (in frame dimension)
**$ ffmpeg -i input.mp4 -filter:v "crop=80:60:200:100" -c:a copy output.mp4**

_80:60 - Crop window size in pixes_

_200:100 - Starting from x=200, y=100 (on original image)_

## Cut video (in lenght)
**$ ffmpeg -i input.vid -ss 00:00:03 -t 00:00:08 -async 1 output.vid**

_ss - Video start time_

_t - Lenght of the part to be cut_

## Repeat video with reverted version (vai e volta)
### Without audio
**$ ffmpeg -i input.mp4 -filter_complex "[0:v]reverse,fifo[r];[0:v][r] concat=n=2:v=1 [v]" -map "[v]" output.mp4**

### With audio
**$ ffmpeg -i input.mp4 -filter_complex "[0:v]reverse,fifo[r];[0:v][0:a][r] [0:a]concat=n=2:v=1:a=1 [v] [a]" -map "[v]" -map "[a]" output.mp4**

## Convert ogv to mp4
**$ ffmpeg -i input.ogv -c:v libx264 -preset veryslow -crf 22 -c:a libmp3lame -qscale:a 2 -ac 2 -ar 44100 output.mp4**

## Convert avi to mkv
**$ ffmpeg -fflags +genpts -i input.avi -c:v copy -c:a copy output.mkv**

_Tirar "-c:v copy" e "-c:a copy" para refazer o enconding_

## MP3 to MP4 with Image

**$ ffmpeg -loop 1 -i image.png -i audio.mp3 -c:a copy -c:v libx264 -shortest video.mp4**

## Convert video to gif
This example will skip the first 30 seconds of the input and create a 3 second output. It will scale the output to be 320 pixels wide and automatically determine the height while preserving the aspect ratio. The palettegen and paletteuse filters will generate and use a custom palette generated from your source. (source: https://superuser.com/questions/556029/how-do-i-convert-a-video-to-gif-using-ffmpeg-with-reasonable-quality)

### Generate a palette:
**$ ffmpeg -y -ss 30 -t 3 -i input.flv -vf fps=10,scale=320:-1:flags=lanczos,palettegen palette.png**
### Output the GIF using the palette:
**$ ffmpeg -ss 30 -t 3 -i input.flv -i palette.png -filter_complex "fps=10,scale=320:-1:flags=lanczos[x];[x][1:v]paletteuse" output.gif**

### Other option
**$ ffmpeg -i video.avi video.gif -hide_banner**

## Create video from folder with images

**$ ffmpeg -r 5 -i filename_%02d.jpg -c:v libx264 -vf fps=25 -pix_fmt yuv420p out.mp4**

_-r images per second_

_%02d regular expression forming strings starting from 00, 01, 02, 03..._
