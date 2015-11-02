# ffcvt - ffmpeg convert wrapper tool

[category Tech][tags Debian,Ubuntu,Linux,go,programming,audio,video,tool,playing,CLI]

Debut blog for [`ffcvt`](https://github.com/suntong001/ffcvt), the `ffmpeg` wrapper to convert audio/video files.

<!--more-->

## Introduction

- The next-generation [High Efficiency Video codec (HEVC), H.265](https://goo.gl/IZrDH2) can produce videos visually comparable to libx264's result, but in [about half the file size](https://trac.ffmpeg.org/wiki/Encode/H.265).
- Meanwhile the [Opus](https://goo.gl/BPUkTf) [audio codec](https://goo.gl/IZrDH2) is becoming the best thing ever for compressing audio -- A 64K Opus audio stream is comparable to mp3 files of 128K to 256K bandwidth.
- Such fantastic high efficiency audio/video codec/encoding capability has long been available in `ffmpeg`, but fewer people know it or use it, partly because the `ffmpeg` command line is not that simple for every one.
- The `ffcvt` is designed to take the burden from normal Joe -- All you need to do to encode a video is to give one parameter to `ffcvt`, i.e., the path and file name of the video to be encoded, and `ffcvt` will take care of the rest, using the recommended values for both audio/video encoding to properly encode it for you.
- It can't be more simpler than that. However, beneath the simple surface, `ffcvt` is versatile and powerful enough to allow you to touch every corner of audio/video encoding. There is a huge list of environment variables which will allow you tweak the encoding methods and parameters to exactly what you prefer instead.
- Moreover, to encode a directory full of video files, including under its sub-directories, you need just to give `ffcvt` one single parameter, the directory location, and `ffcvt` will go ahead and encode all video files under that directory, including all its sub-directories as well. 

## Quick Usage

There is a quick usage help that comes with `ffcvt`, produced when it is invoked without any parameters.

The `ffcvt -f testf.mp4 -debug 1 -force` will invoke

    ffmpeg -i testf.mp4 -c:a libopus -b:a 64k -c:v libx265 -x265-params crf=28 -y testf_.mkv

To use `preset`, do the following or set it in env var FFCVT_O

    cm=medium
    ffcvt -f testf.mp4 -debug 1 -force -suf $cm -- -preset $cm

Which will invoke

    ffmpeg -i testf.mp4 -c:a libopus -b:a 64k -c:v libx265 -x265-params crf=28 -y -preset medium testf_medium_.mkv

Here are the final sizes and the conversion time (in seconds):

	2916841  testf.mp4
	1807513  testf_.mkv
	1743701  testf_veryfast_.mkv   41
	2111667  testf_faster_.mkv     44
	1793216  testf_fast_.mkv       85
	1807513  testf_medium_.mkv    120
	1628502  testf_slow_.mkv      366
	1521889  testf_slower_.mkv    964
	1531154  testf_veryslow_.mkv 1413

I.e., if `preset` is not used, the default is `medium`.

Here is another set of results, sizes and the conversion time (in minutes):

	171019470  testf.avi
	 55114663  testf_veryfast_.mkv  39.2
	 57287586  testf_faster_.mkv    51.07
	 52950504  testf_fast_.mkv     147.11
	 55641838  testf_medium_.mkv   174.25

## Preset Method Comparison

The `ffmpeg` `x265` `preset` determines how fast the encoding process will be. if you choose `ultrafast`, the encoding process is going to run fast, but the file size will be larger when compared to `medium`. [The visual quality will be the same](https://trac.ffmpeg.org/wiki/Encode/H.265). Valid presets are `ultrafast`, `superfast`, `veryfast`, `faster`, `fast`, `medium`, `slow`, `slower`, `veryslow` and `placebo`.

Because that [the visual quality are the same](https://trac.ffmpeg.org/wiki/Encode/H.265), so there is no need to go for the slower options, because you won't be gaining anything but for the final file size. Therefore, check for yourself the above result file sizes and the conversion times, then pick a preset level you feel comfortable. The following present the same data in graphs. Click on them each to bring up bigger and most importantly, interactive graph.

[![preset small](https://github.com/suntong001/ffcvt/raw/master/preset-small.png)](https://fiddle.jshell.net/cL2q5p1z/3/show/ "Preset Small")

[![preset large](https://github.com/suntong001/ffcvt/raw/master/preset-large.png)](https://fiddle.jshell.net/nfLfd9p6/show/ "Preset Large")

I personally would go for `veryfast` because it produces the final size not much different than `fast`, `medium`, but only take less than a quarter of the time (but you may choose anything). I.e., I'll use 

    export FFCVT_O="-preset veryfast"

so as to avoid specifying it each time when invoking `ffcvt`.

## Tools Choices

As suggested before, don't use `avconv`, use `ffmpeg` instead (the `avconv` fork was more for political reasons. I personally believe `ffmpeg` is technically superior although might not be politically).

As for video/movie play back, use [mpv](http://mpv.io/). It is a fork of mplayer2 and MPlayer, and is a true *modern* *all-in-one* movie player that can play ANYTHING, and one of the few movie players being actively developed all the time. Download link is in [mpv.io](http://mpv.io/), from which Ubuntu repo I get my Ubuntu `ffmpeg` package as well. 

Give [`ffcvt`](https://github.com/suntong001/ffcvt) a try. 
