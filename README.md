## nicklansley/allav Docker Image
This project builds an image that provides a complete self-contained library of the very best open-source audio and video converters using an "all batteries included" approach. This means that you can do just about anything these applications will let you because all libraries are compiled into the binary. 
I'll leave you to research and master all the command-line options available, especially for FFmpeg(!). 

allav is pronounced "all ay vee"

Currently, the two installed applications are:
* <b>ffmpeg  </b><i>v4.2.1 "Ada"</i> - powerful program that can convert just about any media file to just about any other format!
* <b>lame  </b><i>v3.99.5</i> - popular MP3 encoder to create the very best MP3s.

Both applications are compiled from source code so a first run of the Dockerfile will take a few minutes
while all this happens. I chose the latest Ubuntu Linux image as it has the all the needed libraries in apt, and compatible with the source libraries and compilers used.

To pull and build the image using Docker (as it is in Docker Hub):
<pre>docker pull nicklansley/allav</pre>

Otherwise you will need to build it by opening a terminal window in the root of this repository and running this command:
<pre>docker build -t allav .</pre>
The build will take a while because a serious amount of compiling of source code takes place, especially
for FFMpeg. Pulling a ready-made image from Docker Hub is there for a reason...! 

### How to use this docker image to convert your audio/video files
Using 'docker run' you attach a volume of any name to a host directory where your media file is stored and then run either FFmpeg or Lame on the file in the attached volume name. 

In the examples below, a directory called '/av' is created within the container, which maps to the current directory on your host computer (in this case I am using Windows Powershell and ${PWD} is a variable describing the current working directory). 
You then tell FFMpeg or Lame where the input file is and where the output will be saved by using the file path in the format '/av/myfile'. Take a look at these two examples:

1 > Use FFMpeg to extract the audio from the video file into a WAV file:
<pre>docker run -v ${PWD}:/av/ allav ffmpeg -i /av/nicklansley-allav-testfile.mp4 /av/audiofromvideo.wav</pre>
2 > Use Lame to convert the WAV file to MP3 format:
<pre>docker run -v ${PWD}:/av/ allav lame -h -V 0  /av/audiofromvideo.wav /av/audiofromvideo.mp3</pre>
3 > Here I use FFMpeg to convert an MP4 file in my Windows Downloads folder to Windows WMV format:
<pre>docker run -v C:\Users\nick\Downloads:/av/ allav ffmpeg -i /av/nicklansley-allav-testfile.mp4 /av/audiofromvideo.wmv</pre>
4 > Here I use FFMpeg to extract a frame from the video file at 1 second intervals (in terms of file realtime playback) and save them as incremental PNG image files:
<pre>docker run -v C:\Users\nick\Downloads:/av/ allav ffmpeg -i /av/nicklansley-allav-testfile.mp4 -r 1 -f image2 image-%2d.png</pre>

## Which FFMpeg Libraries have been enabled?
As well as the standard libraries for video, audio and image that is native to FFMpeg, the following external
libraries have been compiled into the version used in this image:
<table>
<tr><td>libx265</td><td>H.265/HEVC video encoder. See the <a href="https://trac.ffmpeg.org/wiki/Encode/H.265">H.265 Encoding Guide</a> for more information and usage examples. </td></tr>
<tr><td>libx264</td><td>H.264 video encoder. See the <a href="https://trac.ffmpeg.org/wiki/Encode/H.264">H.264 Encoding Guide</a> for more information and usage examples.</td></tr>
<tr><td>libvpx</td><td>VP8/VP9 video encoder/decoder. See the <a href="https://trac.ffmpeg.org/wiki/Encode/VP9">VP9 Video Encoding Guide</a> for more information and usage examples. </td></tr>
<tr><td>libfdk-aac</td><td>AAC audio encoder. See the <a href="https://trac.ffmpeg.org/wiki/Encode/AAC">AAC Audio Encoding Guide</a> for more information and usage examples. </td></tr>
<tr><td>libmp3lame</td><td>MP3 audio encoder - same as used by the Lame application although the latter is easier to use! </td></tr>
<tr><td>libopus</td><td>Opus audio decoder and encoder. </td></tr>
<tr><td>libass</td><td>Subtitle renderer for the ASS/SSA (Advanced Substation Alpha/Substation Alpha) subtitle format.- see <a href="https://github.com/libass/libass">LibAss Github Repo for more details</a></td></tr>
<tr><td>libfreetype</td><td>Font renderer - see <a href="https://www.freetype.org/">https://www.freetype.org/</a> for more information</td></tr>
<tr><td>libtheora</td><td>Theora, the free and open video compression format from the Xiph.org Foundation</td></tr>
<tr><td>libvorbis</td><td>Ogg Vorbis is a fully open, non-proprietary, patent-and-royalty-free, general-purpose compressed audio format for mid to high quality (8kHz-48.0kHz, 16+ bit, polyphonic) audio and music at fixed and variable bitrates from 16 to 128 kbps/channel. See <a href="https://xiph.org/vorbis/">https://xiph.org/vorbis/</a> for more information</td></tr>
</table>