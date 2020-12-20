# Creating and transferring keys

We use GPG to generate keys and transfer them to the Yubikey.

## Install GPG

```sh
sudo apt-get install gnupg
```

## Generate master key and authentication subkeys.

We will generate 1 GPG master key and multiple subkeys that can be moved to the Yubikey. After you move a subkey to the Yubikey, it's private part is destroyed and cannot be recovered.

Use below command and follow the prompts to generate a keypair of type "RSA and RSA". Key lenght might have to be restricted to 2048 if you plan to use a Yubikey.

```sh
gpg --full-generate-key
```

You can list the generated keys using:

```sh
gpg --list-keys         # Public keys
gpg --list-secret-keys  # Private keys
```

Note the **Key ID** (looks sort-of like a GUID).

```
KEY_ID=<The generated key id>
```

## Add a authentication sub-key

```
gpg --expert --edit-key $KEY_ID
```

This will open a `gpg>` prompt.

1. In that prompt, type `addkey`.
1. For type of key, select "(8) RSA (set your own capabilities)".
1. At some point, it will let you select capabilities (Sign/Encrypt/Authenticate).
    - Use the S/E/A keys to toggle capabiltities till you are only left with Authenticate in ON state.
1. Use keysize of 2048.
1. Follow rest of the prompts to create the subkey.

You should now be able to see the subkey when you `gpg --list-keys`.

## Transfer subkey to Yubikey

```sh
gpg --edit-key $KEY_ID
```

This opens the `gpg>` prompt.

1. Type `toggle`to switch to private key editor (this sometimes doesn't show any visible change).
2. Select the subkey you want by typing `key $INDEX`.
    - The keys are 0 indexed.
    - The selected subkey should have a asterisk (*).
    - Make sure the right subkey is selected.
3. Type `keytocard`.
    - Select `(3) Authentication key` when asked to choose the slot in the Yubikey.
    - Follow rest of the prompt.
    - This is a **destructive action** and the private part of the subkey can not be recovered after this.

To configure multiple Yubikeys you can create more authentication subkeys and transfer them to other Yubikeys.