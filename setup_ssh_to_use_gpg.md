# Enable SSH to use GPG agent

## Setup gpg-agent

Make sure there is a file in `.gnupg/gpg-agent.conf` that includes the line `enable-ssh-support`.

If not:

```sh
echo "enable-ssh-support" > .gnupg/gpg-agent.conf
```

Get keygrips of your GPG keys using:

```sh
gpg -K --with-keygrip
```

Note the keygrips of the subkeys you want to use for SSH authentication.

Then add them to the `.gnupg/sshcontrol` file.

```sh
echo <keygrip> >> ~/.gnupg/sshcontrol
```

## Setup bashrc.

Add following lines to .bashrc file:

```
if [ -z "$SSH_CLIENT" ]
then
        export SSH_AUTH_SOCK=$(gpgconf --list-dirs agent-ssh-socket)
fi
gpgconf --launch gpg-agent
```

The purpose of `if [ -z "$SSH_CLIENT" ]` is to make sure that we use the forwarded SSH agent when SSH-ing from Machine 1 -> Machine 2 -> Machine 3. Else machine 2 will try to look for GPG key locally instead of one that was used to SSH from Machine 1 -> Machine 2.

## Get SSH public key

After reloading (`source ~/.bashrc`), use `ssh-add -L` to list SSH public keys provided via gpg-agent.

Note the key with `cardno:` at the end (do this with only 1 Yubikey connected at a time).
    - This line should start with `ssh-rsa`.
    - Copy it to the `.ssh/authorized_keys` file of other hosts that you want to SSH into.