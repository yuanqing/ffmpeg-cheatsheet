# FFmpeg Cheatsheet

> A cheatsheet for common video processing operations in [FFmpeg](https://ffmpeg.org)

## Operations

### Crop

> [Reference](https://ffmpeg.org/ffmpeg-filters.html#crop)

```sh
# Crop to width 360, height 640
$ ffmpeg -i input.mov -filter:v 'crop=360:640:0:0' -codec:a copy output.mov

# Crop to width 360, height 640, starting from coordinates (10, 20)
$ ffmpeg -i input.mov -filter:v 'crop=360:640:10:20' -codec:a copy output.mov
```

### Format

> [Reference](https://stackoverflow.com/questions/8075992/using-ffmpeg-convert-a-file-from-one-output)

```sh
# Convert to GIF
$ ffmpeg -i input.mov output.gif

# Convert from GIF
$ ffmpeg -i input.gif output.mov

# Convert between non-GIF formats
$ ffmpeg -i input.mov -codec:v copy -codec:a copy output.mp4
```

### Frame rate

> [Reference](https://trac.ffmpeg.org/wiki/ChangingFrameRate)

```sh
# Change the frame rate to 12
$ ffmpeg -i input.mov -filter:v 'fps=fps=12' -codec:a copy output.mov
```

### Strip audio

> [Reference](https://superuser.com/questions/268985/remove-audio-from-video-file-with-ffmpeg)

```sh
# Strip audio
$ ffmpeg -i input.mov -codec:v copy -an output.mov
```

### Resize

> [Reference](https://trac.ffmpeg.org/wiki/Scaling)

```sh
# Resize to width 360, height 640
$ ffmpeg -i input.mov -filter:v 'scale=360:640' -codec:a copy output.mov

# Resize to width 360, maintaining the aspect ratio
$ ffmpeg -i input.mov -filter:v 'scale=360:-1' -codec:a copy output.mov

# Resize to height 640, maintaining the aspect ratio
$ ffmpeg -i input.mov -filter:v 'scale=-1:640' -codec:a copy output.mov
```

- Set either `width` or `height` to `-1` to maintain the aspect ratio.

### Reverse

> [Reference](https://video.stackexchange.com/questions/17738/how-to-use-ffmpeg-command-for-reverse-video)

```sh
# Reverse
$ ffmpeg -i input.mov -filter:v 'reverse' -filter:a 'areverse' output.mov
```

### Rotate

> [Reference](https://stackoverflow.com/questions/3937387/rotating-videos-with-ffmpeg)

```sh
# Rotate 90 degrees clockwise
$ ffmpeg -i input.mov -filter:v 'transpose=1' -codec:a copy output.mov

# Rotate 90 degrees counter-clockwise
$ ffmpeg -i input.mov -filter:v 'transpose=2' -codec:a copy output.mov

# Rotate 180 degrees
$ ffmpeg -i input.mov -filter:v 'transpose=1,transpose=1' -codec:a copy output.mov
```

### Speed

> [Reference](https://trac.ffmpeg.org/wiki/How%20to%20speed%20up%20/%20slow%20down%20a%20video)

```sh
# Quarter the speed
$ ffmpeg -i input.mov -filter:v 'setpts=4*PTS' -filter:a 'atempo=0.5,atempo=0.5' output.mov

# Halve the speed
$ ffmpeg -i input.mov -filter:v 'setpts=2*PTS' -filter:a 'atempo=0.5' output.mov

# Double the speed
$ ffmpeg -i input.mov -filter:v 'setpts=0.5*PTS' -filter:a 'atempo=2' output.mov

# Quadruple the speed
$ ffmpeg -i input.mov -filter:v 'setpts=0.25*PTS' -filter:a 'atempo=2,atempo=2' output.mov
```

- Use the formula `1 ÷ speed` to compute the value of `setpts`.
  - Half the speed: `setpts=2*PTS` since `1 ÷ 0.5 = 2`.
  - Double the speed: `setpts=0.5*PTS` since `1 ÷ 2 = 0.5`.

- The value of each `atempo` filter must be between 0.5 and 2.
  - Quarter the speed: `atempo=0.5,atempo=0.5` since `0.5 × 0.5 = 0.25`.
  - Quadruple the speed: `atempo=2,atempo=2` since `2 × 2 = 4`.

### Trim

> [Reference](https://trac.ffmpeg.org/wiki/Seeking#Cuttingsmallsections)

```sh
# Trim from time 0:05 to the end of the input file
$ ffmpeg -ss 0:05 -i input.mov -codec:v copy -codec:a copy output.mov

# Trim from time 0:05 to 0:10
$ ffmpeg -ss 0:05 -to 0:10 -i input.mov -codec:v copy -codec:a copy output.mov
```

- The `-ss` and `-to` flags must come *before* the `-i` flag.

### Volume

> [Reference](https://trac.ffmpeg.org/wiki/AudioVolume)

```sh
# Halve the volume
$ ffmpeg -i input.mov -codec:v copy -filter:a 'volume=0.5' output.mov

# Double the volume
$ ffmpeg -i input.mov -codec:v copy -filter:a 'volume=2' output.mov
```

## Relevant CLI flags

> [Reference](https://ffmpeg.org/ffmpeg.html)

```
ffmpeg
  -an
  -ss <timestamp>
  -to <timestamp>
  -i <input>
  -codec:a <codec>
  -codec:v <codec>
  -filter:a <filtergraph>
  -filter:v <filtergraph>
  -y
  <output>
```

## See also

- [vdx](https://github.com/yuanqing/vdx)
