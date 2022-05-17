# MSF-screenrecord-on-MacOS
By: Antonio Cheong of YCISCQ
## !!!!!!!!!! Apple agreed to credit me for the CVE, however, they have not done so and instead credited "an anonymous researcher" Bounty payment has also yet to be recieved!!!!!!!!!!!!!!!!
The impact description is also misleading as this exploit can bypass nearly all TCC permissions except for camera and microphone.

They have also yet to pay out my security bounty.

## Affected Versions
As a student, I have limited access to devices in which I can test this vulnerability. The versions I have tested so far are `MacOS 12.3.1`, `MacOS 12.3`, `MacOS 12.0`, `MacOS 10.15.7`, and `MacOS 11.6.1`. The vulnerability works on all the versions tested on.

## So what is the bug?
Impact: A malicious application may be able to bypass certain Privacy preferences

Description: When an executable is deleted while running, the process is able to bypass restrictions for Screen Recording as well as certain accessibility settings, namely keystroke injection (using metasploit).

Possible reasons: macOS cannot find the permissions of the parent executable of the process and thus does not prevent screen recording. However, it is confusing that the camera and mic are still properly restricted.

Details: 
- `tccutil reset All` results in a permission prompt being shown -- Accessing camera or mic also results in permission prompt being shown. 
-  The retrieved screenshot includes all visible windows

## Prerequisites
It works on a fresh installation of macOS.

## POC
Running the POC from `src/POC.sh` will produce a screenshot without TCC prompt and includes all windows visible to the user. The script must occasionally be run multiple times (max 3) for a screenshot to be produced.

## History and additional functions
This exploit was first discovered by me when experimenting with Metasploit's web delivery. I found that by deleting the dropped executable immediately after a meterpreter shell opens, the shell gains capabilities which are normally locked behind a TCC prompt. This vulnerability also allows an attacker to inject keystrokes. However, I am not an Objective-C developer and thus have not been able to create my own POC. This effect can still be achieved through Metasploit.

## Source code and credits
`src/POC.sh` -- Shell script: Copies `screenshot`, executes it, and deletes it while it is running.

`src/screenshot` -- Mach-O executable: Takes a screenshot and saves it to `src/screenshot.png`. Compiled from `src/screenshot.m`.

`src/screenshot.m` -- Credits to *theevilbit* for the basic code. The only modification I made was to add a 3 second sleep before the screenshot to allow for enough time to delete the executable before the screenshot is taken.
