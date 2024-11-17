# ComputerCraft-Streaming-Radio

The goal of this project is to enable CC:Tweaked computers in Minecraft to play internet streaming radio stations via in-game speakers.

The project is comprised of two parts:

## **radioTranscodeServer.py**:

This multi-threaded Python daemon opens a WebSocket server that transcodes audio from a streaming radio station to DFPWM format for CC:Tweaked computers.
You must run this on a system that is accessible from the in-game computer.

Note: This script requires **ffmpeg 5.1 or later** with dfpwm support. Ubuntu packages are often out of date.
Additionally, ffmpeg has an issue with DNS name resolution (you will get segmentation faults when trying to use a streaming input). On Ubuntu 22.04 you'll need to install and start the package 'nscd' to fix this.
```
sudo apt-get install nscd
sudo systemctl start nscd.service
```

## **streamRadio.lua**:

This Lua script connects to the WebSocket server and plays the transcoded audio on the CC:Tweaked computer's in-game speakers.

Note: If you are running the daemon on the same network as the minecraft server you will need to alter the computercraft server config file `config/computercraft-server.toml`:
Search for the line **host = "$private"** and change it to **host = ""** to allow the computer to connect to the local transcoding server.
Warning: This is technically insecure and could allow users on your minecraft server to probe your local network. Better practice would be to use an external server such as AWS or Digital Ocean to host the radioTranscodeServer.py script.

This script will default to try to connect to the radioTranscodeServer.py on localhost:8765. If you want to use a different server, you can provide the IP and port as a second arguement:
```
streamRadio.lua https://ice4.somafm.com/groovesalad-128-mp3 1.1.1.1:8765
```

---

## Installation:

### On Transcoding Server:
1. Install Python 3
2. Install FFMPEG 5.1 or later
3. Install Name service caching daemon (nscd)
4. Clone this repository

### On Minecraft server (with CC:Tweaked):
1. Grab the streamRadio.lua script to a computer:
    - ```wget https://raw.githubusercontent.com/liquidthex/ComputerCraft-Streaming-Radio/refs/heads/main/streamRadio.lua streamRadio.lua```

---

## Usage:

1. Run the radioTranscodeServer.py script to start the WebSocket server.
2. Run the streamRadio.lua script on a CC:Tweaked computer and provide the stream (direct MP3) URL as an argument.

Example:
```
streamRadio.lua https://ice4.somafm.com/groovesalad-128-mp3
```

This should play the Groovesalad radio station on the CC:Tweaked computer's in-game speakers.

---

## Requirements:

### On Transcoding Server:
- FFMPEG 5.1 or later
- Python 3
- nscd Name service caching daemon
- Ability for minecraft server to reach TCP port 8765
### On Minecraft server:
- CC:Tweaked
- Websockets enabled
- Optionally: Tweak computercraft server config to enable local network access.
- Computer with speaker attached to any side

---

## Credits:
Designed by Liquidthex (https://liquidthex.com) and ChatGPT o1-preview

## License

This project is licensed under the MIT License. See the [LICENSE](./LICENSE) file for details.
