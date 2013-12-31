[]: {{{1

    File        : HOWTO.md
    Maintainer  : Felix C. Stegerman <flx@obfusk.net>
    Date        : 2013-12-31

    Copyright   : Copyright (C) 2013  Felix C. Stegerman

[]: }}}1

## GnuPG w/ OpenPGP Smart Card v2, Subkeys, SSH, and SHA512

[]: {{{1

  This is a short overview of the steps I took to create a new GPG
  key.  I've used subkeys and put them on an OpenPGP Smart Card v2; I
  made sure to use strong hashes; and I've used an authentication
  subkey for SSH.

  I will refer to the secure computer as `orks`, the daily computer as
  `daily`, and both as `both`.

  For more detailed tutorials and blog posts, see the Links.

  NB: I recommend using a secure computer to generate and manage keys
  on (e.g. Debian on a flash drive, used only for this purpose, not
  connected to a network, with full disk encryption, guarded by orks
  when not used).  Also: **MAKE BACKUPS !!!**.

  NB: Use `shred` or `wipe` instead of `rm`.

[]: }}}1

## orks

[]: {{{1

### 1. Packages

  `gnupg2`, `scdaemon`

### 2. Configuration

  * Configuration Files
  * udev?

### 3. Generate Key(s)

```
$ pkill gpg-agent

# generate 4096-bit RSA key (sign only) (expire after 5y ?!)
$ gpg2 --gen-key

# adduid (Nx), primary
$ gpg2 --edit $KEY

# addkey (signing)
$ gpg2 --edit $KEY

# addkey (encryption)
$ gpg2 --edit $KEY

# addkey (authentication)
$ gpg2 --expert --edit $KEY

# generate revocation certificate
$ gpg2 -a --gen-revoke $KEY > $KEY.revoke.asc

# get public authentication key for ssh
$ gpgkey2ssh $SUBKEY > $SUBKEY.pub
```

### 4. Export, Backup, Copy

```
$ gpg2 -a --export $KEY > $KEY.public.asc
$ gpg2 -a --export-secret-key $KEY > /some/encrypted/flash/drive/$KEY.private.asc

$ cp -a $KEY.public.asc $KEY.revoke.asc /some/encrypted/flash/drive/

$ cp -a ~/.gnupg /some/encrypted/flash/drive/gnupg-backup
$ cp -a ~/.gnupg ~/gnupg-COPY
```

### 5. Configure Card

```
$ gpg2 --card-status

# edit PIN, admin PIN, info; forcesig?
$ gpg2 --card-edit
```

### 6. Copy Keys to Card

```
# edit copy b/c keys will be moved to card !!!
# make sure to write down which subkey is which before toggling
# toggle; keytocard (3x)
$ gpg2 --homedir ~/gnupg-COPY --edit $KEY
```

[]: }}}1

## daily

[]: {{{1

### 1. Packages

  `gnupg2`, `scdaemon`, `pinentry-gtk2`

### 2. Configuration

  * Configuration Files
  * Disable autostart of SSH Agent and GPG Agent (e.g. in
    gnome-session-properties)
  * udev?

### 3. Import Public Key and Use Card

```
$ gpg2 --import < $KEY.asc

# trust
$ gpg2 --edit $KEY

$ gpg2 --card-status
```

### 4. Test !!!

  * encrypt/decrypt
  * sign/verify
  * w/ and w/o card

[]: }}}1

## Configuration Files

[]: {{{1

`gpg.conf`, `both`:

```
# default key server
keyserver hkp://keys.gnupg.net

# strong hashes and algorithms
personal-digest-preferences SHA512
cert-digest-algo SHA512
default-preference-list SHA512 SHA384 SHA256 SHA224 AES256 AES192 AES CAST5 ZLIB BZIP2 ZIP Uncompressed
```

`gpg.conf`, `daily`:

```
use-agent
```

`gpg-agent.conf`, `daily`:

```
enable-ssh-support
```

[]: }}}1

## udev

[]: {{{1

```
$ sudo addgroup --system scard
$ sudo adduser $YOU scard
$ sudo cp -i .../udev/openpgp-smartcard.rules /etc/udev/rules.d/
$ sudo service udev restart
```

[]: }}}1

## Links

[]: {{{1

  * https://wiki.debian.org/subkeys
  * https://wiki.debian.org/Smartcards/OpenPGP
  * https://wiki.fsfe.org/Card_howtos/Card_with_subkeys_using_backups
  * http://www.gnupg.org/howtos/card-howto/en/smartcard-howto.html

#

  * http://www.debian-administration.org/users/dkg/weblog/48
  * https://wiki.ubuntu.com/SecurityTeam/GPGMigration

#

  * http://dilfridge.blogspot.ru/2013/05/openpgp-smartcards-and-gentoo-part-2.html
  * http://ncommander.blogspot.nl/2009/08/so-after-having-my-trusty-sony-vaio-do.html
  * http://n22t.com/2011/03/20/Using-the-OpenPGP-Card-for-SSH-Authentication.html
  * https://alexcabal.com/creating-the-perfect-gpg-keypair/

[]: }}}1

## License

  Creative Commons Attribution-ShareAlike 3.0 Unported
  --- http://creativecommons.org/licenses/by-sa/3.0

[]: ! ( vim: set tw=70 sw=2 sts=2 et fdm=marker : )
