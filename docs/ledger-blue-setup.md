# Setup For Ledger Blue
A more powerful set of command line interfaces. Ledger Blue is an optional set of python scripts that help you manage ledger devices

## Overview

- Install python virtual env
- Install App on Physical Device

## Python Virtual Env

Install `pipenv`
```
sudo apt install pip
pip install --user pipenv
```

Update you path
```
if [ -d /home/me/.local/bin ]; then export PATH=${PATH}:/home/me/.local/bin; fi
```

Make a place for your virtual environments, and run additional setup
```
mkdir -p ~/local/python/virtualenv
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python get-pip.py
```
Test virtual environment install
```
virtualenv --version
```
If this doesn't work you may need to run
```
pip install virtualenv
```

## Install on Physical Device

- Connect your Ledger device to your computer and unlock it.
- Enter your ledger app's directory. If needed build the application binary [see README](../README.md)
- Start a python vitural environment named `ledger`
```
virtualenv -p python3 ledger
```
- Activate & Install Ledger Blue
```
source ledger/bin/activate
pip3 install ledgerblue
```
- Enter container, everything below is inside the container
```
sudo docker run --rm -ti -v "/dev/bus/usb:/dev/bus/usb" -v "$(realpath .):/app" --privileged ledger-app-builder:latest
```
- Test command with `ledgerblue.checkGenuineRemote`
   - See [Target Ids](https://gist.github.com/TamtamHero/b7651ffe6f1e485e3886bf4aba673348)
   - Example below target id is nano s+ (plus)
   - Hardware buttons to accept command
   - See [README for udev rules](../README.md) required on linux
```
python -m ledgerblue.checkGenuineRemote --targetId 0x33000004
```
- Deploy binary `app.hex` to ledger device
   - target below is nano s+
   - hardware buttons to accept command
   - Note if you get error `680f`, delete the app and reload it.
```
python -m ledgerblue.loadApp --appFlags 0x240 --path "44'/194'" --curve secp256k1 --tlv --targetId 0x33100004 --targetVersion="1.0.4" --delete --fileName bin/app.hex --appName Eos --appVersion 1.4.3 --dataSize $((0x`cat debug/app.map |grep _envram_data | tr -s ' ' | cut -f2 -d' '|cut -f2 -d'x'` - 0x`cat debug/app.map |grep _nvram_data | tr -s ' ' | cut -f2 -d' '|cut -f2 -d'x'`)) `ICONHEX=\`python3 /opt/nanosplus-secure-sdk/icon3.py --hexbitmaponly nanox_app_eos.gif  2>/dev/null\` ; [ ! -z "$ICONHEX" ] && echo "--icon $ICONHEX"`
```
- [List of Commands for Ledger Blue](https://github.com/LedgerHQ/blue-loader-python/tree/master/ledgerblue)
- Exit Docker.
- Stop Vitural Environment, type `deactivate` in shell.


## Sources

- [Ledger Short Guide](https://www.ledger.com/a-short-guide-to-nano-s-firmware-1-2-features)
- [Python Virtual Envs](https://docs.python-guide.org/dev/virtualenvs/)
- [Ledger Blue Python Project](https://pypi.org/project/ledgerblue/)
- [Ledger Loading the Application](https://developers.ledger.com/docs/nano-app/load/)
- [Bolos Python Loader Specs](https://readthedocs.org/projects/bolos-python-loader/downloads/pdf/latest/)
