# Setup ChromeOS SSH extension

## Install the smart card connector

https://chrome.google.com/webstore/detail/smart-card-connector/khpfeaanjngmcnplbdlpegiifgpfgdco

## Install SSH extension

https://chrome.google.com/webstore/detail/secure-shell/iodihamcpbpeioajjeobimgagajmlibd

## Configure connection

In the connection dialog of SSH extention, use:

1. `--ssh-agent=gsc` in "SSH relay server options".
1. `-A` in "SSH Arguments".
    - This setups up SSH forwarding, so we can SSH from Machine 1 -> Machine 2 -> Machine 3.

## Configure SSH from Linux container (Crostini)

### First enable SSH in Crostini.

1. If the `/etc/ssh/sshd_not_to_be_run` file exists, rename it to something else.
    - Presense of this file blocks SSH into the container.
1. Add the GPG provided SSH public key to `~/.ssh/authorized_keys`.

### Use SSH extention to SSH into Crostini with SSH agent-forwarding

In the SSH extension new connection profile, use:
1. Name of your user inside the Crostini container as `user`.
1. `penguin.linux.test` as `host`.
1. `--ssh-agent=gsc` in "SSH relay server options".
1. `-A` in "SSH Arguments".
    - This setups up SSH forwarding, so we can SSH from Machine 1 -> Machine 2 -> Machine 3.

Now you can ssh from Crostini into other machines and use the same credentials from Yubikey.

## Reference
1. https://blog.merzlabs.com/posts/yubikey-crostini/