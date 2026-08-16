
# DURA_MIDI
## for MOTU classic Macintosh serial RS422 MIDI interfaces, to keep them going. 

DURA - Dead Unicorn Resurrection Adapter. <br><br>
Bring all those dead MOTUs with 9-pin mini din connectors back to life! All (most?) of those interfaces speaking the same MOTU serial MIDI dialect, even Opcodes.<br>

Hardware, class compliant multi-cable (x8) USB MIDI adapter for large family of old MOTU MIDI interfaces that become obsolete with elimination of serial mini-din ports on old MACs. Opcodes will come alive with MOTUs as well! (Because Opcodes have MOTU emulation.)
<br><img width="400" alt="DURA MIDI" src="https://github.com/user-attachments/assets/67851090-fab5-4048-a1de-60eea7536f9c" />

<br>First draft firmware of mostly fully functional adapter is available (see release).<br>
You need to make some hardware. Ideally - proper RS422 interface with phy chip. But, I initially was running it with just a bunch of resistors to bring signals to recognizable levels. See Poor Man's schematic. It can be cheap alternatives for those who has limited budget. It can be made for under $5.<br><br>

## First release firmware
<br>
Appeared to be fully functional. Some notes:<br>
1. It runs at "fast", aka "x4" interface speed, that is 125KHz. Make sure you set your interface to "fast" and not to 1MHz. I am still trying to get 1MHz going, but it has complex handshake that I did not figured out yet. Any help is appreciated. "Fast" mode does not need handshake, it just works.<br>
2. Some interfaces, like original/classic MIDI Express do not have "fast" mode. If you have one of those - you may only use "Legacy" firmware for transparent one cable (MIDI Port) setup. <br>
3. Firmware fully support and applies "Running Status" to all outgoing messages. It will properly apply and strip "Running Status" as necessary when translating between serial and USB.<br>
4. It will strip "Active Sensing" messages when translating from serial to USB.<br>
5. I chose pin-out for crossover cables. Most cables nowadays sold on Amazon are straight pin-for-pin. To be sure it is crossover - make your own. PoorMan's schematic is for plug, so it is straight pin-out.<br>
6. It will pass SysEx to each port as intended. I do not know if there is any practical limit to the length of SysEx. Most interfaces do choke at some point. Would be interesting to test and see. It may not have a limit at all as it handles MIDI as a stream. Presently I do not parse SysEx messages intended for the interface itself. It will be needed to implement ClockWork like functionality. Sometime in the future. <br>
7. For highly technical crowd: It appeared that all interfaces I have to try are able to receive asynchronous communications. That 1MHz synchronous clock apparently for the Mac and not for the interfaces. So far I see no problem (no communication errors) running 100% asynchronous UART. Let's keep an eye on it.<br>

<br>
The driver: It may prompt for MOTU driver. Please manually select generic "Composite Device".
<br><br>
Enjoy it and let me know how it works for you.



