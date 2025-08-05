# Export keys and subkeys

Continuing from [generate-keys](./generate-keys.md).

## Exporting primary key

#### Without secrets

Primary key no secret export:

```shell
$ gpg --homedir=base-keys/ --no-default-keyring --keyring=base-keys/base-keyring --armor --export 50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1 > 50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1.prim.no-sec.asc
```

Primary key no secret read:

```shell
$ gpg --show-keys 50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1.prim.no-sec.asc
```

```terminaloutput
pub   ed25519 2025-08-04 [C]
      50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1
uid                      ss-2 <ss-2@ss.ss>
uid                      ss-1 <ss-1@ss.ss>
sub   ed25519 2025-08-04 [S] [expires: 2025-10-03]
sub   cv25519 2025-08-04 [E] [expires: 2025-10-03]
```

#### With secrets

Primary key with secret export:

```shell
$ gpg --homedir=base-keys/ --no-default-keyring --keyring=base-keys/base-keyring --armor --export-secret-keys 50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1 > 50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1.prim.sec.asc
```

Primary key with secret read:

```shell
$ gpg --show-keys 50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1.prim.sec.asc
```

```terminaloutput
sec#  ed25519 2025-08-04 [C]
      50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1
uid                      ss-2 <ss-2@ss.ss>
uid                      ss-1 <ss-1@ss.ss>
ssb#  ed25519 2025-08-04 [S] [expires: 2025-10-03]
ssb#  cv25519 2025-08-04 [E] [expires: 2025-10-03]
```

Notice the sec# exporting the secret as well.

Machine-readable format using the `--with-colons` switch:

```shell
$ gpg --with-colons --show-keys 50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1.prim.sec.asc
```

```terminaloutput
sec:-:255:22:101DA1D42E4FECA1:1754310156:::-:::cESC:::#::ed25519:::0:
fpr:::::::::50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1:
grp:::::::::96F6FC9D62E39F5639A593C99BDF6EADBF9C9719:
uid:-::::1754310216::4A99323F78611F8232E22CBE2DDD7A9294645FF6::ss-2 <ss-2@ss.ss>::::::::::0:
uid:-::::1754310156::F19433FDF4480C086E0D9D7967BB63A6DC30805E::ss-1 <ss-1@ss.ss>::::::::::0:
ssb:-:255:22:53AC884CDF5DFACD:1754310230:1759494230:::::s:::#::ed25519::
fpr:::::::::A4635B7B7AA88A81313A5DEF53AC884CDF5DFACD:
grp:::::::::854E4CFDC4B90782647897558BC100E238B85CFE:
ssb:-:255:18:F25884FA53937E77:1754310280:1759494280:::::e:::#::cv25519::
fpr:::::::::9FE62818CD52EDD6F33DEF04F25884FA53937E77:
grp:::::::::E249207A7C6DF644BE46AAECAC34E248069AC621:
```

Notice the secret `sec 101DA1D42E4FECA1` and all the subkeys are exported.

## Exporting specific subkey

`F25884FA53937E77` is only for encryption and expires earlier than the main primary key.

```shell
$ gpg --homedir=base-keys/ --no-default-keyring --keyring=base-keys/base-keyring --armor --export F25884FA53937E77! > F25884FA53937E77.enc.ssb.no-sec.asc
```

F25884FA53937E77 file contents

```shell
cat F25884FA53937E77.enc.ssb.no-sec.asc
```

```terminaloutput
-----BEGIN PGP PUBLIC KEY BLOCK-----

mDMEaJCmDBYJKwYBBAHaRw8BAQdA97kSezg5DSlDuel6jY5o8xRez4qoNDrEPTOb
eQdTTvC0EXNzLTEgPHNzLTFAc3Muc3M+iJMEExYKADsWIQRQw1VDue1EKebEprgQ
HaHULk/soQUCaJCmDAIbAQULCQgHAgIiAgYVCgkICwIEFgIDAQIeBwIXgAAKCRAQ
HaHULk/soWL2AQC6FlmdLkb8b5w4FLaz3n0YEWwwBKH7RKXT1URvweRVjwD+ML0Y
LlKtp6WhE58xifJM/ijbgYG4y5VJ9w1j5fGKUwS0EXNzLTIgPHNzLTJAc3Muc3M+
iJMEExYKADsWIQRQw1VDue1EKebEprgQHaHULk/soQUCaJCmSAIbAQULCQgHAgIi
AgYVCgkICwIEFgIDAQIeBwIXgAAKCRAQHaHULk/socTOAP9BtOGcrTWuMPgprQNo
xhTdz768oqKpedZN4elJZ9KgegEAyDTu/W5coz62E1M14R/Ghb6w3w24Q/x0OLnT
gi4/6QK4OARokKaIEgorBgEEAZdVAQUBAQdAbCzYnqcl8qnj7INJlTe57BiWJc1J
YQ/rXcseYf4lMzgDAQgHiH4EGBYKACYWIQRQw1VDue1EKebEprgQHaHULk/soQUC
aJCmiAIbDAUJAE8aAAAKCRAQHaHULk/soaYxAQDuYYpm84cQYOdAZ5LLV91qcAvO
bksAUlZsn7XkzgdemwEAnL8+b/7+qUR6DuRb45BYRvVvrgnTggsHwGGv6IuVsAY=
=KyAy
-----END PGP PUBLIC KEY BLOCK-----
```

Get keys on `F25884FA53937E77`:

```shell
$ gpg --show-keys F25884FA53937E77.enc.ssb.no-sec.asc 
```

```terminaloutput
pub   ed25519 2025-08-04 [C]
50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1
uid                      ss-2 <ss-2@ss.ss>
uid                      ss-1 <ss-1@ss.ss>
sub   cv25519 2025-08-04 [E] [expires: 2025-10-03]
```

Machine-readable format, using the `--with-colons` switch:

```shell
$ gpg --with-colons --show-keys F25884FA53937E77.enc.ssb.no-sec.asc 
```

```terminaloutput
pub:-:255:22:101DA1D42E4FECA1:1754310156:::-:::cEC:::::ed25519:::0:
fpr:::::::::50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1:
uid:-::::1754310216::4A99323F78611F8232E22CBE2DDD7A9294645FF6::ss-2 <ss-2@ss.ss>::::::::::0:
uid:-::::1754310156::F19433FDF4480C086E0D9D7967BB63A6DC30805E::ss-1 <ss-1@ss.ss>::::::::::0:
sub:-:255:18:F25884FA53937E77:1754310280:1759494280:::::e:::::cv25519::
fpr:::::::::9FE62818CD52EDD6F33DEF04F25884FA53937E77:
```

As evident, only the exported key is exported, leaving out the primary key and secret key contents. 


## Exporting specific subkeys with its secrets

Whenever a specific subkey is exported with its secrets, then the whole primary key and secrets are exported.

This is the case because secret is tied to the whole primary key.

```shell
$ gpg --homedir=base-keys/ --no-default-keyring --keyring=base-keys/base-keyring --export-secret-subkeys --armor F25884FA53937E77 > F25884FA53937E77.enc.asc
```

`F25884FA53937E77.enc.asc` is an encryption subkey export.

Contents of `F25884FA53937E77.enc.asc`:

```shell
$ gpg --with-colons --show-keys F25884FA53937E77.enc.asc
```

```terminaloutput
sec:-:255:22:101DA1D42E4FECA1:1754310156:::-:::cESC:::#::ed25519:::0:
fpr:::::::::50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1:
grp:::::::::96F6FC9D62E39F5639A593C99BDF6EADBF9C9719:
uid:-::::1754310216::4A99323F78611F8232E22CBE2DDD7A9294645FF6::ss-2 <ss-2@ss.ss>::::::::::0:
uid:-::::1754310156::F19433FDF4480C086E0D9D7967BB63A6DC30805E::ss-1 <ss-1@ss.ss>::::::::::0:
ssb:-:255:22:53AC884CDF5DFACD:1754310230:1759494230:::::s:::#::ed25519::
fpr:::::::::A4635B7B7AA88A81313A5DEF53AC884CDF5DFACD:
grp:::::::::854E4CFDC4B90782647897558BC100E238B85CFE:
ssb:-:255:18:F25884FA53937E77:1754310280:1759494280:::::e:::#::cv25519::
fpr:::::::::9FE62818CD52EDD6F33DEF04F25884FA53937E77:
grp:::::::::E249207A7C6DF644BE46AAECAC34E248069AC621:
```

As evident, all the subkeys (see the signing subkey `53AC884CDF5DFACD`) and secrets (all fingerprints plus `sec` `101DA1D42E4FECA1`) are also exported.

## Trust information is not exported.

Here the `[ultimate]` signals trust level.

When queried directly from cmd:

```shell
$ gpg --homedir=base-keys/ --no-default-keyring --keyring=base-keys/base-keyring --list-keys
```

```terminaloutput
----------------------
pub   ed25519 2025-08-04 [C]
      50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1
uid           [ultimate] ss-2 <ss-2@ss.ss>
uid           [ultimate] ss-1 <ss-1@ss.ss>
sub   ed25519 2025-08-04 [S] [expires: 2025-10-03]
sub   cv25519 2025-08-04 [E] [expires: 2025-10-03]
```

When exported to a file and then queried from the file, no trust level is indicated:

Primary key no secret export:

```shell
$ gpg --homedir=base-keys/ --no-default-keyring --keyring=base-keys/base-keyring --armor --export 50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1 > 50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1.prim.no-sec.asc
```

Primary key no secret read:

```shell
$ gpg --show-keys 50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1.prim.no-sec.asc
```

```terminaloutput
pub   ed25519 2025-08-04 [C]
      50C35543B9ED4429E6C4A6B8101DA1D42E4FECA1
uid                      ss-2 <ss-2@ss.ss>
uid                      ss-1 <ss-1@ss.ss>
sub   ed25519 2025-08-04 [S] [expires: 2025-10-03]
sub   cv25519 2025-08-04 [E] [expires: 2025-10-03]
```

Notice the no `[ultimate]` trust level is indicated.
