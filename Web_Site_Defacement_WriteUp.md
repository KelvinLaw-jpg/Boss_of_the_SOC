# Web Site Defacement Write Up

Scenario: 

Today is Alice's first day at the Wayne Enterprises' Security Operations Center. Lucius sits Alice down and gives her first assignment: 
A memo from Gotham City Police Department (GCPD). Apparently GCPD has found evidence online (http://pastebin.com/Gw6dWjS9) that the website
www[.]imreallynotbatman[.]com hosted on Wayne Enterprises' IP address space has been compromised. The group has multiple objectives... 
but a key aspect of their modus operandi is to deface websites in order to embarrass their victim. Lucius has asked Alice to determine if www[.]imreallynotbatman[.]com. 
(the personal blog of Wayne Corporations CEO) was really compromised.

## Write up

1. Check all indexes and endpoints to see the scope we are dealing with. (| eventcount summarize=false index=*), (| metadata type=sourcetypes | fields sourcetype)
2. Since the first Q is asking an ip for scanning, it is likely we will find the answer in firewall based logs or the end point, EDR, XDR
3. first, limit the time frame
4. search (index=botsv1 sourcetype=fgt* "domainname")
5. look for traffic for this domain IP (index=botsv1 sourcetype=stream:http "IP")
6. Do reconnaissance from attack IP
