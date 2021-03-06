**HQStreamer2** - Stream audio between DAWs across the internet
Copyright (C) 2020 Sauraen, <sauraen@gmail.com>

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU Affero General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU Affero General Public License for more details.

You should have received a copy of the GNU Affero General Public License
along with this program.  If not, see <https://www.gnu.org/licenses/>.

## Download Instructions

Downloads are in the `Downloads` folder above. Here are detailed download/install/run instructions [for Windows](https://github.com/sauraen/HQStreamer2/wiki/Download-and-run-instructions---Windows) and [for Mac](https://github.com/sauraen/HQStreamer2/wiki/Download-and-run-instructions---Mac).

## General

HQStreamer2 is a cross-platform, cross-format plugin and server backend for streaming low-latency, lossless-quality audio between computers across the internet. It is perfect for livestreaming concerts, teaching classes on music or audio production topics, or enabling novel networked electronic performances. It can handle up to 128-channel audio at arbitrary sample rates (if you have enough network bandwidth!), and does not require any special technical expertise to configure (when streaming via an existing hqs2relay server).

## Compatibility

* Windows 7/8/10 x64: VST3, Standalone, Relay
* Mac OS 10.7 (Lion) - 10.14 (Mojave), and unofficially 10.15 (Catalina): VST3, AU, AUv3, Standalone, Relay
* Linux: Standalone, Relay

The standalone plugins run on their own without a DAW, and can be connected to any system audio I/O devices. "Relay" is the component of HQStreamer2 which runs on a server and routes audio from hosts to many clients each.

A VST2 version of HQStreamer2 can technically be built, but due to Steinberg licensing restrictions cannot be distributed.

Regarding Mac OS 10.15 (Catalina): As an experimental open-source program, HQStreamer2 is neither code-signed nor notarized by Apple. Apple does not officially support such software on Catalina, and may completely ban all such software on future versions of Mac OS. However, there is a workaround to permit individual programs to run on Catalina despite these restrictions.
* For standalone or relay: Right-click or Ctrl-click on the app and choose Open. In the security dialog box that pops up, choose Open.
* For the plugins: After you attempt to load the plugin into your DAW and receive the security warning, close your DAW. Go to the Apple menu  > System Preferences > Security & Privacy > General. There will be a notice about HQStreamer2 being unidentified. Click the Open Anyway button, then reopen your DAW and if necessary rescan for plugins.


## Manual Installation

On Mac, it's easier to use the InstallForSystem or InstallForUser scripts within HQStreamer2.dmg, instead of manually following the steps below.

### VST3

Move or copy HQStreamer2.vst3 into your system (or user) 64-bit VST3 folder. Create this folder if it doesn't exist.
* Windows: `C:\Program Files\Common Files\VST3`
* Mac OS system VST3 folder (for all users): `/Library/Audio/Plug-Ins/VST3`
* Mac OS user VST3 folder: `/Users/your_name/Library/Audio/Plug-Ins/VST3` (Note that the folder `/Users/your_name/Library` DOES already exist, it's hidden by default.)

If your DAW does not automatically detect HQStreamer2 on startup, tell it to rescan for new/modified plugins. If your DAW still does not detect the plugin on Windows, and you're sure your DAW supports VST3 plugins (Pro Tools and older DAWs will not), try installing [the Visual C++ Redistributable 2017 from Microsoft](https://aka.ms/vs/16/release/vc_redist.x64.exe).

### AU (Mac OS only)

Move or copy HQStreamer2.component into your system or user AU components folder. Some DAWs do not support the user AU components folder, so use the system one if possible.
* System (for all users): `/Library/Audio/Plug-Ins/Components`
* User: `/Users/your_name/Library/Audio/Plug-Ins/Components` (Note that the folder `/Users/your_name/Library` DOES already exist, it's hidden by default.)

If your DAW does not automatically detect HQStreamer2 on startup, tell it to rescan for new/modified plugins. It has been reported that some AU plugins are not detected until the computer is restarted, though we have not found this to be the case for HQStreamer2.

Note that HQStreamer2 is by nature not "sandbox safe" (it opens network connections). Apple requires AU plugins to be "sandbox safe" to be loaded in newer versions of GarageBand.

### AUv3 (Mac OS only)

The developer's host of choice (Reaper) does not support AUv3 plugins, so we have no idea if the AUv3 plugin we put up (HQStreamer2.appex) works or not.

Typically AUv3 plugins are installed in the same `Components` folder as AU plugins above.

### Standalone

Simply run the executable (HQStreamer2.exe on Windows or HQStreamer2.app on Mac), it can be in any folder.

### Relay

##### General info:

* Port 21568 must be open for bidirectional TCP.
* To set up the passphrase for hosting sessions, do ./hqs2relay --setpassphrase, then when prompted enter the new passphrase. It is stored hashed (not in plaintext) in HQStreamer2/hqs2relay.cfg in your user application data directory (`C:\Users\your_name\AppData\Roaming\`, `~/Library/`, or `~/.config/` for Windows/Mac/Linux respectively).
* You can also run `./hqs2relay -i` to run in interactive mode, where you can examine connected users and kick sessions. But this requires `screen` or some other mechanism to allow you to keep having input available, otherwise when you background the process it will grind to a halt (the actual routing too).

##### Linux build and run:

* `sudo apt install build-essential` (and `git` to obtain the repo in the first place)
* `git clone https://github.com/sauraen/HQStreamer2 && cd HQStreamer2` or otherwise obtain the repo
* `cd hqs2relay/Builds/LinuxMakefile && make`
* `cd build && cp ../../../Downloads/linux_x64/autorun.sh . && chmod a+x autorun.sh`
* `autorun.sh &`. When you need to stop this, do `top -u your_name`, then find the `bash` PID which is close in number to the `hqs2relay` PID, and then kill that process. Examine the logs routinely (no need to stop HQStreamer2) and let Sauraen know of any crash info.


## HQStreamer2 Usage

### Common (all plugin versions or standalone)

#### Module Chooser

At startup, HQStreamer2 gives you buttons for three choices:
* Host a session on a server: Send audio from your computer to a server running hqs2relay, so it can be distributed to multiple clients connecting from around the world.
* Host a session locally: Allow clients to connect directly to your computer and receive audio without an intermediate server. Requires port forwarding setup and lots of upload bandwidth.
* Join a session: Connect to someone else streaming with either of the methods above. Receive audio from them.

Once you select one of these options, the interface changes and provides module-specific controls. There is no way to go back to the module chooser, since this would be rarely needed in practice; simply exit and restart the program, or remove and add the plugin from your DAW track.

The controls which are common to all modules are:
* Status: The top label shows a live-updating status message about the plugin and network connections. If something isn't working right, pay attention to what this is saying.
* Volume slider: Sets the volume for sending or receiving audio to/from the network. The scale is in the equivalent of dBV (20 dB = 10x). It is initialized to -12 dB for clients and 0 dB for senders. Senders should always send audio as close to full-scale as possible without clipping, for maximum quality on the int16 or DPCM formats.
* Buffer bar: Shows the audio passing through HQStreamer2's internal buffer. This allows the user to visually confirm that audio is being sent or received. In addition, the color shows how "healthy" the buffer is, with green being best and red being worst. If the buffer overflows or underflows, it instantly resets itself to a healthy position again, but this of course causes a brief click or silence in the audio.

#### Audio Parameters

The sample rate and number of channels are determined by the sender. HQStreamer2 supports arbitrary sample rates and between 1 and 128 channels. Note that clients will refuse to output audio received in a format which is not compatible with their DAW's current state. Most commonly, this means clients will have to change their DAW's sample rate to match the sender's, and this is true for multichannel streams as well.

The sender choose the audio format, from the following three choices:
* float32: Standard 32-bit floating point, as used in most audio applications. This format is the only one that does not clip incoming audio: incoming data greater than 0 dB (+/- 1.0f) will be transmitted and received without distortion.
* int16: 16-bit signed integer audio, like used in CDs and many WAV files. 16-bit is perceptually lossless as long as the signal is close to full-scale, so the host should ensure the input is sufficiently loud. However, 16-bit is also clipped at 0 dB, so the host should ensure that the input does not exceed this. A warning status is shown if the input audio is being clipped.
* DPCM: A delta encoding of int16 audio that saves up to 50% of the bandwidth. It does so by representing the changes between samples as one byte if they are small and two bytes otherwise. It is possible to construct an audio signal which the DPCM encoding produces incorrect, distorted results for (e.g. full-scale non-antialiased square waves); however, this is very rare in practice.

Internally, there is also a fourth audio format:
* zeros: If all the samples in a packet are zeros (complete silence), the packet is instead transmitted as "zeros" format, and no actual sample data is transmitted. This reduces the bandwidth dramatically when the audio is silent, and the transition to and from this format is seamless, on a packet-by-packet basis.

#### Session Host

The controls for hosting a session on a server are:
* Server IP/URL: An address of the server running hqs2relay which your audio will be streamed through. This can be an IP address such as `123.234.56.178` or a hostname such as `servername.music.university.edu`.
* Host passprhase: hqs2relay requires a configurable password to host sessions, in an attempt to help reduce potential abuse. Find out this password from the person who set up your server.
* Session name/ID: A hqs2relay server may host any number of independent sessions at a time; this is the name of your session. It can also be used as a weak password for entry: if outsiders can't guess the session name, such as "Music 101 SP2020 c5c7fc6452e0", they can't join.
* Format: The host chooses one of the audio formats explained above.
* Connect/Disconnect: Self-explanatory.

#### Local Host

The controls for hosting a stream locally are:
* Session name/ID: Same as above.
* Format: Same as above.
* Start/Stop: Self-explanatory.

#### Client

The controls for joining a session are:
* Server IP/URL: Same as above. The host will give you the information to fill in here.
* Session name/ID: Same as above. The host will give you the information to fill in here.
* Connect/disconnect: Self-explanatory.
* Latency: The client gets to pick the latency of the connection. Lower latencies are of course preferable, but may be less stable. The risk is that the buffer would overflow or underflow and cause brief interruptions in the audio while it gets back on track. You should experiment with this setting for each server you connect to.

#### Other information

* Ping times are round-trip.


## Troubleshooting

#### The buffer is flashing colors and rapidly wiping across the screen, and I don't hear any audio!

First, check for an error message in the status at the top of the plugin. A common issue is a message saying that the host is sending audio at a different sample rate than your DAW is set to. In this case, change the sample rate in your DAW (or using Options > Audio/MIDI settings if this is standalone) and the problem should fix itself (no need to disconnect/restart the plugin).

If there's no error message (the status says "OK"), and you're a client:
* If the buffer is wiping from the middle to the right, this means your DAW has stopped processing the plugin (taking audio out of the buffer), while the network is still putting incoming audio into the buffer. Different DAWs have different policies about when to process plugins, but they may stop processing it if playback is stopped, if the track is not record armed or monitoring, or if the DAW has lost focus. Try fixing these issues and the audio should resume.
* If the buffer is wiping from the middle to the left, this means no new audio is being received from the network while the plugin is still being processed, but the network connection is still established. Typically this means the host's DAW is temporarily not processing the plugin, as described in the point above. You as a client can't do anything about this, other than telling the host to fix it on their end.

## TODOs

* Socket based live config for relay
* Test AUv3 version
* Store server IP, passphrase, and session name in DAW project
