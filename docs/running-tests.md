# Running Tests

## Summary

Only works with an actual devices. Tests need a USB dongle to work. The test are run from within the docker container hosting the image `ledger-app-builder:latest`.

## Install

Look at the steps [Install on Physical Device](testing-setup.md)

1. Connect You Device to your computer
2. Start up docker container
```
sudo docker run --rm -ti -v "/dev/bus/usb:/dev/bus/usb" -v "$(realpath .):/app" --privileged ledger-app-builder:latest
```
3. Build your App
```
make clean
# all make cms with BOLOS_SDK=$NANOX_SDK for nano-x or BOLOS_SDK=$NANOSP_SDK for nano-s-plus
make delete # approve on device
make build  
make load # approve on device
```
4. Install python libaries
```
pip install base58
pip install asn1
```
4. Unlock Device
5. Open Eos App
7. Run get Pub Key test
```
python3 getPublicKey.py
```
6. Run Tests
```
cd test
# follow prompts on device
for i in *.json ; do echo "$i" ; python3 signTransaction.py --file "$i" ; sleep 1 ; done
```
7. Look through ouput for errors. `transaction_unknown.json` is expected to error.

## Errors and resolutions

- Error 0X6E01 : App is not open. Open the app before running tests
- Invalid status 5515 : Device Locked. Unlock device before running tests
