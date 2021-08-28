# issabel Feature Code For Say Date and Time
Create Feature Code for Say Date and Time on Persian language

**I used [jdate](http://jdf.scr.ir/) for date convertor with bash**

1. Copy `jalalidate` folder in `/etc/asterisk/`
- Check this directory `/var/lib/asterisk/sounds` for exist `pr` folder
2. Open `/etc/asterisk/extensions_custom.conf` create `zarbinnetwork-features` context and include it
```
[from-internal-custom]
include => zarbinnetwork-features

[zarbinnetwork-features]
exten => *20,1,Answer()
exten => *20,n,Set(CHANNEL(language)=pr)
exten => *20,n,Set(date=${SHELL(sh /etc/asterisk/jalalidate/jdate.sh)})
exten => *20,n,Set(year=${CUT(date,,1)})
exten => *20,n,Set(month=${CUT(date,,2)})
exten => *20,n,Set(day=${CUT(date,,3)})
exten => *20,n,Playback(pr/today)
exten => *20,n,Playback(pr/digits/${day}-me)
exten => *20,n,Playback(pr/digits/${month}mo-e)
exten => *20,n,SayNumber(${year})
exten => *20,n,Set(time=${STRFTIME(${EPOCH},,%H%M%S)})
exten => *20,n,Playback(pr/currently)
exten => *20,n,Playback(pr/hours)
exten => *20,n,SayNumber(${time:0:2})
exten => *20,n,Playback(pr/extra/va)
exten => *20,n,SayNumber(${time:2:2})
exten => *20,n,Playback(pr/digits/daghigheh-o)
exten => *20,n,SayNumber(${time:-2})
exten => *20,n,Playback(pr/digits/sanieh)
exten => *20,n,Playback(pr/extra/mibashad)
exten => *20,n,Hangup()
```
3. Reload asterisk dialplan
``` bash script
asterisk -r
reload
exit
```
