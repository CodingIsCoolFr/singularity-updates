# Singularity releases

This repository carries the installers for **Singularity**, and nothing else.
There is no source code here.

## Why it exists

Singularity itself lives in a private repository. A private repository answers
`404` to anyone without a token, so an updater pointed at it would need a token
shipped inside the program — and anything shipped inside a program can be taken
back out of it. That token would then grant read access to the whole source.

So the source stays private and the installers are published here instead. The
updater in the program reads this repository's latest release, and nothing
secret has to travel with the application to make that work.

## Download

The newest installer is always on the [releases page](https://github.com/CodingIsCoolFr/singularity-updates/releases/latest).

Windows x64. It installs for you only and never asks for administrator: there
is no service, no driver and nothing shared, so there is nothing to elevate for.

## Warning

Discord does not permit third party clients on a normal user account, and using
one can get the account banned.
