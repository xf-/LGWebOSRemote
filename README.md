# LGWebOSRemote
Command line webOS remote for LGTVs. This tool uses a connection via websockets to port 3000 on newer LG TVs, there are other tools which use a restful connection to port 8080 however that port is closed on newer firmware versions.

## Supported models

### Tested with

  * 43LM6300PSB
  * 43UN73003LC
  * 60UJ6300-UA
  * HU80KG.AEU (CineBeam 4K)
  * OLED48A2
  * OLED55B7
  * OLED55C9
  * OLED55CX5LB
  * OLED55CXAUA
  * OLED65B9PUA
  * OLED77CX9LA
  * OLED77GX
  * OLED48C1 (ssl)
  * OLED42C2 (ssl)
  * OLED48C2 (ssl)
  * SK8500PLA
  * SM9010PLA
  * UF776V
  * UF830V
  * UH650V
  * UJ6309
  * UJ635V
  * UJ6570
  * UJ701V
  * [please add more!]

Tested with python 2.7 on mac/linux and works fine, your mileage may vary with windows, patches welcome.
Tested with python 3.9 on Debian Unstable.
Tested with python 3.10 on Windows 10/11
Tested with 3.10 on WSL (Ubuntu 20.04)

### Likely supports

All devices with firmware major version 4, product name "webOSTV 2.0"

## Available Commands
	lgtv scan
	lgtv auth <host> MyTV
	lgtv --name MyTV --ssl audioStatus
	lgtv --name MyTV --ssl audioVolume
	lgtv --name MyTV --ssl closeAlert <alertId>
	lgtv --name MyTV --ssl closeApp <appid>
	lgtv --name MyTV --ssl createAlert <message> <button>
	lgtv --name MyTV --ssl execute <command>
	lgtv --name MyTV --ssl getCursorSocket
	lgtv --name MyTV --ssl getForegroundAppInfo
	lgtv --name MyTV --ssl getPictureSettings
	lgtv --name MyTV --ssl getPowerState
	lgtv --name MyTV --ssl getSoundOutput
	lgtv --name MyTV --ssl getSystemInfo
	lgtv --name MyTV --ssl getTVChannel
	lgtv --name MyTV --ssl input3DOff
	lgtv --name MyTV --ssl input3DOn
	lgtv --name MyTV --ssl inputChannelDown
	lgtv --name MyTV --ssl inputChannelUp
	lgtv --name MyTV --ssl inputMediaFastForward
	lgtv --name MyTV --ssl inputMediaPause
	lgtv --name MyTV --ssl inputMediaPlay
	lgtv --name MyTV --ssl inputMediaRewind
	lgtv --name MyTV --ssl inputMediaStop
	lgtv --name MyTV --ssl listApps
	lgtv --name MyTV --ssl listLaunchPoints
	lgtv --name MyTV --ssl listChannels
	lgtv --name MyTV --ssl listInputs
	lgtv --name MyTV --ssl listServices
	lgtv --name MyTV --ssl mute <true|false>
	lgtv --name MyTV --ssl notification <message>
	lgtv --name MyTV --ssl notificationWithIcon <message> <url>
	lgtv --name MyTV --ssl off
	lgtv --name MyTV --ssl on
	lgtv --name MyTV --ssl openAppWithPayload <payload>
	lgtv --name MyTV --ssl openBrowserAt <url>
	lgtv --name MyTV --ssl openYoutubeId <videoid>
	lgtv --name MyTV --ssl openYoutubeURL <url>
	lgtv --name MyTV --ssl openYoutubeLegacyId <videoid>
	lgtv --name MyTV --ssl openYoutubeLegacyURL <url>
	lgtv --name MyTV --ssl serialise
	lgtv --name MyTV --ssl setInput <input_id>
	lgtv --name MyTV --ssl setSoundOutput <tv_speaker|external_optical|external_arc|external_speaker|lineout|headphone|tv_external_speaker|tv_speaker_headphone|bt_soundbar>
	lgtv --name MyTV --ssl screenOff
	lgtv --name MyTV --ssl screenOn
	lgtv --name MyTV --ssl setTVChannel <channelId>
	lgtv --name MyTV --ssl setVolume <level>
	lgtv --name MyTV --ssl startApp <appid>
	lgtv --name MyTV --ssl swInfo
	lgtv --name MyTV --ssl volumeDown
	lgtv --name MyTV --ssl volumeUp

## Install

Requires wakeonlan, websocket for python (python3-websocket for python3), and getmac.
python-pip (python3-pip for python3) and git are required for the installation process.

    python -m venv lgtv-venv
    source lgtv-venv/bin/activate
    pip install git+https://github.com/klattimer/LGWebOSRemote

To install it system wide:

	sudo mkdir -p /opt
	sudo python -m venv /opt/lgtv-venv
	source /opt/lgtv-venv/bin/activate
	sudo pip install git+https://github.com/klattimer/LGWebOSRemote

## Example usage
    # Scan/Authenticate
    $ lgtv scan
    {
        "count": 1,
        "list": [
            {
                "address": "192.168.1.31",
                "model": "UF830V",
                "uuid": "10f34f86-0664-f223-4b8f-d16a772d9baf"
            }
        ],
        "result": "ok"
    }
    $ lgtv auth 192.168.1.31 MyTV
    # At this point the TV will request pairing, follow the instructions on screen

    # Commands are basically
    #$ lgtv --name TVNAME --ssl COMMAND COMMAND_ARGS

    $ lgtv --name MyTV --ssl on
    $ lgtv --name MyTV --ssl off

    # If you have the youtube plugin
    $ lgtv --name MyTV --ssl openYoutubeURL https://www.youtube.com/watch?v=dQw4w9WgXcQ

    # Otherwise, this works reasonably well
    $ lgtv --name MyTV --ssl openBrowserAt https://www.youtube.com/tv#/watch?v=dQw4w9WgXcQ

## SSL

Starting 25th of January 2023 LG has deprecated insecure ws connections, ssl is now required. Because of this, should you wish to use it on newer firmware devices you can append the argument "ssl" at the back. It connects to 3001 with wss. 
### Example
```
$ lgtv auth 192.168.1.31 MyTV
$ lgtv --name MyTV --ssl off
$ lgtv --name MyTV --ssl screenOff
```

## Caveats

You need to auth with the TV before being able to use the on command as it requires the mac address.

## TODO

Implement the following features:

	closeToast
	getSystemSettings

## Bugs

I couldn't test youtube because it seems the app isn't installed and not available to download right now
maybe they're updating it?
