# iso-mounting-forensic

PS C:\Users\Andrea\Desktop\Events-Ripper-main> .\erip.exe
eRip v.1.0 - CLI events file ripper tool


Ex: C:\>erip -f c:\case\events.txt -a

    C:\>erip -f c:\case\events.txt -p failedlogins
    
    C:\>erip -l -c

All output goes to STDOUT; use redirection (ie, > or >>) to output to a file.

copyright 2022 Quantum Analytics Research, LLC

PS C:\Users\Andrea\Desktop\Events-Ripper-main> .\wevtx.bat .\Microsoft-Windows-VHDMP-Operational.evtx evets10.txt

PS C:\Users\Andrea\Desktop\Events-Ripper-main> .\erip.exe -f .\evets10.txt -p mount

Launching mount v.20221010

Get VHD[X}/ISO files mounted


System name: LAPTOP-HL1G97FB

Files mounted (VHD[X], ISO):

C:\Users\Andrea\Downloads\ubuntu-20.04.4-desktop-amd64.iso

Analysis Tip: Microsoft-Windows-VHDMP/1 events provide a list of files mounted or "surfaced".
