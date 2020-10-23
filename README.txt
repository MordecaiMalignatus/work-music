Az' Work Music Stash
====================

I like to listen to DJ sets while working, so I build a little thing to download
stuff for me. This is not the actual content because that would be unwieldy, but
it's a log of the stuff I downloaded and the tooling to download it again.

Requires `youtube-dl`, and Ruby/Rake. You'll also want `open` to be a command,
this specifically is for Macs but if you edit it to be `xdg-open` it should also
work with linux. I usually use foobar2k for this.

Usage
=====

Adding a new set:
-----------------

1. rake
2. Follow instructions

Downloading `dl_store.csv`:
---------------------------

1. rake dl
2. Wait.

Opening a random set:
---------------------

1. rake play
2. Start grooving.
