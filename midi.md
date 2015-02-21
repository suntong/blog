# How to play midi files under Ubuntu Linux

[category Tech][tags Debian,Ubuntu,Linux,midi,player]

Playing midi under Linux is an old topic that nobody talks about nowadays. Hence all documents/blogs/how-tos on playing midi under Linux are more or less outdated. I've exhausted all my google searched but still having trouble to piece together the puzzles into a whole piece. 

Finally with the help from [John O'M](http://superuser.com/users/414099/john-om), it is working for me now. Here is how I get it working.

<!--more-->

## The problem

After an exhausted search from Google, this is what I came back as the conclusion: 

    sudo apt-get install timidity timidity-interfaces-extra

However, all that I got is:

    $ timidity awesometune.mid
    /etc/timidity/freepats.cfg: No such file or directory
    timidity: Error reading configuration file.
    Please check /etc/timidity/timidity.cfg

Following another article and adding `fluid-soundfont-gm` to the installation does not solve the problem at all. 

## The solution

Thanks to John O'M, The puzzle is now complete. What's missing is the package `freepats`, available from Ubuntu 14.04 onward.

So the total solution for play midi files under Ubuntu is:

0. Install all required pacages by running

        sudo apt-get install freepats timidity timidity-interfaces-extra

0. Run `timidity -iA` in terminal first to check/get started. It should give something like this

        $ timidity -iA
        Requested buffer size 32768, fragment size 8192
        ALSA pcm 'default' set buffer size 32768, period size 8192 bytes
        TiMidity starting in ALSA server mode
        Opening sequencer port: 129:0 129:1 129:2 129:3

If so, congratulations, your midi is working.  
You can play a midi file directly from the command timidity followed by the file name. For example,

    timidity awesometune.mid

Or, if you want a GUI, you can simply type

    timidity -ig

and an old-fashioned window will pop up.

![timidity-ig](http://i.stack.imgur.com/7I48E.png)

## The explanation

### Alternative

This only works in terminal and can only play one midi file at a time.

If you want a playlist and graphical interface and controls, a proposed solution is Audacious, I.e.,

Audacious + AMidi Plug Plugin + fluid-soundfont-gm

But I don't know how legitimate or how feasible is that, because it is from the same author that said "timidity + fluid-soundfont-gm" will work and provide "excellent sound". 

### Back to timidity

In fact, although `timidity` is a command line tool, it can not only play files one-at-a-time, but also can play an entire directory. Timidity will also direct output to audio file (could be mp3 or ogg) with the identical sound so you can use them on any player. Good to share the MIDIs with those who don't have proper midi player or good soundfont.

Moreover, with the `timidity-interfaces-extra`, it allows you to play midi files from your file browser as well. It will let you select which interface you prefer (`timidity -ia` = default Gnome; `timidity -ig` = GTK interface; `timidity -ik` = TKM interface).
Make sure that you specify "Open With" to point to timidity. So, the command attached to your timidity icon should read `timidity -ig` (if that is your choice).

### Rants

Feel free to skip this ranting section if you want.

The Ubuntu [Software Synthesis HowTo](https://help.ubuntu.com/community/Midi/SoftwareSynthesisHowTo) is the fist hit that I visited, but that page is full of outdated info.

#### MIDI Tools

The above wiki says:

> There are three main programs that do software synthesis: TiMidity++, Fluidsynth and ZynAddSubFX
>
>  ZynAddSubFX is easiest to use when you want to output a single instrument, as it does not require samples or soundfonts. When you want to play a MIDI stream with multiple instruments, such as a General MIDI file, FluidSynth or Timidity++ are an easier fit. FluidSynth has a nice GUI, but you will have to search for a suitable soundfont to go with it. TiMidity++ is a bit harder to install and use, but you can easily install a sample set for it from the repostories.

My first impression is,

0. ZynAddSubFX is easiest to play a MIDI stream with a single instrument
0. FluidSynth can handle multiple instruments but you will have to search for a suitable soundfont to go with it.
0. TiMidity++ is a bit harder to install and use than FluidSynth

If you have such similar impression as mine, or your agree with my understanding from above wiki article, then congratulation, you have also been successfully fooled by the wiki article, because

* ZynAddSubFX will not help your to play a MIDI file.
* TiMidity is not hard to install at all. FluidSynth is the hardest. 

#### The freepats samples

Still on the above wiki, at:
https://help.ubuntu.com/community/Midi/SoftwareSynthesisHowTo#Install_samples
it says this sample set (freepats) "is incomplete at the moment and doesn't cover the whole General MIDI standard yet". I'm sure it is true, but I have to say it is very misleading -- to me "incomplete" means work stopped half the way, but I played a pop music, which consists of 18 instrument tracks, I get this:

```
Format: 1  Tracks: 29  Divisions: 384
Track name: Vocals
Track name: Shaker
Track name: Maracas
Track name: Bass Drum
Track name: Open Hi Hat
Track name: Side Stick
Track name: Toms
Track name: Crash Cymbal
Track name: Splash Cymbal
Track name: Snare Drum
Track name: Synth Brass
Track name: Strings
Track name: Synth Strings
Track name: BASS
Track name: Clean Guitar
Track name: New Age
Track name: Distrortion Guitar
Track name: Overdriven Guitar
Track name: ...
No instrument mapped to tone bank 0, program 49 - this instrument will not be heard
No instrument mapped to tone bank 0, program 51 - this instrument will not be heard
No instrument mapped to tone bank 0, program 63 - this instrument will not be heard
Playing time: ~345 seconds
Notes cut: 0
Notes lost totally: 0
```

That's much more than I expected. I don't care if the "New Age", or "Distrortion Guitar" or "Overdriven Guitar" is available or not, the more basic things like Shaker, Drum or Brass etc are more important to me. As long as they are there, missing some bells or whistles is not that a huge issue to me. 

#### The CPU Usage

Still that wiki, it tells people how to fix TiMidity if it uses too much CPU. Well, when I play the above 15-instrument midi, my 10+ years old CPU barely shows any usage. The CPU graph is flat at the bottom, barely see any dent. 

## **Ref**:

https://help.ubuntu.com/community/Midi/SoftwareSynthesisHowTo  
http://blog.thameera.com/playing-midi-files-in-ubuntu/

