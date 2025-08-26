# Keyring 

Check how gpg stores keys in keyrings.

## Tests

All tests to be done in a gpg homedir so as to not pollute the real homedir.

```shell
$ export GNUPGHOME=/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/
$ gpg --list-keys
```

- [Basic key behavior in gpg keyrings](#key-storage).
- [Key included in Multiple Keyrings](#keys-in-multiple-keyrings).
- [Creating a keyring](#creating-keyrings).

### Key Storage

Check how keys are stored and operated in the gpg keyrings.

#### Generate keys

##### Generate key in keyring

Generate key in keyring and **do not include** the `--no-default-keyring` switch, so as to include the key in both, the keyring as well as the default keyring.

```shell
$ gpg --keyring keyring1 --full-gen-key
```
```terminaloutput
gpg (GnuPG) 2.4.5-unknown; Copyright (C) 2024 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

gpg: keyblock resource '/d/TestTmpDir/gpg/gpg-keyring-test/keyring1': No such file or directory
Please select what kind of key you want:
   (1) RSA and RSA
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (9) ECC (sign and encrypt) *default*
  (10) ECC (sign only)
  (14) Existing key from card
Your selection? 
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
Key is valid for? (0)
Key does not expire at all
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: ss1
Email address: ss1@keyring1-home.ss
Comment: key included in the keyring1 keyring and also the default keyring
You selected this USER-ID:
    "ss1 (key included in the keyring1 keyring and also the default keyring) <ss1@keyring1-home.ss>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? o
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: directory '/d/TestTmpDir/gpg/gpg-keyring-test/openpgp-revocs.d' created
gpg: revocation certificate stored as '/d/TestTmpDir/gpg/gpg-keyring-test/openpgp-revocs.d/BBA35D0B2EFC94BEDFB483561FE583FDBC0670A2.rev'
public and secret key created and signed.

pub   ed25519 2025-08-15 [SC]
      BBA35D0B2EFC94BEDFB483561FE583FDBC0670A2
uid                      ss1 (key included in the keyring1 keyring and also the default keyring) <ss1@keyring1-home.ss>
sub   cv25519 2025-08-15 [E]

```

> Referring to the line
>
> gpg: keyblock resource '/d/TestTmpDir/gpg/gpg-keyring-test/keyring1': No such file or directory
> 
> `keyring1` wasn't actually created and hence key `BBA35D0B2EFC94BEDFB483561FE583FDBC0670A2` only belongs to the default keyring.

##### Generate key in keyring without including in default keyring

Generate key in keyring and **include** the `--no-default-keyring` switch, so as to include the key in only the keyring but exclude it from the default keyring.

```shell
$ gpg --keyring keyring1 --no-default-keyring --full-gen-key
```
```terminaloutput
gpg (GnuPG) 2.4.5-unknown; Copyright (C) 2024 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

gpg: keybox '/d/TestTmpDir/gpg/gpg-keyring-test/keyring1' created
Please select what kind of key you want:
   (1) RSA and RSA
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (9) ECC (sign and encrypt) *default*
  (10) ECC (sign only)
  (14) Existing key from card
Your selection?
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
Key is valid for? (0)
Key does not expire at all
Is this correct? (y/N)
Key is valid for? (0) 
Key does not expire at all
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: ss1
Email address: ss1@keyring1.no-home
Comment: key included in the keyring1 keyring but not the default keyring
You selected this USER-ID:
    "ss1 (key included in the keyring1 keyring but not the default keyring) <ss1@keyring1.no-home>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? o
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: revocation certificate stored as '/d/TestTmpDir/gpg/gpg-keyring-test/openpgp-revocs.d/F32B1F56055B4F0C6D01E312B2CC556843AC8FFE.rev'
public and secret key created and signed.

pub   ed25519 2025-08-16 [SC]
      F32B1F56055B4F0C6D01E312B2CC556843AC8FFE
uid                      ss1 (key included in the keyring1 keyring but not the default keyring) <ss1@keyring1.no-home>
sub   cv25519 2025-08-16 [E]

```

##### Generate key in just the default keyring

Generate key and do not mention any keyring also **do not include** the `--no-default-keyring` switch.

```shell
$ gpg --full-gen-key
```
```terminaloutput
$ gpg --full-gen-key
gpg (GnuPG) 2.4.5-unknown; Copyright (C) 2024 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Please select what kind of key you want:
   (1) RSA and RSA
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (9) ECC (sign and encrypt) *default*
  (10) ECC (sign only)
  (14) Existing key from card
Your selection?
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
Key is valid for? (0)
Key does not expire at all
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: ss1
Email address: ss1@home
Comment: key only in default keyring
You selected this USER-ID:
    "ss1 (key only in default keyring) <ss1@home>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? o
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: revocation certificate stored as '/d/TestTmpDir/gpg/gpg-keyring-test/openpgp-revocs.d/465034F5989D81C4E73331D85B3058066AB061B0.rev'
public and secret key created and signed.

pub   ed25519 2025-08-16 [SC]
      465034F5989D81C4E73331D85B3058066AB061B0
uid                      ss1 (key only in default keyring) <ss1@home>
sub   cv25519 2025-08-16 [E]

```

##### Generate key with no keyring and no default keyring as well

Generate key and do not mention any keyring also **include** the `--no-default-keyring` switch.

```shell
$ gpg --no-default-keyring --full-gen-key
```
```terminaloutput
gpg (GnuPG) 2.4.5-unknown; Copyright (C) 2024 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Please select what kind of key you want:
   (1) RSA and RSA
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (9) ECC (sign and encrypt) *default*
  (10) ECC (sign only)
  (14) Existing key from card
Your selection?
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
Key is valid for? (0)
Key does not expire at all
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: ss1
Email address: ss1@no-keyring.no-home
Comment: key not included in the keyring1 keyring and not included in the default keyring
You selected this USER-ID:
    "ss1 (key not included in the keyring1 keyring and not included in the default keyring) <ss1@no-keyring.no-home>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? o
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: revocation certificate stored as '/d/TestTmpDir/gpg/gpg-keyring-test/openpgp-revocs.d/A8C986652CB37391FA0F8742DC92CCF248179623.rev'
public and secret key created and signed.

pub   ed25519 2025-08-16 [SC]
      A8C986652CB37391FA0F8742DC92CCF248179623
uid                      ss1 (key not included in the keyring1 keyring and not included in the default keyring) <ss1@no-keyring.no-home>
sub   cv25519 2025-08-16 [E]

```

#### Key summary

<details>
<summary>
key in keyring1 and the default keyring: BBA35D0B2EFC94BEDFB483561FE583FDBC0670A2
</summary>

- without any switch
    ```shell
    $ gpg --list-keys BBA35D0B2EFC94BEDFB483561FE583FDBC0670A2
    ```
    ```terminaloutput
    pub   ed25519 2025-08-15 [SC]
        BBA35D0B2EFC94BEDFB483561FE583FDBC0670A2
    uid           [ultimate] ss1 (key included in the keyring1 keyring and also the default keyring) <ss1@keyring1-home.ss>
    sub   cv25519 2025-08-15 [E]

    ```

- with `keyring1`
    ```shell
    $ gpg --list-keys --keyring=keyring1 BBA35D0B2EFC94BEDFB483561FE583FDBC0670A2
    ```
    ```terminaloutput
    pub   ed25519 2025-08-15 [SC]
        BBA35D0B2EFC94BEDFB483561FE583FDBC0670A2
    uid           [ultimate] ss1 (key included in the keyring1 keyring and also the default keyring) <ss1@keyring1-home.ss>
    sub   cv25519 2025-08-15 [E]

    ```

- with `keyring1` and `--no-default-keyring` switch
    ```shell
    $ gpg --keyring=keyring1 --no-default-keyring --list-keys BBA35D0B2EFC94BEDFB483561FE583FDBC0670A2
    ```
    ```terminaloutput
    gpg: error reading key: No public key
    ```
    exit code: 2

> Now, this is behaving exactly as it should, as key `BBA35D0B2EFC94BEDFB483561FE583FDBC0670A2` the key gene command for this key didn't actually proceed to create the `keyring1` keyring.

</details>

<details>
<summary>
key in keyring1 but not in the default keyring: F32B1F56055B4F0C6D01E312B2CC556843AC8FFE
</summary>

- without any switch
    ```shell
    $ gpg --list-keys F32B1F56055B4F0C6D01E312B2CC556843AC8FFE
    ```
    ```terminaloutput
    gpg: error reading key: No public key
    ```
    exit code 2

- with `--no-default-keyring` switch
    ```shell
    $ gpg --no-default-keyring --list-keys F32B1F56055B4F0C6D01E312B2CC556843AC8FFE
    ```
    ```terminaloutput
    gpg: error reading key: No public key
    ```
    exit code 2

    <div class="terminaloutput" style="background:#111; color:#eee; padding:0.5em; font-family:monospace; border-radius:6px">
    <span style="color:red"> STDERR: a line </span><br>
    <span> STDOUT: another line </span><br>
    </div>



</details>

<details>
<summary>
key in default keyring but not keyring1: 465034F5989D81C4E73331D85B3058066AB061B0
</summary>

some more more details.
</details>

<details>
<summary>
key in no keyring and no default keyring as well: A8C986652CB37391FA0F8742DC92CCF248179623
</summary>

some some more more details.
</details>

<details>
<summary>
Querying keys without any switch
</summary>

```shell
$ gpg --list-keys --keyid-format=long
```
```terminaloutput
/d/TestTmpDir/gpg/gpg-keyring-test/pubring.kbx
----------------------------------------------
pub   ed25519/1FE583FDBC0670A2 2025-08-15 [SC]
      BBA35D0B2EFC94BEDFB483561FE583FDBC0670A2
uid                 [ultimate] ss1 (key included in the keyring1 keyring and also the default keyring) <ss1@keyring1-home.ss>
sub   cv25519/EA3577D08A2771BF 2025-08-15 [E]

pub   ed25519/5B3058066AB061B0 2025-08-16 [SC]
      465034F5989D81C4E73331D85B3058066AB061B0
uid                 [ultimate] ss1 (key only in default keyring) <ss1@home>
sub   cv25519/BFB13FB2698AE2C3 2025-08-16 [E]

pub   ed25519/DC92CCF248179623 2025-08-16 [SC]
      A8C986652CB37391FA0F8742DC92CCF248179623
uid                 [ultimate] ss1 (key not included in the keyring1 keyring and not included in the default keyring) <ss1@no-keyring.no-home>
sub   cv25519/23E7DA898D7C23C3 2025-08-16 [E]

```

</details>

<details>
<summary>
List all the keys in keyring1 with no `--no-default-keyring` switch
</summary>

```shell
$ gpg --list-keys --keyid-format=long --keyring=keyring1
```
```terminaloutput
/d/TestTmpDir/gpg/gpg-keyring-test/pubring.kbx
----------------------------------------------
pub   ed25519/1FE583FDBC0670A2 2025-08-15 [SC]
      BBA35D0B2EFC94BEDFB483561FE583FDBC0670A2
uid                 [ultimate] ss1 (key included in the keyring1 keyring and also the default keyring) <ss1@keyring1-home.ss>
sub   cv25519/EA3577D08A2771BF 2025-08-15 [E]

pub   ed25519/5B3058066AB061B0 2025-08-16 [SC]
      465034F5989D81C4E73331D85B3058066AB061B0
uid                 [ultimate] ss1 (key only in default keyring) <ss1@home>
sub   cv25519/BFB13FB2698AE2C3 2025-08-16 [E]

pub   ed25519/DC92CCF248179623 2025-08-16 [SC]
      A8C986652CB37391FA0F8742DC92CCF248179623
uid                 [ultimate] ss1 (key not included in the keyring1 keyring and not included in the default keyring) <ss1@no-keyring.no-home>
sub   cv25519/23E7DA898D7C23C3 2025-08-16 [E]

/d/TestTmpDir/gpg/gpg-keyring-test/keyring1
-------------------------------------------
pub   ed25519/B2CC556843AC8FFE 2025-08-16 [SC]
      F32B1F56055B4F0C6D01E312B2CC556843AC8FFE
uid                 [ unknown] ss1 (key included in the keyring1 keyring but not the default keyring) <ss1@keyring1.no-home>
sub   cv25519/3E27EA41BA19ACE6 2025-08-16 [E]

```

</details>

<details>
<summary>
List all the keys in keyring1 with `--no-default-keyring` switch
</summary>

```shell
$ gpg --list-keys --keyid-format=long --keyring=keyring1 --no-default-keyring
```
```terminaloutput
/d/TestTmpDir/gpg/gpg-keyring-test/keyring1
-------------------------------------------
pub   ed25519/B2CC556843AC8FFE 2025-08-16 [SC]
      F32B1F56055B4F0C6D01E312B2CC556843AC8FFE
uid                 [ unknown] ss1 (key included in the keyring1 keyring but not the default keyring) <ss1@keyring1.no-home>
sub   cv25519/3E27EA41BA19ACE6 2025-08-16 [E]

```

</details>

#### Conclusions

- `--no-default-keyring` switch only works when a keyring is already specified. For eg. `A8C986652CB37391FA0F8742DC92CCF248179623` although specified to not be included in `keyring1` as well as not in the default keyring, actually ended-up showing in the default keyring.

### Keys in multiple Keyrings

See key behavior when it is included in multiple keyrings.

#### Key included in multiple keyrings at the time of generation

##### Generate keys

First generate key which is already included in two keyrings.

```shell
$ gso gpg --keyring=keyring1 --keyring=keyring2 --no-default-keyring --full-gen-key
```
<pre>
gpg (GnuPG) 2.4.5-unknown; Copyright (C) 2024 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

gpg: keyblock resource '/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/keyring2': No such file or directory
Please select what kind of key you want:
   (1) RSA and RSA
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (9) ECC (sign and encrypt) *default*
  (10) ECC (sign only)
  (14) Existing key from card
Your selection?
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
Key is valid for? (0)
Key does not expire at all
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: ss-keyring1-keyring2
Email address: ss-k1-k2@no-home
Comment: key in keyring1 and keyring2 but not in default keyring
You selected this USER-ID:
    "ss-keyring1-keyring2 (key in keyring1 and keyring2 but not in default keyring) <ss-k1-k2@no-home>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? o
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: revocation certificate stored as '/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/openpgp-revocs.d/67B6CEE060BF7AFE6001E424B104CD6A97F1E4C0.rev'
public and secret key created and signed.

<span style="color:green">pub   ed25519 2025-08-25 [SC]</span>
<span style="color:green">      67B6CEE060BF7AFE6001E424B104CD6A97F1E4C0</span>
<span style="color:green">uid                      ss-keyring1-keyring2 (key in keyring1 and keyring2 but not in default keyring) <ss-k1-k2@no-home></span>
<span style="color:green">sub   cv25519 2025-08-25 [E]</span>
<span style="color:green"></span>
<span style="color:red">[GSO]</span> exit code: 2
</pre>


##### List keys

<details>
<summary>67B6CEE060BF7AFE6001E424B104CD6A97F1E4C0 key is in keyring1 as well as keyring2 but not in default keyring</summary>

###### Check only applying `keyring1` filter

```shell
$ gso gpg --keyring=keyring1 --list-keys
```
<pre>
gpg: checking the trustdb
gpg: marginals needed: 3  completes needed: 1  trust model: pgp
gpg: depth: 0  valid:   5  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 5u
<span style="color:green">/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/pubring.kbx</span>
<span style="color:green">-----------------------------------------------------</span>
<span style="color:green">pub   ed25519 2025-08-15 [SC]</span>
<span style="color:green">      BBA35D0B2EFC94BEDFB483561FE583FDBC0670A2</span>
<span style="color:green">uid           [ultimate] ss1 (key included in the keyring1 keyring and also the default keyring) <ss1@keyring1-home.ss></span>
<span style="color:green">sub   cv25519 2025-08-15 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      465034F5989D81C4E73331D85B3058066AB061B0</span>
<span style="color:green">uid           [ultimate] ss1 (key only in default keyring) <ss1@home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      A8C986652CB37391FA0F8742DC92CCF248179623</span>
<span style="color:green">uid           [ultimate] ss1 (key not included in the keyring1 keyring and not included in the default keyring) <ss1@no-keyring.no-home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:green">/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/keyring1</span>
<span style="color:green">--------------------------------------------------</span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      F32B1F56055B4F0C6D01E312B2CC556843AC8FFE</span>
<span style="color:green">uid           [ultimate] ss1 (key included in the keyring1 keyring but not the default keyring) <ss1@keyring1.no-home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-25 [SC]</span>
<span style="color:green">      67B6CEE060BF7AFE6001E424B104CD6A97F1E4C0</span>
<span style="color:green">uid           [ultimate] ss-keyring1-keyring2 (key in keyring1 and keyring2 but not in default keyring) <ss-k1-k2@no-home></span>
<span style="color:green">sub   cv25519 2025-08-25 [E]</span>
<span style="color:green"></span>
<span style="color:red">[GSO]</span> exit code: 0
</pre>

###### Check only applying `keyring2` filter

```shell
$ gso gpg --keyring=keyring2 --list-keys
```
<pre>
gpg: keyblock resource '/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/keyring2': No such file or directory
<span style="color:green">/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/pubring.kbx</span>
<span style="color:green">-----------------------------------------------------</span>
<span style="color:green">pub   ed25519 2025-08-15 [SC]</span>
<span style="color:green">      BBA35D0B2EFC94BEDFB483561FE583FDBC0670A2</span>
<span style="color:green">uid           [ultimate] ss1 (key included in the keyring1 keyring and also the default keyring) <ss1@keyring1-home.ss></span>
<span style="color:green">sub   cv25519 2025-08-15 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      465034F5989D81C4E73331D85B3058066AB061B0</span>
<span style="color:green">uid           [ultimate] ss1 (key only in default keyring) <ss1@home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      A8C986652CB37391FA0F8742DC92CCF248179623</span>
<span style="color:green">uid           [ultimate] ss1 (key not included in the keyring1 keyring and not included in the default keyring) <ss1@no-keyring.no-home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:red">[GSO]</span> exit code: 2
</pre>

</details>

##### Conclusion

`keyring2` wasn't created at the time of key generation and hence key is only included in `keyring1`. But since `keyring1` was already present hence, this begs the question whether `keyring2` will be created at the time of key generation if it were not already created earlier and it were the only keyring mentioned.

#### Check if keyring is generated at the time of key generation

##### Generate keys

Generate key in `keyring2` (which is not already present) and check if `keyring2` gets created in this process.

```shell
$ gso gpg --keyring=keyring2 --no-default-keyring --full-gen-key --verbose
```
<pre>
gpg (GnuPG) 2.4.5-unknown; Copyright (C) 2024 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

gpg: enabled compatibility flags:
gpg: keybox '/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/keyring2' created
Please select what kind of key you want:
   (1) RSA and RSA
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (9) ECC (sign and encrypt) *default*
  (10) ECC (sign only)
  (14) Existing key from card
Your selection?
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
Key is valid for? (0)
Key does not expire at all
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: ss-keyring2
Email address: ss-k2@no-home
Comment: key in keyring2 and no default keyring. Keyring2 not yet created
You selected this USER-ID:
    "ss-keyring2 (key in keyring2 and no default keyring. Keyring2 not yet created) <ss-k2@no-home>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? o
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: pinentry launched (1517 w32 1.3.1-unknown /dev/cons2 xterm-256color needs-to-be-defined 20600/197609/197609 197609/197609 0)
gpg: pinentry launched (1518 w32 1.3.1-unknown /dev/cons2 xterm-256color needs-to-be-defined 20600/197609/197609 197609/197609 0)
gpg: pinentry launched (1519 w32 1.3.1-unknown /dev/cons2 xterm-256color needs-to-be-defined 20600/197609/197609 197609/197609 0)
gpg: writing self signature
gpg: EDDSA/SHA512 signature from: "6AC07DE1DF471CCC [?]"
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: writing key binding signature
gpg: EDDSA/SHA512 signature from: "6AC07DE1DF471CCC [?]"
gpg: writing public key to '/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/keyring2'
gpg: using pgp trust model
gpg: writing to '/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/openpgp-revocs.d/3D0C46A6CA69E511C1DEF2036AC07DE1DF471CCC.rev'
gpg: EDDSA/SHA512 signature from: "6AC07DE1DF471CCC ss-keyring2 (key in keyring2 and no default keyring. Keyring2 not yet created) <ss-k2@no-home>"
gpg: revocation certificate stored as '/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/openpgp-revocs.d/3D0C46A6CA69E511C1DEF2036AC07DE1DF471CCC.rev'
public and secret key created and signed.

<span style="color:green">pub   ed25519 2025-08-25 [SC]</span>
<span style="color:green">      3D0C46A6CA69E511C1DEF2036AC07DE1DF471CCC</span>
<span style="color:green">uid                      ss-keyring2 (key in keyring2 and no default keyring. Keyring2 not yet created) <ss-k2@no-home></span>
<span style="color:green">sub   cv25519 2025-08-25 [E]</span>
<span style="color:green"></span>
<span style="color:red">[GSO]</span> exit code: 0
</pre>

##### Conclusion

`keyring2` got generated in the process of key geenration as it was not already present.

#### Check if key can be included in multiple keyrings if all the keyrings are present already

`keyring1` and `keyring2` already present from previous tests.

##### Generate keys

```shell
$ gso gpg --keyring=keyring1 --keyring=keyring2 --no-default-keyring --full-gen-key
```
<pre>
gpg (GnuPG) 2.4.5-unknown; Copyright (C) 2024 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Please select what kind of key you want:
   (1) RSA and RSA
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (9) ECC (sign and encrypt) *default*
  (10) ECC (sign only)
  (14) Existing key from card
Your selection?
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
Key is valid for? (0)
Key does not expire at all
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: ss-k1-k2
Email address: ss-k1-k2@gen.no-home
Comment: keyring1+keyring2-noDefault both keyrings already present
You selected this USER-ID:
    "ss-k1-k2 (keyring1+keyring2-noDefault both keyrings already present) <ss-k1-k2@gen.no-home>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? o
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: revocation certificate stored as '/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/openpgp-revocs.d/C9A3A14816B4FB7074DBC08520BCE80213976CCE.rev'
public and secret key created and signed.

<span style="color:green">pub   ed25519 2025-08-25 [SC]</span>
<span style="color:green">      C9A3A14816B4FB7074DBC08520BCE80213976CCE</span>
<span style="color:green">uid                      ss-k1-k2 (keyring1+keyring2-noDefault both keyrings already present) <ss-k1-k2@gen.no-home></span>
<span style="color:green">sub   cv25519 2025-08-25 [E]</span>
<span style="color:green"></span>
<span style="color:red">[GSO]</span> exit code: 0
</pre>

##### List keys

###### All in `keyring1`

```shell
$ gso gpg --keyring=keyring1 --list-keys
```
<pre>
<span style="color:green">/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/pubring.kbx</span>
<span style="color:green">-----------------------------------------------------</span>
<span style="color:green">pub   ed25519 2025-08-15 [SC]</span>
<span style="color:green">      BBA35D0B2EFC94BEDFB483561FE583FDBC0670A2</span>
<span style="color:green">uid           [ unknown] ss1 (key included in the keyring1 keyring and also the default keyring) <ss1@keyring1-home.ss></span>
<span style="color:green">sub   cv25519 2025-08-15 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      465034F5989D81C4E73331D85B3058066AB061B0</span>
<span style="color:green">uid           [ unknown] ss1 (key only in default keyring) <ss1@home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      A8C986652CB37391FA0F8742DC92CCF248179623</span>
<span style="color:green">uid           [ unknown] ss1 (key not included in the keyring1 keyring and not included in the default keyring) <ss1@no-keyring.no-home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:green">/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/keyring1</span>
<span style="color:green">--------------------------------------------------</span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      F32B1F56055B4F0C6D01E312B2CC556843AC8FFE</span>
<span style="color:green">uid           [ultimate] ss1 (key included in the keyring1 keyring but not the default keyring) <ss1@keyring1.no-home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-25 [SC]</span>
<span style="color:green">      67B6CEE060BF7AFE6001E424B104CD6A97F1E4C0</span>
<span style="color:green">uid           [ultimate] ss-keyring1-keyring2 (key in keyring1 and keyring2 but not in default keyring) <ss-k1-k2@no-home></span>
<span style="color:green">sub   cv25519 2025-08-25 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-25 [SC]</span>
<span style="color:green">      C9A3A14816B4FB7074DBC08520BCE80213976CCE</span>
<span style="color:green">uid           [ultimate] ss-k1-k2 (keyring1+keyring2-noDefault both keyrings already present) <ss-k1-k2@gen.no-home></span>
<span style="color:green">sub   cv25519 2025-08-25 [E]</span>
<span style="color:green"></span>
<span style="color:red">[GSO]</span> exit code: 0
</pre>

> `C9A3A14816B4FB7074DBC08520BCE80213976CCE` present in keyring1

###### All in `keyring1` and `--no-default-keyring`

```shell
$ gso gpg --keyring=keyring1 --no-default-keyring --list-keys
```
<pre>
gpg: checking the trustdb
gpg: public key of ultimately trusted key 6AC07DE1DF471CCC not found
gpg: public key of ultimately trusted key DC92CCF248179623 not found
gpg: public key of ultimately trusted key 5B3058066AB061B0 not found
gpg: public key of ultimately trusted key 1FE583FDBC0670A2 not found
gpg: marginals needed: 3  completes needed: 1  trust model: pgp
gpg: depth: 0  valid:   7  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 7u
<span style="color:green">/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/keyring1</span>
<span style="color:green">--------------------------------------------------</span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      F32B1F56055B4F0C6D01E312B2CC556843AC8FFE</span>
<span style="color:green">uid           [ultimate] ss1 (key included in the keyring1 keyring but not the default keyring) <ss1@keyring1.no-home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-25 [SC]</span>
<span style="color:green">      67B6CEE060BF7AFE6001E424B104CD6A97F1E4C0</span>
<span style="color:green">uid           [ultimate] ss-keyring1-keyring2 (key in keyring1 and keyring2 but not in default keyring) <ss-k1-k2@no-home></span>
<span style="color:green">sub   cv25519 2025-08-25 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-25 [SC]</span>
<span style="color:green">      C9A3A14816B4FB7074DBC08520BCE80213976CCE</span>
<span style="color:green">uid           [ultimate] ss-k1-k2 (keyring1+keyring2-noDefault both keyrings already present) <ss-k1-k2@gen.no-home></span>
<span style="color:green">sub   cv25519 2025-08-25 [E]</span>
<span style="color:green"></span>
<span style="color:red">[GSO]</span> exit code: 2
</pre>

> `C9A3A14816B4FB7074DBC08520BCE80213976CCE` present in keyring1 + no-default-keyring


###### All in `keyring2`

```shell
$ gso gpg --keyring=keyring2 --list-keys
```
<pre>
<span style="color:green">/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/pubring.kbx</span>
<span style="color:green">-----------------------------------------------------</span>
<span style="color:green">pub   ed25519 2025-08-15 [SC]</span>
<span style="color:green">      BBA35D0B2EFC94BEDFB483561FE583FDBC0670A2</span>
<span style="color:green">uid           [ unknown] ss1 (key included in the keyring1 keyring and also the default keyring) <ss1@keyring1-home.ss></span>
<span style="color:green">sub   cv25519 2025-08-15 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      465034F5989D81C4E73331D85B3058066AB061B0</span>
<span style="color:green">uid           [ unknown] ss1 (key only in default keyring) <ss1@home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      A8C986652CB37391FA0F8742DC92CCF248179623</span>
<span style="color:green">uid           [ unknown] ss1 (key not included in the keyring1 keyring and not included in the default keyring) <ss1@no-keyring.no-home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:green">/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/keyring2</span>
<span style="color:green">--------------------------------------------------</span>
<span style="color:green">pub   ed25519 2025-08-25 [SC]</span>
<span style="color:green">      3D0C46A6CA69E511C1DEF2036AC07DE1DF471CCC</span>
<span style="color:green">uid           [ unknown] ss-keyring2 (key in keyring2 and no default keyring. Keyring2 not yet created) <ss-k2@no-home></span>
<span style="color:green">sub   cv25519 2025-08-25 [E]</span>
<span style="color:green"></span>
<span style="color:red">[GSO]</span> exit code: 0
</pre>

> `C9A3A14816B4FB7074DBC08520BCE80213976CCE` not present in `keyring2`.

###### All in `keyring2` and `--no-default-keyring`

```shell
$ gso gpg --keyring=keyring2 --no-default-keyring --list-keys
```

<pre>
<span style="color:green">/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/keyring2</span>
<span style="color:green">--------------------------------------------------</span>
<span style="color:green">pub   ed25519 2025-08-25 [SC]</span>
<span style="color:green">      3D0C46A6CA69E511C1DEF2036AC07DE1DF471CCC</span>
<span style="color:green">uid           [ unknown] ss-keyring2 (key in keyring2 and no default keyring. Keyring2 not yet created) <ss-k2@no-home></span>
<span style="color:green">sub   cv25519 2025-08-25 [E]</span>
<span style="color:green"></span>
<span style="color:red">[GSO]</span> exit code: 0
</pre>

> `C9A3A14816B4FB7074DBC08520BCE80213976CCE` not present in `keyring2`

###### All in `keyring1` and `keyring2` and `--no-default-keyring`

```shell
$ gso gpg --keyring=keyring2 --keyring=keyring1 --no-default-keyring --list-keys
```

<pre>
<span style="color:green">/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/keyring2</span>
<span style="color:green">--------------------------------------------------</span>
<span style="color:green">pub   ed25519 2025-08-25 [SC]</span>
<span style="color:green">      3D0C46A6CA69E511C1DEF2036AC07DE1DF471CCC</span>
<span style="color:green">uid           [ unknown] ss-keyring2 (key in keyring2 and no default keyring. Keyring2 not yet created) <ss-k2@no-home></span>
<span style="color:green">sub   cv25519 2025-08-25 [E]</span>
<span style="color:green"></span>
<span style="color:green">/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/keyring1</span>
<span style="color:green">--------------------------------------------------</span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      F32B1F56055B4F0C6D01E312B2CC556843AC8FFE</span>
<span style="color:green">uid           [ultimate] ss1 (key included in the keyring1 keyring but not the default keyring) <ss1@keyring1.no-home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-25 [SC]</span>
<span style="color:green">      67B6CEE060BF7AFE6001E424B104CD6A97F1E4C0</span>
<span style="color:green">uid           [ultimate] ss-keyring1-keyring2 (key in keyring1 and keyring2 but not in default keyring) <ss-k1-k2@no-home></span>
<span style="color:green">sub   cv25519 2025-08-25 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-25 [SC]</span>
<span style="color:green">      C9A3A14816B4FB7074DBC08520BCE80213976CCE</span>
<span style="color:green">uid           [ultimate] ss-k1-k2 (keyring1+keyring2-noDefault both keyrings already present) <ss-k1-k2@gen.no-home></span>
<span style="color:green">sub   cv25519 2025-08-25 [E]</span>
<span style="color:green"></span>
<span style="color:red">[GSO]</span> exit code: 0
</pre>

###### All in `keyring1` and `keyring2`

```shell
$ gso gpg --keyring=keyring2 --keyring=keyring1 --list-keys
```

<pre>
<span style="color:green">/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/pubring.kbx</span>
<span style="color:green">-----------------------------------------------------</span>
<span style="color:green">pub   ed25519 2025-08-15 [SC]</span>
<span style="color:green">      BBA35D0B2EFC94BEDFB483561FE583FDBC0670A2</span>
<span style="color:green">uid           [ unknown] ss1 (key included in the keyring1 keyring and also the default keyring) <ss1@keyring1-home.ss></span>
<span style="color:green">sub   cv25519 2025-08-15 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      465034F5989D81C4E73331D85B3058066AB061B0</span>
<span style="color:green">uid           [ unknown] ss1 (key only in default keyring) <ss1@home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      A8C986652CB37391FA0F8742DC92CCF248179623</span>
<span style="color:green">uid           [ unknown] ss1 (key not included in the keyring1 keyring and not included in the default keyring) <ss1@no-keyring.no-home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:green">/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/keyring2</span>
<span style="color:green">--------------------------------------------------</span>
<span style="color:green">pub   ed25519 2025-08-25 [SC]</span>
<span style="color:green">      3D0C46A6CA69E511C1DEF2036AC07DE1DF471CCC</span>
<span style="color:green">uid           [ unknown] ss-keyring2 (key in keyring2 and no default keyring. Keyring2 not yet created) <ss-k2@no-home></span>
<span style="color:green">sub   cv25519 2025-08-25 [E]</span>
<span style="color:green"></span>
<span style="color:green">/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/keyring1</span>
<span style="color:green">--------------------------------------------------</span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      F32B1F56055B4F0C6D01E312B2CC556843AC8FFE</span>
<span style="color:green">uid           [ultimate] ss1 (key included in the keyring1 keyring but not the default keyring) <ss1@keyring1.no-home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-25 [SC]</span>
<span style="color:green">      67B6CEE060BF7AFE6001E424B104CD6A97F1E4C0</span>
<span style="color:green">uid           [ultimate] ss-keyring1-keyring2 (key in keyring1 and keyring2 but not in default keyring) <ss-k1-k2@no-home></span>
<span style="color:green">sub   cv25519 2025-08-25 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-25 [SC]</span>
<span style="color:green">      C9A3A14816B4FB7074DBC08520BCE80213976CCE</span>
<span style="color:green">uid           [ultimate] ss-k1-k2 (keyring1+keyring2-noDefault both keyrings already present) <ss-k1-k2@gen.no-home></span>
<span style="color:green">sub   cv25519 2025-08-25 [E]</span>
<span style="color:green"></span>
<span style="color:red">[GSO]</span> exit code: 0
</pre>

> `C9A3A14816B4FB7074DBC08520BCE80213976CCE` only present in `keyring1`

#### Export key from one keyring and import into another

##### List keys

key `3D0C46A6CA69E511C1DEF2036AC07DE1DF471CCC` only included in `keyring2` and not the default keyring.

###### with `--no-default-keyring` switch

```shell
$ gso gpg --keyring=keyring2 --no-default-keyring --list-keys 3D0C46A6CA69E511C1DEF2036AC07DE1DF471CCC
```

<pre>
<span style="color:green">pub   ed25519 2025-08-25 [SC]</span>
<span style="color:green">      3D0C46A6CA69E511C1DEF2036AC07DE1DF471CCC</span>
<span style="color:green">uid           [ unknown] ss-keyring2 (key in keyring2 and no default keyring. Keyring2 not yet created) <ss-k2@no-home></span>
<span style="color:green">sub   cv25519 2025-08-25 [E]</span>
<span style="color:green"></span>
<span style="color:red">[GSO]</span> exit code: 0
</pre>

###### without the `--no-default-keyring` switch

```shell
$ gso gpg --keyring=keyring2 --list-keys 3D0C46A6CA69E511C1DEF2036AC07DE1DF471CCC
```

<pre>
<span style="color:green">pub   ed25519 2025-08-25 [SC]</span>
<span style="color:green">      3D0C46A6CA69E511C1DEF2036AC07DE1DF471CCC</span>
<span style="color:green">uid           [ unknown] ss-keyring2 (key in keyring2 and no default keyring. Keyring2 not yet created) <ss-k2@no-home></span>
<span style="color:green">sub   cv25519 2025-08-25 [E]</span>
<span style="color:green"></span>
<span style="color:red">[GSO]</span> exit code: 0
</pre>

##### Export `3D0C46A6CA69E511C1DEF2036AC07DE1DF471CCC`

###### without the `--no-default-keyring` switch

```shell
$ gso gpg --keyring=keyring2 --armor --export 3D0C46A6CA69E511C1DEF2036AC07DE1DF471CCC
```

<pre>
<span style="color:green">-----BEGIN PGP PUBLIC KEY BLOCK-----</span>
<span style="color:green"></span>
<span style="color:green">mDMEaKyDhRYJKwYBBAHaRw8BAQdAzmeOKmbzCAJu3ly2w43Tr5mpCrWsNAhYoYJv</span>
<span style="color:green">ljKx67G0XnNzLWtleXJpbmcyIChrZXkgaW4ga2V5cmluZzIgYW5kIG5vIGRlZmF1</span>
<span style="color:green">bHQga2V5cmluZy4gS2V5cmluZzIgbm90IHlldCBjcmVhdGVkKSA8c3MtazJAbm8t</span>
<span style="color:green">aG9tZT6IkwQTFgoAOxYhBD0MRqbKaeURwd7yA2rAfeHfRxzMBQJorIOFAhsDBQsJ</span>
<span style="color:green">CAcCAiICBhUKCQgLAgQWAgMBAh4HAheAAAoJEGrAfeHfRxzM8cIBAJoCTUsP6cPy</span>
<span style="color:green">qFzzuAH8IvCJeV/15ljhYNvSxpBJMyDMAP4ij8c+I+FXgSJlIbxH3XqcNDK6eJPx</span>
<span style="color:green">4ZTux3YflWwfAbg4BGisg4USCisGAQQBl1UBBQEBB0B6YlrVk2uu5y2BCqgDq3KI</span>
<span style="color:green">grHtcLzuLfLenp8CyNEAfAMBCAeIeAQYFgoAIBYhBD0MRqbKaeURwd7yA2rAfeHf</span>
<span style="color:green">RxzMBQJorIOFAhsMAAoJEGrAfeHfRxzMKP4BALk+UI2s+Bb+gyfaTckuqE018pfB</span>
<span style="color:green">XIaafz/eK9+dmy69AQDBXgpsZPffL/CEgMkf0nEUiZMxl8UBr/4MPeAGo4SpDQ==</span>
<span style="color:green">=QfNE</span>
<span style="color:green">-----END PGP PUBLIC KEY BLOCK-----</span>
<span style="color:red">[GSO]</span> exit code: 0
</pre>

###### with the `--no-default-keyring` switch

```shell
$ gso gpg --keyring=keyring2 --no-default-keyring --armor --export 3D0C46A6CA69E511C1DEF2036AC07DE1DF471CCC
```

<pre>
<span style="color:green">-----BEGIN PGP PUBLIC KEY BLOCK-----</span>
<span style="color:green"></span>
<span style="color:green">mDMEaKyDhRYJKwYBBAHaRw8BAQdAzmeOKmbzCAJu3ly2w43Tr5mpCrWsNAhYoYJv</span>
<span style="color:green">ljKx67G0XnNzLWtleXJpbmcyIChrZXkgaW4ga2V5cmluZzIgYW5kIG5vIGRlZmF1</span>
<span style="color:green">bHQga2V5cmluZy4gS2V5cmluZzIgbm90IHlldCBjcmVhdGVkKSA8c3MtazJAbm8t</span>
<span style="color:green">aG9tZT6IkwQTFgoAOxYhBD0MRqbKaeURwd7yA2rAfeHfRxzMBQJorIOFAhsDBQsJ</span>
<span style="color:green">CAcCAiICBhUKCQgLAgQWAgMBAh4HAheAAAoJEGrAfeHfRxzM8cIBAJoCTUsP6cPy</span>
<span style="color:green">qFzzuAH8IvCJeV/15ljhYNvSxpBJMyDMAP4ij8c+I+FXgSJlIbxH3XqcNDK6eJPx</span>
<span style="color:green">4ZTux3YflWwfAbg4BGisg4USCisGAQQBl1UBBQEBB0B6YlrVk2uu5y2BCqgDq3KI</span>
<span style="color:green">grHtcLzuLfLenp8CyNEAfAMBCAeIeAQYFgoAIBYhBD0MRqbKaeURwd7yA2rAfeHf</span>
<span style="color:green">RxzMBQJorIOFAhsMAAoJEGrAfeHfRxzMKP4BALk+UI2s+Bb+gyfaTckuqE018pfB</span>
<span style="color:green">XIaafz/eK9+dmy69AQDBXgpsZPffL/CEgMkf0nEUiZMxl8UBr/4MPeAGo4SpDQ==</span>
<span style="color:green">=QfNE</span>
<span style="color:green">-----END PGP PUBLIC KEY BLOCK-----</span>
<span style="color:red">[GSO]</span> exit code: 0
</pre>

##### Import into `keyring1`

```shell
$ gpg --keyring=keyring2 --no-default-keyring --armor --export 3D0C46A6CA69E511C1DEF2036AC07DE1DF471CCC > 3D0C46A6CA69E511C1DEF2036AC07DE1DF471CCC.pub.key
$ gpg --keyring=keyring1 --no-default-keyring --armor --import 3D0C46A6CA69E511C1DEF2036AC07DE1DF471CCC.pub.key
```
```terminaloutput
gpg: key 6AC07DE1DF471CCC: public key "ss-keyring2 (key in keyring2 and no default keyring. Keyring2 not yet created) <ss-k2@no-home>" imported
gpg: Total number processed: 1
gpg:               imported: 1
gpg: public key of ultimately trusted key 4FC5668576ACD2D0 not found
gpg: public key of ultimately trusted key DC92CCF248179623 not found
gpg: public key of ultimately trusted key 5B3058066AB061B0 not found
gpg: public key of ultimately trusted key 1FE583FDBC0670A2 not found
gpg: marginals needed: 3  completes needed: 1  trust model: pgp
gpg: depth: 0  valid:   8  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 8u
```
exit code: 2

```shell
$ gso gpg --keyring=keyring1 --no-default-keyring --armor --import 3D0C46A6CA69E511C1DEF2036AC07DE1DF471CCC.pub.ke
```

<pre>
gpg: key 6AC07DE1DF471CCC: "ss-keyring2 (key in keyring2 and no default keyring. Keyring2 not yet created) <ss-k2@no-home>" not changed
gpg: Total number processed: 1
gpg:              unchanged: 1
<span style="color:red">[GSO]</span> exit code: 0
</pre>

```shell
$ gso gpg --keyring=keyring1 --armor --import 3D0C46A6CA69E511C1DEF2036AC07DE1DF471CCC.pub.key
```

<pre>
gpg: key 6AC07DE1DF471CCC: "ss-keyring2 (key in keyring2 and no default keyring. Keyring2 not yet created) <ss-k2@no-home>" not changed
gpg: Total number processed: 1
gpg:              unchanged: 1
<span style="color:red">[GSO]</span> exit code: 0
</pre>

##### List keys into keyring2

```shell
$ gso gpg --keyring=keyring2 --list-keys
```
<pre>
<span style="color:green">/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/pubring.kbx</span>
<span style="color:green">-----------------------------------------------------</span>
<span style="color:green">pub   ed25519 2025-08-15 [SC]</span>
<span style="color:green">      BBA35D0B2EFC94BEDFB483561FE583FDBC0670A2</span>
<span style="color:green">uid           [ unknown] ss1 (key included in the keyring1 keyring and also the default keyring) <ss1@keyring1-home.ss></span>
<span style="color:green">sub   cv25519 2025-08-15 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      465034F5989D81C4E73331D85B3058066AB061B0</span>
<span style="color:green">uid           [ unknown] ss1 (key only in default keyring) <ss1@home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      A8C986652CB37391FA0F8742DC92CCF248179623</span>
<span style="color:green">uid           [ unknown] ss1 (key not included in the keyring1 keyring and not included in the default keyring) <ss1@no-keyring.no-home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:green">/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/keyring2</span>
<span style="color:green">--------------------------------------------------</span>
<span style="color:green">pub   ed25519 2025-08-25 [SC]</span>
<span style="color:green">      3D0C46A6CA69E511C1DEF2036AC07DE1DF471CCC</span>
<span style="color:green">uid           [ultimate] ss-keyring2 (key in keyring2 and no default keyring. Keyring2 not yet created) <ss-k2@no-home></span>
<span style="color:green">sub   cv25519 2025-08-25 [E]</span>
<span style="color:green"></span>
<span style="color:red">[GSO]</span> exit code: 0
</pre>

##### List keys into keyring1

```shell
$ gso gpg --keyring=keyring1 --list-keys
```
<pre>
<span style="color:green">/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/pubring.kbx</span>
<span style="color:green">-----------------------------------------------------</span>
<span style="color:green">pub   ed25519 2025-08-15 [SC]</span>
<span style="color:green">      BBA35D0B2EFC94BEDFB483561FE583FDBC0670A2</span>
<span style="color:green">uid           [ unknown] ss1 (key included in the keyring1 keyring and also the default keyring) <ss1@keyring1-home.ss></span>
<span style="color:green">sub   cv25519 2025-08-15 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      465034F5989D81C4E73331D85B3058066AB061B0</span>
<span style="color:green">uid           [ unknown] ss1 (key only in default keyring) <ss1@home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      A8C986652CB37391FA0F8742DC92CCF248179623</span>
<span style="color:green">uid           [ unknown] ss1 (key not included in the keyring1 keyring and not included in the default keyring) <ss1@no-keyring.no-home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:green">/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/keyring1</span>
<span style="color:green">--------------------------------------------------</span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      F32B1F56055B4F0C6D01E312B2CC556843AC8FFE</span>
<span style="color:green">uid           [ultimate] ss1 (key included in the keyring1 keyring but not the default keyring) <ss1@keyring1.no-home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-25 [SC]</span>
<span style="color:green">      67B6CEE060BF7AFE6001E424B104CD6A97F1E4C0</span>
<span style="color:green">uid           [ultimate] ss-keyring1-keyring2 (key in keyring1 and keyring2 but not in default keyring) <ss-k1-k2@no-home></span>
<span style="color:green">sub   cv25519 2025-08-25 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-25 [SC]</span>
<span style="color:green">      C9A3A14816B4FB7074DBC08520BCE80213976CCE</span>
<span style="color:green">uid           [ultimate] ss-k1-k2 (keyring1+keyring2-noDefault both keyrings already present) <ss-k1-k2@gen.no-home></span>
<span style="color:green">sub   cv25519 2025-08-25 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-25 [SC]</span>
<span style="color:green">      3D0C46A6CA69E511C1DEF2036AC07DE1DF471CCC</span>
<span style="color:green">uid           [ultimate] ss-keyring2 (key in keyring2 and no default keyring. Keyring2 not yet created) <ss-k2@no-home></span>
<span style="color:green">sub   cv25519 2025-08-25 [E]</span>
<span style="color:green"></span>
<span style="color:red">[GSO]</span> exit code: 0
</pre>

##### List keys in keyring1 and keyring2

```shell
$ gso gpg --keyring=keyring1 --keyring=keyring2 --list-keys
```
<pre>
<span style="color:green">/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/pubring.kbx</span>
<span style="color:green">-----------------------------------------------------</span>
<span style="color:green">pub   ed25519 2025-08-15 [SC]</span>
<span style="color:green">      BBA35D0B2EFC94BEDFB483561FE583FDBC0670A2</span>
<span style="color:green">uid           [ unknown] ss1 (key included in the keyring1 keyring and also the default keyring) <ss1@keyring1-home.ss></span>
<span style="color:green">sub   cv25519 2025-08-15 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      465034F5989D81C4E73331D85B3058066AB061B0</span>
<span style="color:green">uid           [ unknown] ss1 (key only in default keyring) <ss1@home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      A8C986652CB37391FA0F8742DC92CCF248179623</span>
<span style="color:green">uid           [ unknown] ss1 (key not included in the keyring1 keyring and not included in the default keyring) <ss1@no-keyring.no-home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:green">/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/keyring1</span>
<span style="color:green">--------------------------------------------------</span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      F32B1F56055B4F0C6D01E312B2CC556843AC8FFE</span>
<span style="color:green">uid           [ultimate] ss1 (key included in the keyring1 keyring but not the default keyring) <ss1@keyring1.no-home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-25 [SC]</span>
<span style="color:green">      67B6CEE060BF7AFE6001E424B104CD6A97F1E4C0</span>
<span style="color:green">uid           [ultimate] ss-keyring1-keyring2 (key in keyring1 and keyring2 but not in default keyring) <ss-k1-k2@no-home></span>
<span style="color:green">sub   cv25519 2025-08-25 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-25 [SC]</span>
<span style="color:green">      C9A3A14816B4FB7074DBC08520BCE80213976CCE</span>
<span style="color:green">uid           [ultimate] ss-k1-k2 (keyring1+keyring2-noDefault both keyrings already present) <ss-k1-k2@gen.no-home></span>
<span style="color:green">sub   cv25519 2025-08-25 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-25 [SC]</span>
<span style="color:green">      3D0C46A6CA69E511C1DEF2036AC07DE1DF471CCC</span>
<span style="color:green">uid           [ultimate] ss-keyring2 (key in keyring2 and no default keyring. Keyring2 not yet created) <ss-k2@no-home></span>
<span style="color:green">sub   cv25519 2025-08-25 [E]</span>
<span style="color:green"></span>
<span style="color:green">/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/keyring2</span>
<span style="color:green">--------------------------------------------------</span>
<span style="color:green">pub   ed25519 2025-08-25 [SC]</span>
<span style="color:green">      3D0C46A6CA69E511C1DEF2036AC07DE1DF471CCC</span>
<span style="color:green">uid           [ultimate] ss-keyring2 (key in keyring2 and no default keyring. Keyring2 not yet created) <ss-k2@no-home></span>
<span style="color:green">sub   cv25519 2025-08-25 [E]</span>
<span style="color:green"></span>
<span style="color:red">[GSO]</span> exit code: 0
</pre>

##### List keys without specifying any keyrings

```shell
$ gso gpg --list-keys
```
<pre>
<span style="color:green">/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/pubring.kbx</span>
<span style="color:green">-----------------------------------------------------</span>
<span style="color:green">pub   ed25519 2025-08-15 [SC]</span>
<span style="color:green">      BBA35D0B2EFC94BEDFB483561FE583FDBC0670A2</span>
<span style="color:green">uid           [ unknown] ss1 (key included in the keyring1 keyring and also the default keyring) <ss1@keyring1-home.ss></span>
<span style="color:green">sub   cv25519 2025-08-15 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      465034F5989D81C4E73331D85B3058066AB061B0</span>
<span style="color:green">uid           [ unknown] ss1 (key only in default keyring) <ss1@home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      A8C986652CB37391FA0F8742DC92CCF248179623</span>
<span style="color:green">uid           [ unknown] ss1 (key not included in the keyring1 keyring and not included in the default keyring) <ss1@no-keyring.no-home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:red">[GSO]</span> exit code: 0
</pre>


##### Conclusion

Keys can be part of multiple keyrings if exported and imported explicitly into keyrings.

### Creating Keyrings

#### Just create keyrings with keyring name

Create `keyring3` which is not yet created.

##### Using simple `--fingerprint` command

###### using `--no-default-keyring` switch

```shell
$ gso gpg --keyring=keyring3 --no-default-keyring --fingerprint
```

<pre>
gpg: keybox '/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/keyring3' created
<span style="color:red">[GSO]</span> exit code: 0
</pre>

`keyring3` is created when `--no-default-keyring` is used while listing.

###### no `--no-default-keyring` switch

```shell
$ gso gpg --keyring=keyring4 --fingerprint
```

<pre>
gpg: keyblock resource '/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/keyring4': No such file or directory
<span style="color:green">/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/pubring.kbx</span>
<span style="color:green">-----------------------------------------------------</span>
<span style="color:green">pub   ed25519 2025-08-15 [SC]</span>
<span style="color:green">      BBA3 5D0B 2EFC 94BE DFB4  8356 1FE5 83FD BC06 70A2</span>
<span style="color:green">uid           [ unknown] ss1 (key included in the keyring1 keyring and also the default keyring) <ss1@keyring1-home.ss></span>
<span style="color:green">sub   cv25519 2025-08-15 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      4650 34F5 989D 81C4 E733  31D8 5B30 5806 6AB0 61B0</span>
<span style="color:green">uid           [ unknown] ss1 (key only in default keyring) <ss1@home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      A8C9 8665 2CB3 7391 FA0F  8742 DC92 CCF2 4817 9623</span>
<span style="color:green">uid           [ unknown] ss1 (key not included in the keyring1 keyring and not included in the default keyring) <ss1@no-keyring.no-home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:red">[GSO]</span> exit code: 2
</pre>

No `keyring4` created when `--no-default-keyring` switch is not used.

#### Create keyrings with file paths

Keyrings can be created as a file path as keyrings are mostly files

##### Slash in filepath

Slashes may behave as directory tree strctures and hance may require directory creation

###### using no `--no-default-keyring` switch

```shell
$ gso gpg --keyring=path/with/slash/to/the/keyring --fingerprint
```

<pre>
gpg: keyblock resource 'path/with/slash/to/the/keyring': No such file or directory
<span style="color:green">/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/pubring.kbx</span>
<span style="color:green">-----------------------------------------------------</span>
<span style="color:green">pub   ed25519 2025-08-15 [SC]</span>
<span style="color:green">      BBA3 5D0B 2EFC 94BE DFB4  8356 1FE5 83FD BC06 70A2</span>
<span style="color:green">uid           [ unknown] ss1 (key included in the keyring1 keyring and also the default keyring) <ss1@keyring1-home.ss></span>
<span style="color:green">sub   cv25519 2025-08-15 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      4650 34F5 989D 81C4 E733  31D8 5B30 5806 6AB0 61B0</span>
<span style="color:green">uid           [ unknown] ss1 (key only in default keyring) <ss1@home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      A8C9 8665 2CB3 7391 FA0F  8742 DC92 CCF2 4817 9623</span>
<span style="color:green">uid           [ unknown] ss1 (key not included in the keyring1 keyring and not included in the default keyring) <ss1@no-keyring.no-home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:red">[GSO]</span> exit code: 2
</pre>

###### Using `--no-default-keyring` switch

```shell
$ gso gpg --keyring=path/with/slash/to/the/keyring --no-default-keyring --fingerprint
```

<pre>
gpg: keyblock resource 'path/with/slash/to/the/keyring': No such file or directory
<span style="color:red">[GSO]</span> exit code: 2
</pre>

###### create parent directories and then try

```shell
$ mkdir -p .gnupg/path/with/slash/to/the
$ gso gpg --keyring=.gnupg/path/with/slash/to/the/keyring --no-default-keyring --fingerprint
```

<pre>
gpg: keybox '.gnupg/path/with/slash/to/the/keyring' created
<span style="color:red">[GSO]</span> exit code: 0
</pre>

Path is created even when it isn't under the .gnupg dir

```shell
$ mkdir -p path/with/slash/to/the
$ gso gpg --keyring=path/with/slash/to/the/keyring --no-default-keyring --fingerprint
```

<pre>
gpg: keybox 'path/with/slash/to/the/keyring' created
<span style="color:red">[GSO]</span> exit code: 0
</pre>


###### Conclusion

keyring not created when path/to/keyring has no already existing parent directories.

##### `.com` in filepath

Check if keyrings can be segreated by their origin url

```shell
$ mkdir -p .gnupg/github.com/suhasdotcom
$ gso gpg --keyring=.gnupg/github.com/suhasdotcom/deep-tool-docs.git --no-default-keyring --fingerprint
```

<pre>
gpg: keybox '.gnupg/github.com/suhasdotcom/deep-tool-docs.git' created
<span style="color:red">[GSO]</span> exit code: 0
</pre>

###### Conclusion

Yes, keyrings can be seggregated into their own keyring URLs.

##### Create the full keyring file and then list fingerprints

###### Without `--no-default-keyring` switch

```shell
$ touch .gnupg/github.com/suhasdotcom/deep-tool-docs-1.git
$ gso gpg --keyring=.gnupg/github.com/suhasdotcom/deep-tool-docs-1.git --no-default-keyring --fingerprint
```

<pre>
<span style="color:red">[GSO]</span> exit code: 0
</pre>

```shell
$ gso gpg --keyring=.gnupg/github.com/suhasdotcom/deep-tool-docs-1.git --no-default-keyring --full-gen-key
```

<pre>
gpg (GnuPG) 2.4.5-unknown; Copyright (C) 2024 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Please select what kind of key you want:
   (1) RSA and RSA
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (9) ECC (sign and encrypt) *default*
  (10) ECC (sign only)
  (14) Existing key from card
Your selection?
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
Key is valid for? (0) 
Key does not expire at all
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: ss-dtd
Email address: ss-dtd@no-home
Comment: Key in .gnupg/github.com/suhasdotcom/deep-tool-docs-1.git keyring but not default
You selected this USER-ID:
    "ss-dtd (Key in .gnupg/github.com/suhasdotcom/deep-tool-docs-1.git keyring but not default) <ss-dtd@no-home>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit?
Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? o
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: revocation certificate stored as '/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/openpgp-revocs.d/D7CD162262505513BBA15DD54FC5668576ACD2D0.rev'
public and secret key created and signed.

<span style="color:green">pub   ed25519 2025-08-26 [SC]</span>
<span style="color:green">      D7CD162262505513BBA15DD54FC5668576ACD2D0</span>
<span style="color:green">uid                      ss-dtd (Key in .gnupg/github.com/suhasdotcom/deep-tool-docs-1.git keyring but not default) <ss-dtd@no-home></span>
<span style="color:green">sub   cv25519 2025-08-26 [E]</span>
<span style="color:green"></span>
<span style="color:red">[GSO]</span> exit code: 0
</pre>


Now, list keys only in the `.gnupg/github.com/suhasdotcom/deep-tool-docs-1.git` keyring

```shell
$ gso gpg --keyring=.gnupg/github.com/suhasdotcom/deep-tool-docs-1.git --no-default-keyring --fingerprint
```

<pre>
gpg: checking the trustdb
gpg: public key of ultimately trusted key 20BCE80213976CCE not found
gpg: public key of ultimately trusted key 6AC07DE1DF471CCC not found
gpg: public key of ultimately trusted key B104CD6A97F1E4C0 not found
gpg: public key of ultimately trusted key DC92CCF248179623 not found
gpg: public key of ultimately trusted key 5B3058066AB061B0 not found
gpg: public key of ultimately trusted key B2CC556843AC8FFE not found
gpg: public key of ultimately trusted key 1FE583FDBC0670A2 not found
gpg: marginals needed: 3  completes needed: 1  trust model: pgp
gpg: depth: 0  valid:   8  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 8u
<span style="color:green">.gnupg/github.com/suhasdotcom/deep-tool-docs-1.git</span>
<span style="color:green">--------------------------------------------------</span>
<span style="color:green">pub   ed25519 2025-08-26 [SC]</span>
<span style="color:green">      D7CD 1622 6250 5513 BBA1  5DD5 4FC5 6685 76AC D2D0</span>
<span style="color:green">uid           [ultimate] ss-dtd (Key in .gnupg/github.com/suhasdotcom/deep-tool-docs-1.git keyring but not default) <ss-dtd@no-home></span>
<span style="color:green">sub   cv25519 2025-08-26 [E]</span>
<span style="color:green"></span>
<span style="color:red">[GSO]</span> exit code: 2
</pre>


Now, list keys in default-keyring:

```shell
$ gso gpg --fingerprint
```

<pre>
<span style="color:green">/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/pubring.kbx</span>
<span style="color:green">-----------------------------------------------------</span>
<span style="color:green">pub   ed25519 2025-08-15 [SC]</span>
<span style="color:green">      BBA3 5D0B 2EFC 94BE DFB4  8356 1FE5 83FD BC06 70A2</span>
<span style="color:green">uid           [ unknown] ss1 (key included in the keyring1 keyring and also the default keyring) <ss1@keyring1-home.ss></span>
<span style="color:green">sub   cv25519 2025-08-15 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      4650 34F5 989D 81C4 E733  31D8 5B30 5806 6AB0 61B0</span>
<span style="color:green">uid           [ unknown] ss1 (key only in default keyring) <ss1@home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      A8C9 8665 2CB3 7391 FA0F  8742 DC92 CCF2 4817 9623</span>
<span style="color:green">uid           [ unknown] ss1 (key not included in the keyring1 keyring and not included in the default keyring) <ss1@no-keyring.no-home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:red">[GSO]</span> exit code: 0
</pre>

List keys in `.gnupg/github.com/suhasdotcom/deep-tool-docs-1.git` keyring without `--no-default-keyring` switch

```shell
$ gso gpg --keyring=.gnupg/github.com/suhasdotcom/deep-tool-docs-1.git --fingerprint
```

<pre>
<span style="color:green">/d/TestTmpDir/gpg/gpg-keyring-test/.gnupg/pubring.kbx</span>
<span style="color:green">-----------------------------------------------------</span>
<span style="color:green">pub   ed25519 2025-08-15 [SC]</span>
<span style="color:green">      BBA3 5D0B 2EFC 94BE DFB4  8356 1FE5 83FD BC06 70A2</span>
<span style="color:green">uid           [ unknown] ss1 (key included in the keyring1 keyring and also the default keyring) <ss1@keyring1-home.ss></span>
<span style="color:green">sub   cv25519 2025-08-15 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      4650 34F5 989D 81C4 E733  31D8 5B30 5806 6AB0 61B0</span>
<span style="color:green">uid           [ unknown] ss1 (key only in default keyring) <ss1@home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:green">pub   ed25519 2025-08-16 [SC]</span>
<span style="color:green">      A8C9 8665 2CB3 7391 FA0F  8742 DC92 CCF2 4817 9623</span>
<span style="color:green">uid           [ unknown] ss1 (key not included in the keyring1 keyring and not included in the default keyring) <ss1@no-keyring.no-home></span>
<span style="color:green">sub   cv25519 2025-08-16 [E]</span>
<span style="color:green"></span>
<span style="color:green">.gnupg/github.com/suhasdotcom/deep-tool-docs-1.git</span>
<span style="color:green">--------------------------------------------------</span>
<span style="color:green">pub   ed25519 2025-08-26 [SC]</span>
<span style="color:green">      D7CD 1622 6250 5513 BBA1  5DD5 4FC5 6685 76AC D2D0</span>
<span style="color:green">uid           [ultimate] ss-dtd (Key in .gnupg/github.com/suhasdotcom/deep-tool-docs-1.git keyring but not default) <ss-dtd@no-home></span>
<span style="color:green">sub   cv25519 2025-08-26 [E]</span>
<span style="color:green"></span>
<span style="color:red">[GSO]</span> exit code: 0
</pre>
