# FFmpeg_notes
FFmpeg command list

".vid" = Any video format supported
".aud" = Any audio format supported

### Insert audio on video
**$ ffmpeg -i a_input.aud -i v_input.vid -codec copy -shortest v_output.vid**
_Comments: Seems not to work if you put the audio file as the second input parameter_

### Concatenate video files
**$ ffmpeg -f concat -safe 0 -i v_input1.vid -i v_input2.vid -c copy v_output.vid**
