# app-eos

Eos wallet application framework for Ledger Nano S

This follows the specification available in the doc/ folder

To use the generic wallet refer to `signTransaction.py`, `getPublicKey.py` or Ledger EOS Wallet application available on Github at https://github.com/tarassh/fairy-wallet

# How to Install developer version
## Configuring Ledger Environment

* Install Docker on your machines
* Checkout the app builder repository

```
git clone https://github.com/LedgerHQ/ledger-app-builder.git
cd ledger-app-builder
sudo docker build -t ledger-app-builder:latest .
```

This will take a few minutes to install

## Compile your ledger app

* go to your instance directory:
* copy over the files and enter the docker container
```
cd /User/me/my-app/
sudo docker run --rm -ti -v "/Users/me/my-app:/app" ledger-app-builder:latest
bash-5.1# make clean
bash-5.1# make
```

## Clang Analyzer

```
sudo docker run --rm -ti -v "$(realpath .):/app" ledger-app-builder:latest
bash-5.1# make scan-build
```

## Ledger Variants

For Nano X, specify the BOLOS_SDK environment variable before building your app:

```
sudo docker run --rm -ti -v "$(realpath .):/app" ledger-app-builder:latest
bash-5.1# make clean
bash-5.1# BOLOS_SDK=$NANOX_SDK make
```

For Nano S+, specify the BOLOS_SDK environment variable before building your app:

```
sudo docker run --rm -ti -v "$(realpath .):/app" ledger-app-builder:latest
bash-5.1# make clean
bash-5.1# BOLOS_SDK=$NANOSP_SDK make
```

Instructions taken from [Ledger HQ App Builder Readme](https://raw.githubusercontent.com/LedgerHQ/ledger-app-builder/master/README.md) with modification.
