---
date: "2010-08-20T00:00:00Z"
published: true
title: 'Tomato in, IPCop out'
---

<table align="" cellpadding="0" cellspacing="0" class="tr-caption-container" style="float: right; margin-left: 1em; text-align: right;"><tbody>
<tr><td style="text-align: center;"><img border="0" height="146" src="/img/ipcop-20100819-2.png" width="200" /></td></tr>
<tr><td class="tr-caption" style="text-align: center;">It even shows inodes usage.</td></tr>
</tbody></table>

I've bid <i>vale</i> to my beige-box <a href="http://www.ipcop.org/">IPCop</a> firewall. I had built it in 2004 out of a circa-1996 Pentium machine and a couple NICs. For 6 years I've wondered when this little choo-choo would die... it never did.

Some neat things about IPCop compared to its contemporaries: usage graphs, port forwarding, intrusion detection, dynamic DNS; plus other things I never used, such as traffic shaping.

Using IPCop, one could make a router from spare desktop parts, and avoid the Linksys reboot ritual.

<table cellpadding="0" cellspacing="0" class="tr-caption-container" style="clear: right; float: right; margin-bottom: 1em; margin-left: 1em; text-align: right;"><tbody>
<tr><td style="text-align: center;"><img border="0" height="200" src="/img/ipcop-20100819-3.png" width="197" /></td></tr>
<tr><td class="tr-caption" style="text-align: center;">A 100 MHz P1 was overkill.</td></tr>
</tbody></table><img border="0" height="146" src="/img/ipcop_beige_box-20100819.jpg" height="300" width="400" />

The machine ran headless, with no fans (except the PSU). I even had custom shutdown &amp; bootup melodies (using <a href="http://johnath.com/beep/">beep</a> to vary the pitch of the system speaker).

Eventually, over years, my network turned into this:

<pre>[IPCop] &lt;-- [Netgear switch] &lt;-- [Buffalo WAP]</pre>

Obviously this can be improved. The Buffalo WAP firmware can be replaced with<a href="http://www.polarcloud.com/tomato"> tomato, an open source Broadcom firmware replacement</a> that fixes bugs present in the factory firmware, adds features, and has a very nice web GUI. I can eliminate the IPCop box and the Netgear switch; this saves electricity and reduces ambient noise.

### Installing tomato on a Buffalo whr-hp-g54

The readme.htm gets you 97% there, but the following notes may save you time:

- The "reset" button is the INIT button on the bottom of the router unit--recessed, so that a pen/paperclip is needed to push it. It is <i>not</i> the button on the top of the unit (the AOSS button).
- After you reset the router unit, if you can't ping 192.168.11.1, make sure you plugged the ethernet cable into one of the numbered ports, <i>not</i> the WAN port. (Oops.)
- On my install of Windows 7 Ultimate (x64), the <b>tftp</b> command was not available from the command line, so `whr_install.bat` failed. I found tftp in:
  ```
  C:\Windows\winsxs\amd64_microsoft-windows-t..-deployment-package_31bf3856ad364e35_6.1.7600.16385_none_bac291589d407fde\tftp
  ```
  then I edited whr_install.bat so that references to tftp used that absolute path.

