[]: {{{1

    File        : HOWTO.md
    Maintainer  : Felix C. Stegerman <flx@obfusk.net>
    Date        : 2013-12-30

    Copyright   : Copyright (C) 2013  Felix C. Stegerman

[]: }}}1

## GnuPG w/ OpenPGP Smart Card v2, Subkeys, SSH, and SHA512

  This is a short overview of the steps I took to create a new GPG
  key.  I've used subkeys and put them on an OpenPGP Smart Card v2; I
  made sure to use strong hashes; and I've used an authentication
  subkey for SSH.  For more detailed tutorials and blog posts, see the
  Links.

## gpg.conf

```
personal-digest-preferences SHA512
cert-digest-algo SHA512
default-preference-list SHA512 SHA384 SHA256 SHA224 AES256 AES192 AES CAST5 ZLIB BZIP2 ZIP Uncompressed
```

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

[]: ! ( vim: set tw=0 sw=2 sts=2 et fdm=marker : )
