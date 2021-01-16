# Firefox notes

## Start Firefox with profile selector

* Via script in [/usr/local/bin/firefox-profiles.sh](usr/local/bin/firefox-profiles.sh)
* Directly on menu: [/usr/share/applications/firefox-profiles.desktop](usr/share/applications/firefox-profiles.desktop)
  * Starts firefox with ```firefox -p %u```
  * Adds ```Actions=new-window;new-private-window;```
  * Take a look at the files

## Enforcing copy & paste on websites

Disable clipboardevents in about:config:

about:config ==> dom.event.clipboardevents.enabled ==> false

