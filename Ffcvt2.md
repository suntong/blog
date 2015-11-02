# ffcvt (ffmpeg convert wrapper) for YouTube

[category Tech][tags Debian,Ubuntu,Linux,go,programming,audio,video,tool,playing,CLI]

The [`ffcvt`](https://github.com/suntong001/ffcvt) can now easily do encoding for YouTube as well.

<!--more-->

The target type `youtube` has now been added, with settings and parameters taken from [How to Encode Videos for YouTube and other Video Sharing Sites](https://trac.ffmpeg.org/wiki/Encode/YouTube). In essence, because *"Since YouTube, Vimeo, and other similar sites will re-encode anything you give it* ***the best practice is to provide the highest quality video*** *that is practical for you to upload."*, every parameter has been set to aim for that high standard. I.e., a command `ffcvt -f Whatever1.mp4 -debug 1 -force -t youtube` will do:

    ffmpeg -i Whatever1.mp4 -c:a libvorbis -q:a 5 -c:v libx264 -x264-params crf=20 -pix_fmt yuv420p -y Whatever1_.mkv

All parameters are following the recommendation from the above [official ffmpeg page](https://trac.ffmpeg.org/wiki/Encode/YouTube), just that I give `crf=20` as the default because I believe it is good enough. You can still change it to `18`, which is [often considered to be roughly "visually lossless"](https://trac.ffmpeg.org/wiki/EncodingForStreamingSites), by:

    export FFCVT_CRF="18"

Of course, you can do all kinds of other tweaking as well. For a quick test, I just use the crappy video and audio:

	export FFCVT_O="-preset veryfast -q:a 2"

and it works just fine -- https://www.youtube.com/watch?v=Rms6sDp3UNM.

Note,

- The above same command, `ffcvt -f Whatever1.mp4 -debug 1 -force -t youtube` will now do:

      `ffmpeg -i Whatever1.mp4 -c:a libvorbis -q:a 5 -c:v libx264 -x264-params crf=20 -pix_fmt yuv420p -y -preset veryfast -q:a 2 Whatever1_.mkv`
  
- The default parameter `-q:a 5` will produce a vorbis audio of 91.9kbits/s. Overriding it with `-q:a 2`, will cause the audio to become 67.5kbits/s.
- The result video looks very crappy on YouTube. It is not because of the encoding method, as the very same file used for uploading view just fine on my machine. It looks very crappy because it has been through four rounds of encoding -- the source is from YouTube itself, with original author's and YouTube's first round encoding, plus mine, then plus the final YouTube re-encoding, the forth time, which is the final push to make the video very crappy. The 67.5kbits/s, `-q:a 2` sound however, is not bad at all. 

To recap, for high standard encoding, set 

	export FFCVT_O="-preset slow"

and optionally, as explained before:

    export FFCVT_CRF="18"

Then just use `ffcvt -f` for files or `ffcvt -d` for directories. 

