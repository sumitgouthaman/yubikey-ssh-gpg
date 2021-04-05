# New computer as SSH Client

On new computers, the SSH client won't know that it should use the Yubikey. Here's how to make it work.

## On old computer

1. Copy `.gnupg/gpg-agent.conf` and `.gnupg/sshcontrol` to a USB drive.

2. Export private / public keys from gpupg:

    ```sh
    gpg -a --export > /my/usb/drive/my_public_keys.asc
    gpg -a --export-secret-keys > /my/usb/drive/my_private_keys.asc
    ```

## On new computer

1. Copy `.gnupg/gpg-agent.conf` and `.gnupg/sshcontrol` to the new computer's ~/.gnupg folder.

2. Import the private / public keys:

    ```sh
    gpg --import /my/usb/drive/my_public_keys.asc
    gpg --import /my/usb/drive/my_private_keys.asc
    ```

3. Add following to end of `~/.bashrc` file:

    ```sh
    if [ -z "$SSH_CLIENT" ]
    then
        export SSH_AUTH_SOCK=$(gpgconf --list-dirs agent-ssh-socket)
    fi
    gpgconf --launch gpg-agent
    ```

4. Reboot

## Test

Connect Yubikey and test by running:

```sh
gpg -K --with-keygrip
ssh-add -L
```

Then try connecting with other SSH servers.