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

Pivot on VHDMP timestamp of the mount operation, look for shellbags to determine if mount path was opened up, then evidence of presence/ execution artifacts to locate any binaries, check MFT before and after TS for files created and pivot on anything found

There could also be event logs around the TS, like MS office events, wmi and pwsh. If the iso came over the internet, browser history around TS could help determine initial vector if it was webmail, any emails found can then be examined for HTML smuggling.
