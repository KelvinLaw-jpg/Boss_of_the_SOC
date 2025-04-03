# Web Site Defacement Write Up

Scenario: 

Today is Alice's first day at the Wayne Enterprises' Security Operations Center. Lucius sits Alice down and gives her first assignment: 
A memo from Gotham City Police Department (GCPD). Apparently GCPD has found evidence online (http://pastebin.com/Gw6dWjS9) that the website
www[.]imreallynotbatman[.]com hosted on Wayne Enterprises' IP address space has been compromised. The group has multiple objectives... 
but a key aspect of their modus operandi is to deface websites in order to embarrass their victim. Lucius has asked Alice to determine if www[.]imreallynotbatman[.]com. 
(the personal blog of Wayne Corporations CEO) was really compromised.

## My work flow

It's important to state that every analyst has their own way of working. Thus, there are many ways to the same answer. 
Personally, before answering any CTF questions, I like to play around and be familiarise with what I am dealing with, because
in real world sceneario, there isn't any flag to capture, rather it is more like defending the castle at all times. Therefore,
the more familiar the analyst is to the network and endpoints he/she is dealing with. The better he/she is equiped to spot 
anomalies. 

**Questions I like to answer for myself**
1. What sources and source types are we dealing with?
2. How many endpoints are we dealing with?
3. What type of device and their systems/brands/usage are we dealing with?
4. What types of logs are we dealing with? (Network/Endpoints)
5. What duration is the scope?
6. What department of the company is associated with which endpoints?


## Write Up
1. Check all indexes and endpoints to see the scope we are dealing with. (| eventcount summarize=false index=*), (| metadata type=sourcetypes | fields sourcetype)
2. Since the first Q is asking an ip for scanning, it is likely we will find the answer in firewall based logs or the end point, EDR, XDR
3. first, limit the time frame
4. search (index=botsv1 sourcetype=fgt* "domainname")
5. look for traffic for this domain IP (index=botsv1 sourcetype=stream:http "IP")
6. Do reconnaissance from attack IP
