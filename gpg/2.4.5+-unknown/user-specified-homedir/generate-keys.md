# Generate keys on homedir and keyring

Keyrings are just normal files for better key segregation.

## Sample sessions

#### generate initial primary key

```shell
$ gpg --homedir=base-keys/ --no-default-keyring --keyring=base-keys/base-keyring --expert --full-gen-key
```
```terminaloutput
gpg (GnuPG) 2.4.5-unknown; Copyright (C) 2024 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

gpg: keybox 'base-keys/base-keyring' created
Please select what kind of key you want:
   (1) RSA and RSA
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
   (9) ECC (sign and encrypt) *default*
  (10) ECC (sign only)
  (11) ECC (set your own capabilities)
  (13) Existing key
  (14) Existing key from card
Your selection? 11

Possible actions for this ECC key: Sign Certify Authenticate
Current allowed actions: Sign Certify

   (S) Toggle the sign capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? S

Possible actions for this ECC key: Sign Certify Authenticate
Current allowed actions: Certify

   (S) Toggle the sign capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? Q
Please select which elliptic curve you want:
   (1) Curve 25519 *default*
   (2) Curve 448
   (3) NIST P-256
   (4) NIST P-384
   (5) NIST P-521
   (6) Brainpool P-256
   (7) Brainpool P-384
   (8) Brainpool P-512
   (9) secp256k1
Your selection?
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 0
Key does not expire at all
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: ss-1
Email address: ss-1@ss.ss
Comment:
You selected this USER-ID:
    "ss-1 <ss-1@ss.ss>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? o
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: /d/TestTmpDir/gpg/gpg-subkey-secrets-exports/base-keys/trustdb.gpg: trustdb created
gpg: directory '/d/TestTmpDir/gpg/gpg-subkey-secrets-exports/base-keys/openpgp-revocs.d' created
gpg: revocation certificate stored as '/d/TestTmpDir/gpg/gpg-subkey-secrets-exports/base-keys/openpgp-revocs.d/50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1.rev'
public and secret key created and signed.

pub   ed25519 2025-08-04 [C]
      50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1
uid                      ss-1 <ss-1@ss.ss>
```

The key `50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1` is primary public key.

> Note: It is just my preference to keep the primary key's authority till certification and not any other operation, like:
>   - sign
>   - encrypt
>   - authenticate
> 
> As I believe in minimalism and segmentation on the control of authority. 

#### List keys on the primary key

```shell
$ gpg --homedir=base-keys/ --no-default-keyring --keyring=base-keys/base-keyring --list-keys
```

```terminaloutput
gpg: checking the trustdb
gpg: marginals needed: 3  completes needed: 1  trust model: pgp
gpg: depth: 0  valid:   1  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 1u
base-keys/base-keyring
----------------------
pub   ed25519 2025-08-04 [C]
      50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1
uid           [ultimate] ss-1 <ss-1@ss.ss>
```

#### trust the primary key

This step is optional, but, trusting the key is an indication for others to know that you own a certain key.

```shell
$ gpg --homedir=base-keys/ --no-default-keyring --keyring=base-keys/base-keyring --edit-key 50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1
```

```terminaloutput
gpg (GnuPG) 2.4.5-unknown; Copyright (C) 2024 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret key is available.

sec  ed25519/101DA1D42E4FECA1
     created: 2025-08-04  expires: never       usage: C
     trust: ultimate      validity: ultimate
[ultimate] (1). ss-1 <ss-1@ss.ss>

gpg>

sec  ed25519/101DA1D42E4FECA1
     created: 2025-08-04  expires: never       usage: C
     trust: ultimate      validity: ultimate
[ultimate] (1). ss-1 <ss-1@ss.ss>

gpg> trust
sec  ed25519/101DA1D42E4FECA1
     created: 2025-08-04  expires: never       usage: C
     trust: ultimate      validity: ultimate
[ultimate] (1). ss-1 <ss-1@ss.ss>

Please decide how far you trust this user to correctly verify other users' keys
(by looking at passports, checking fingerprints from different sources, etc.)

  1 = I don't know or won't say
  2 = I do NOT trust
  3 = I trust marginally
  4 = I trust fully
  5 = I trust ultimately
  m = back to the main menu

Your decision? 5
Do you really want to set this key to ultimate trust? (y/N) y

sec  ed25519/101DA1D42E4FECA1
     created: 2025-08-04  expires: never       usage: C
     trust: ultimate      validity: ultimate
[ultimate] (1). ss-1 <ss-1@ss.ss>
```

#### Generate a signing subkey

Continuing on the philosophy or minimal and segmented authority, it is beneficial to have subkeys for each operation.

This one is for signing.

```shell
$ gpg --homedir=base-keys/ --no-default-keyring --keyring=base-keys/base-keyring --edit-key 50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1
```

```terminaloutput
gpg (GnuPG) 2.4.5-unknown; Copyright (C) 2024 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret key is available.

sec  ed25519/101DA1D42E4FECA1
     created: 2025-08-04  expires: never       usage: C
     trust: ultimate      validity: ultimate
[ultimate] (1). ss-1 <ss-1@ss.ss>

gpg> addkey
Please select what kind of key you want:
   (3) DSA (sign only)
   (4) RSA (sign only)
   (5) Elgamal (encrypt only)
   (6) RSA (encrypt only)
  (10) ECC (sign only)
  (12) ECC (encrypt only)
  (14) Existing key from card
Your selection? 10
Please select which elliptic curve you want:
   (1) Curve 25519 *default*
   (4) NIST P-384
   (6) Brainpool P-256
Your selection?
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 2m
Key expires at Fri Oct  3 17:54:27 2025 IST
Is this correct? (y/N) y
Really create? (y/N) y
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.

sec  ed25519/101DA1D42E4FECA1
     created: 2025-08-04  expires: never       usage: C
     trust: ultimate      validity: ultimate
ssb  ed25519/53AC884CDF5DFACD
     created: 2025-08-04  expires: 2025-10-03  usage: S
[ultimate] (1)  ss-1 <ss-1@ss.ss>
```

#### Add another user id

Helpful to maintain multiple identities on the same key (primary key: 50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1), can also be done on the subkey (53AC884CDF5DFACD) if secret is known.

```shell
$ gpg --homedir=base-keys/ --no-default-keyring --keyring=base-keys/base-keyring --edit-key 50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1
```

```terminaloutput
gpg (GnuPG) 2.4.5-unknown; Copyright (C) 2024 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret key is available.

sec  ed25519/101DA1D42E4FECA1
     created: 2025-08-04  expires: never       usage: C
     trust: ultimate      validity: ultimate
[ultimate] (1). ss-1 <ss-1@ss.ss>

gpg> adduid
Real name: ss-2
Email address: ss-2@ss.ss
Comment:
You selected this USER-ID:
    "ss-2 <ss-2@ss.ss>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? o

sec  ed25519/101DA1D42E4FECA1
     created: 2025-08-04  expires: never       usage: C
     trust: ultimate      validity: ultimate
[ultimate] (1)  ss-1 <ss-1@ss.ss>
[ unknown] (2). ss-2 <ss-2@ss.ss>
```

#### Generate an encrypt capability subkey

Encrypt capability subkey.

```shell
$ gpg --homedir=base-keys/ --no-default-keyring --keyring=base-keys/base-keyring --edit-key 50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1
```

```terminaloutput
gpg (GnuPG) 2.4.5-unknown; Copyright (C) 2024 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret key is available.

sec  ed25519/101DA1D42E4FECA1
     created: 2025-08-04  expires: never       usage: C
     trust: ultimate      validity: ultimate
ssb  ed25519/53AC884CDF5DFACD
     created: 2025-08-04  expires: 2025-10-03  usage: S
[ultimate] (1)  ss-1 <ss-1@ss.ss>
[ unknown] (2). ss-2 <ss-2@ss.ss>

gpg> addkey
Please select what kind of key you want:
   (3) DSA (sign only)
   (4) RSA (sign only)
   (5) Elgamal (encrypt only)
   (6) RSA (encrypt only)
  (10) ECC (sign only)
  (12) ECC (encrypt only)
  (14) Existing key from card
Your selection? 12
Please select which elliptic curve you want:
   (1) Curve 25519 *default*
   (4) NIST P-384
   (6) Brainpool P-256
Your selection?
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 2m
Key expires at Fri Oct  3 17:54:49 2025 IST
Is this correct? (y/N) y
Really create? (y/N) y
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.

sec  ed25519/101DA1D42E4FECA1
     created: 2025-08-04  expires: never       usage: C
     trust: ultimate      validity: ultimate
ssb  ed25519/53AC884CDF5DFACD
     created: 2025-08-04  expires: 2025-10-03  usage: S
ssb  cv25519/F25884FA53937E77
     created: 2025-08-04  expires: 2025-10-03  usage: E
[ultimate] (1)  ss-1 <ss-1@ss.ss>
[ unknown] (2). ss-2 <ss-2@ss.ss>
```

Key `F25884FA53937E77` now only has the encrypt capability.

#### Keys only persist in their own homedir and keyrings

Querying key details from their own keyring shows them:

```shell
$ gpg --homedir=base-keys/ --no-default-keyring --keyring=base-keys/base-keyring --expert --list-key F25884FA53937E77
```

```terminaloutput
pub   ed25519 2025-08-04 [C]
      50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1
uid           [ultimate] ss-2 <ss-2@ss.ss>
uid           [ultimate] ss-1 <ss-1@ss.ss>
sub   ed25519 2025-08-04 [S] [expires: 2025-10-03]
sub   cv25519 2025-08-04 [E] [expires: 2025-10-03]
```

Specific subkey queries (using the ! mark) shows the full key:

```shell
$ gpg --homedir=base-keys/ --no-default-keyring --keyring=base-keys/base-keyring --expert --list-key F25884FA53937E77!
```

```terminaloutput
pub   ed25519 2025-08-04 [C]
      50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1
uid           [ultimate] ss-2 <ss-2@ss.ss>
uid           [ultimate] ss-1 <ss-1@ss.ss>
sub   ed25519 2025-08-04 [S] [expires: 2025-10-03]
sub   cv25519 2025-08-04 [E] [expires: 2025-10-03]
```

##### Using no keyring identification, when the key was based inside a keyring, errs as no public key found

The primary key (50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1) and hence, all its subkeys persist in keyring=base-keys/base-keyring.

###### Querying these without specifying the keyring raises not found error:

```shell
$ gpg --homedir=base-keys/ --no-default-keyring --expert --list-key F25884FA53937E77!
```

<code style="color : red">
gpg: error reading key: No public key
</code>

###### Since `--no-default-keyring` was used on key creation hence the default keyring also doesn't have the key:

```shell
$ gpg --homedir=base-keys/ --expert --list-key F25884FA53937E77!
```

<code style="color : red">
gpg: error reading key: No public key
</code>
