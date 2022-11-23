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

#SCRUM https://twitter.com/Purp1eW0lf/status/1504491539099172866
SRUM is maybe one of the best Windows digital forensic artefacts, if you’re willing to roll your sleeves up. 

You can get proof of execution and execution runtime, as well as proof of network communication and the bytes sent and received

Let's take a look in this #DFIR thread
Since Win8, System Resource Usage Monitor (SRUM) monitors a bunch! 

What we’re most interested in is its detailed record of programs and network activity. 

SRUM has a LONG memory compared to some of the other more ephemeral artefacts📜
To put SRUM to forensic work, grab its .DAT file

C:\Windows\System32\sru\SRUDB.dat

To gain extra contextual data, we're advised to also collect the SOFTWARE hive. 

I didn't do that however, because I am a bad person 😞

We'll leverage one of 
@EricRZimmerman
's brilliant tools to parse out the .DAT

https://f001.backblazeb2.com/file/EricZimmermanTools/SrumECmd.zip

And we can simply execute with : `.\SrumECmd.exe -f .\SRUDB.dat --csv .

You should get a bunch of CSV files 

I tend to prioritise the following ones:
- SrumECmd_NetworkUsages_Output.csv
- SrumECmd_AppResourceUseInfo_Output.csv
- SrumECmd_Unknown*_Output.csv (occasionally)

But maybe you'll find use from the others?

S͟r͟u͟m͟E͟C͟m͟d͟_N͟e͟t͟w͟o͟r͟k͟U͟s͟a͟g͟e͟s͟_O͟u͟t͟p͟u͟t͟.c͟s͟v͟

When tidied up has some cool fields. Most noteworthy in the orange box are the network bytes IN/OUT

If you’re looking for possible indicator of C2 or data exfil, try this:

Convert these columns into a graph. I'd separate graphs for bytes in/out, initially. 

You can then click on these points in the graph, and it will highlight the EXE back in your table.

S͟r͟u͟m͟E͟C͟m͟d͟_A͟p͟p͟R͟e͟s͟o͟u͟r͟c͟e͟U͟s͟e͟I͟n͟f͟o͟_O͟u͟t͟p͟u͟t͟.c͟s͟v͟ 

We can use it to see programmes more / less resource intensive. 

Maybe it will snitch on coin miners using a lot of resource, or quiet backdoors using fewer.
