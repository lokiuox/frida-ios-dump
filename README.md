# frida-ios-dump
Pull a decrypted IPA from a jailbroken device.

This fork adds the following features:
- Fixed to work with Frida 17+
- Fixed to work over WiFi


## Setup
1. Install the [`frida`](http://www.frida.re/) and `openssh-server` packages on the target device
2. Create a virtual environment: `python3 -m venv ./venv`
3. Activate it: `source ./venv/bin/activate`
4. Install packages: `pip install -r requirements.txt --upgrade`

## Connect
To check if the device is detected, you can run `frida-ls-devices`:
```
Id                      Type    Name             OS
----------------------  ------  ---------------  --------------
local                   local   Local System     macOS 15.5
barebone                remote  GDB Remote Stub
socket                  remote  Local Socket
54eebb9a50a287349[...]  remote  iPad             iPhone OS 18.2 # <- This means it works
```
Otherwise, you can try to forward Frida's port via SSH:
```sh
ssh mobile@<TARGET_IP> -L 27042:127.0.0.1:27042 -L 2222:127.0.0.1:22
```
Note: if this works, it will appear as `socket`

## Usage
```sh
./dump.py -H <TARGET_IP> -p 22 <Display name|Bundle identifier>
```

### Sample log
```
./dump.py Aftenposten
Start the target app Aftenposten
Dumping Aftenposten to /var/folders/wn/9v1hs8ds6nv_xj7g95zxyl140000gn/T
start dump /var/containers/Bundle/Application/66423A80-0AFE-471C-BC9B-B571107D3C27/AftenpostenApp.app/AftenpostenApp
start dump /private/var/containers/Bundle/Application/66423A80-0AFE-471C-BC9B-B571107D3C27/AftenpostenApp.app/Frameworks/AFNetworking.framework/AFNetworking
start dump /private/var/containers/Bundle/Application/66423A80-0AFE-471C-BC9B-B571107D3C27/AftenpostenApp.app/Frameworks/ATInternet_iOS_ObjC_SDK.framework/ATInternet_iOS_ObjC_SDK
start dump /private/var/containers/Bundle/Application/66423A80-0AFE-471C-BC9B-B571107D3C27/AftenpostenApp.app/Frameworks/SPTEventCollector.framework/SPTEventCollector
start dump /private/var/containers/Bundle/Application/66423A80-0AFE-471C-BC9B-B571107D3C27/AftenpostenApp.app/Frameworks/SPiDSDK.framework/SPiDSDK
start dump /private/var/containers/Bundle/Application/66423A80-0AFE-471C-BC9B-B571107D3C27/AftenpostenApp.app/Frameworks/libswiftCore.dylib
start dump /private/var/containers/Bundle/Application/66423A80-0AFE-471C-BC9B-B571107D3C27/AftenpostenApp.app/Frameworks/libswiftCoreData.dylib
start dump /private/var/containers/Bundle/Application/66423A80-0AFE-471C-BC9B-B571107D3C27/AftenpostenApp.app/Frameworks/libswiftCoreGraphics.dylib
start dump /private/var/containers/Bundle/Application/66423A80-0AFE-471C-BC9B-B571107D3C27/AftenpostenApp.app/Frameworks/libswiftCoreImage.dylib
start dump /private/var/containers/Bundle/Application/66423A80-0AFE-471C-BC9B-B571107D3C27/AftenpostenApp.app/Frameworks/libswiftCoreLocation.dylib
start dump /private/var/containers/Bundle/Application/66423A80-0AFE-471C-BC9B-B571107D3C27/AftenpostenApp.app/Frameworks/libswiftDarwin.dylib
start dump /private/var/containers/Bundle/Application/66423A80-0AFE-471C-BC9B-B571107D3C27/AftenpostenApp.app/Frameworks/libswiftDispatch.dylib
start dump /private/var/containers/Bundle/Application/66423A80-0AFE-471C-BC9B-B571107D3C27/AftenpostenApp.app/Frameworks/libswiftFoundation.dylib
start dump /private/var/containers/Bundle/Application/66423A80-0AFE-471C-BC9B-B571107D3C27/AftenpostenApp.app/Frameworks/libswiftObjectiveC.dylib
start dump /private/var/containers/Bundle/Application/66423A80-0AFE-471C-BC9B-B571107D3C27/AftenpostenApp.app/Frameworks/libswiftQuartzCore.dylib
start dump /private/var/containers/Bundle/Application/66423A80-0AFE-471C-BC9B-B571107D3C27/AftenpostenApp.app/Frameworks/libswiftUIKit.dylib
Generating Aftenposten.ipa

Done.
```

Congratulations!!! You've got a decrypted IPA file.

Drag to [MonkeyDev](https://github.com/AloneMonkey/MonkeyDev), Happy hacking!

### Troubleshooting

### SSH root connection fails
On recent iOS versions you may need to reset root's password. The old password is `alpine`, you can set it again as the new one or set a different one and specify it in `./dump.py` with the `-P` flag.

### Other issues
If the following error occurs:

* causes device to reboot
* lost connection
* unexpected error while probing dyld of target process

please open the application before dumping.


## Help text
```
usage: dump.py [-h] [-l] [-o OUTPUT_IPA] [-H SSH_HOST] [-p SSH_PORT] [-u SSH_USER] [-P SSH_PASSWORD] [-K SSH_KEY_FILENAME] [target]

frida-ios-dump (by AloneMonkey v2.0)

positional arguments:
  target                Bundle identifier or display name of the target app

optional arguments:
  -h, --help            show this help message and exit
  -l, --list            List the installed apps
  -o OUTPUT_IPA, --output OUTPUT_IPA
                        Specify name of the decrypted IPA
  -H SSH_HOST, --host SSH_HOST
                        Specify SSH hostname
  -p SSH_PORT, --port SSH_PORT
                        Specify SSH port
  -u SSH_USER, --user SSH_USER
                        Specify SSH username
  -P SSH_PASSWORD, --password SSH_PASSWORD
                        Specify SSH password
  -K SSH_KEY_FILENAME, --key_filename SSH_KEY_FILENAME
                        Specify SSH private key file path
```

## Development
To change the `dump.js` script:
1. `npm install`
2. Change the script
3. `npm run build`


## Support
Python 2.x and 3.x
