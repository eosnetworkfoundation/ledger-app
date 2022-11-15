# Testing for Ledger Hardware Wallet

## Overview

- Install python virtual env
- Install App on Physical Device
- Build Emulator Speculos

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
- Enter container
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
python -m ledgerblue.loadApp --targetId 0x33100004 --apdu --tlv --fileName bin/app.hex --appName Eos --appFlags 0x00
```
- [List of Commands for Ledger Blue](https://github.com/LedgerHQ/blue-loader-python/tree/master/ledgerblue)
- Stop Vitural Environment, type `deactivate` in shell.

## Emulator

- Get the [Source Code](https://github.com/LedgerHQ/speculos)
- [Follow Instructions](https://speculos.ledger.com/) to build
  - Must use with VNC option `cmake -Bbuild -H. -DWITH_VNC=1 && make -C build/`
- Look for `speculos.py`
- Test to validate install
  - `pip install pytest`
  - `python3 -m pytest -s -v tests/apps/`
- Validate by running with default BTC app
  - run in foreground
  - display text mode, otherwise default is QT
```
./speculos.py --display text ./apps/btc.elf
```
- You can go to `http://127.0.0.1:5000/` for another interface and more data




## Sources

- [Ledger Short Guide](https://www.ledger.com/a-short-guide-to-nano-s-firmware-1-2-features)
- [Python Virtual Envs](https://docs.python-guide.org/dev/virtualenvs/)
- [Ledger Blue Python Project](https://pypi.org/project/ledgerblue/)
- [Ledger Loading the Application](https://developers.ledger.com/docs/nano-app/load/)
- [Bolos Python Loader Specs](https://readthedocs.org/projects/bolos-python-loader/downloads/pdf/latest/)
