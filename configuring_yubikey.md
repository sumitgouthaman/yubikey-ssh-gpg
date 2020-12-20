# Configuring the Yubikey.

## First we need the ykman tool

```sh
sudo apt-add-repository ppa:yubico/stable
sudo apt update
sudo apt install yubikey-manager
```

## Make sure Yubikey has CCID mode enabled.

Check which modes are enabled:

```sh
ykman list
```

If CCID mode is not enabled, enable using:

```
# Exact modes enabled depends on what you need.
ykman mode OTP+FIDO+CCID
```

## Change PIN or Admin PIN

The default pin for new Yubikeys is `123456` and default admin pin is `12345678`.

You can change this using the gpg command (`sudo apt-get install gnupg`).

See example below:

```sh
gpg --card-edit

Reader ...........: Yubico Yubikey ...
...
...

gpg/card> admin
Admin commands are allowed

gpg/card> passwd
gpg: OpenPGP card no. .... detected

1 - change PIN
2 - unblock PIN
3 - change Admin PIN
4 - set the Reset Code
Q - quit

Your selection? 1
PIN changed.

1 - change PIN
2 - unblock PIN
3 - change Admin PIN
4 - set the Reset Code
Q - quit

Your selection? 3
PIN changed.

1 - change PIN
2 - unblock PIN
3 - change Admin PIN
4 - set the Reset Code
Q - quit

Your selection? q

gpg/card> quit
```
