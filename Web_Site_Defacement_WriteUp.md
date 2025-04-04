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
7. If there was an incident/event happened, what is the characteristics of this event and what logs would it be associated with?


## Write Up

![overview](images/01_IndexSummary.png)

1. Check all indexes and endpoints to see the scope we are dealing with. (| eventcount summarize=false index=*), (| metadata type=sourcetypes | fields sourcetype)

Looking at the summary we are dealing with 7 indexes here. 

![st](images/02_sourcetype.png)
![st](images/03_useragent.png)
![st](images/04_srcIP.png)
![st](images/05_2attackIP.png)
![st](images/06_victimIP.png)
![st](images/07_AlertCat.png)
![st](images/08_AlertSig.png)
![st](images/09_VictimIP.png)
![st](images/10_httpRefer.png)
![st](images/11_Request.png)
![st](images/12_URL.png)
![st](images/13_AttackerIP.png)
![st](images/14_formData.png)
![st](images/15_maliciousexe.png)
![st](images/16_MaliciousHash.png)
![st](images/17_MaliciousHash.png)
![st](images/18_DomainEmail.png)
![st](images/19_Password.png)
![st](images/20_correctpassword.png)
![st](images/21_Gettingmeanpwlen.png)

![st](images/23_vbsLeng.png)
