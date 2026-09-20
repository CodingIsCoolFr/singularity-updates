<div align="center">

<img src="brand/banner.png" alt="Singularity" width="100%">

<br>

![Windows](https://img.shields.io/badge/platform-Windows%20x64-cdd6e6?style=flat-square&labelColor=07090e)
![Installer](https://img.shields.io/badge/installer-no%20administrator-8892a6?style=flat-square&labelColor=07090e)
![Voice](https://img.shields.io/badge/voice-end--to--end%20encrypted-a9c6e0?style=flat-square&labelColor=07090e)
![Updates](https://img.shields.io/badge/updates-built%20in-8892a6?style=flat-square&labelColor=07090e)

<br>

### [Download the latest release](https://github.com/CodingIsCoolFr/singularity-updates/releases/latest)

Windows x64 · about 54 MB · installs for you only

</div>

---

A Discord client written from scratch in C++ with Qt 6. No Electron, no web
view, no plugin folder — the plugins are part of the binary, so there is
nothing on disk for anything else to swap out.

Calls work: you can talk, you can hear, and the audio is end to end encrypted
the way Discord has required since March 2026. Cameras and shared screens are
received and decoded as well.

**This repository holds the installers and nothing else.** There is no source
code here. See [why](#why-this-is-a-separate-repository) below.

## Installing

Download the setup program from the
[latest release](https://github.com/CodingIsCoolFr/singularity-updates/releases/latest)
and run it.

It installs **for you only and never asks for administrator**. There is no
service, no driver and nothing shared, so there is nothing to elevate for, and
a small program asking for rights it does not need is how it starts looking
untrustworthy.

Windows may warn that the publisher is unknown, because the installer is not
code signed. A certificate costs money every year and this is a personal
project; choose *More info* then *Run anyway* if you are willing to.

Uninstalling asks whether to remove your saved sign-in and settings, and
defaults to keeping them. Those live in your roaming profile rather than the
install folder, so removing the program does not silently take the account
with it.

## Updating

You should not need to come back here. The program checks for a newer version
a few seconds after it starts, and quietly: an up to date copy says nothing at
all. An updater that interrupts to announce that nothing has happened is one
people learn to dismiss without reading, which is the wrong habit for the day
it has something to say.

**Singularity → Check for updates...** asks on demand and reports either way.
Nothing downloads or installs without being agreed to.

## Why this is a separate repository

Singularity's source is private. A private repository answers `404` to anyone
without a token, so an updater pointed at it would need a token shipped inside
the program — and anything shipped inside a program can be taken back out of
it. That token would then grant read access to the whole source.

So the source stays private and the installers are published here. The updater
reads this repository's latest release, and nothing secret has to travel with
the application to make that work.

Two things are checked rather than trusted, since this is where a program
fetches code it will then run:

| | |
| --- | --- |
| **Where it came from** | The download address is taken from a release body, which is text from a web service. It is matched against the hosts GitHub actually serves releases from, including the one it redirects to, rather than followed because of where it was found. |
| **That it is whole** | The finished file is measured against the size the release declares. A truncated installer runs, fails part way through, and leaves a half replaced program behind. |

## Warning, read this before installing

**Discord does not permit third party clients on a normal user account, and
using one can get the account banned.** That risk is taken here deliberately.

Your password is sent to `discord.com` and nowhere else, is never written to
disk and never logged. Only the session token that comes back is kept, sealed
with your Windows account key so no other account on the machine can read it.
Captchas are never answered; if Discord demands one, the sign-in window says so
and offers token entry instead. Images are only ever fetched from Discord's own
hosts, so a stranger's message cannot make your client call an address of their
choosing.
