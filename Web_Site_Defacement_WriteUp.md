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

1. Check all indexes and endpoints to see the scope we are dealing with. (| eventcount summarize=false index=*), (| metadata type=sourcetypes | fields sourcetype)
   
![overview](images/01_IndexSummary.png)

Looking at the summary we are dealing with 7 indexes here. Since we know the botsv1 index is what we will be working with here there isn't much point to check the index list, however, it is good practice. 

2. Since we know the victim domain is "imreallynotbatman.com", we will type a query that shows all related logs.
![st](images/02_sourcetype.png)
4 sourcetype related:
- Suricata: A popular open source IDS & IPS
- Stream : http:  http traffic
- fgt_utm: FortiGate Unified Threat Management solution
- iis: Internet Information Services web server logs

### Q1. What is the likely IPv4 address of someone from the Po1s0n1vy group scanning imreallynotbatman.com for web application vulnerabilities?
### Q2. What company created the web vulnerability scanner used by Po1s0n1vy? Type the company name.

To see who is scanning the website, lets have a look at the user agents that made request.
![st](images/03_useragent.png)
The Acunetix agent is quite interesting, since it is a web sec vulnerability scanner, not a normal agent that people would use. Lets check out the traffic
![st](images/04_srcIP.png)
So looking at the traffic, it is very likely that IP starts with 40 is the attacker IP, however, lets see what other IPs are requesting traffic to our server as well as the IPs related to "www[.]imreallynotbatman[.]com".
![st](images/05_2attackIP.png)
![st](images/06_victimIP.png)
So the above shows the magority of the traffic is going from 40.80.148[.]42 to 192.168.250[.]70. Thus, it is very fair to say that's our attacker and server IP.

![st](images/07_AlertCat.png)
![st](images/08_AlertSig.png)

To confirm that, you can also look at any logs from suricata which shows an attack alert.
### Q3. What content management system is imreallynotbatman.com likely using?
We can look at any http related traffic and you will see the CMS joomla
![st](images/10_httpRefer.png)

### Q.4 What is the name of the file that defaced the imreallynotbatman.com website? Please submit only the name of the file with extension?
### Q.5 This attack used dynamic DNS to resolve to the malicious IP. What fully qualified domain name (FQDN) is associated with this attack?
### Q.6 What IPv4 address has Po1s0n1vy tied to domains that are pre-staged to attack Wayne Enterprises?
Very interesting question, to deface the website, meaning the attacker uploaded a file to the server, or we can say our server GET a file from the attack. Thus, lets use a the query `index=botsv1 src_ip="192.168.250.70" http_method=GET`.
Now lets look at all the request and url send from our server.
![st](images/11_Request.png)
![st](images/12_URL.png)
For Q.6, we will check out the traffic from the FQDN in Q.5
![st](images/13_AttackerIP.png)

### Q.7 What IPv4 address is likely attempting a brute force password attack against imreallynotbatman.com?

Now brute forcing means it's sending a bunch of http POST to our server, if we go back to the original query `index=botsv1 sourcetypes=stream:http imreallynotbatman.com`, and check the IPs, there are 2 only, both of which belongs to the attacker, lets check out the traffic for both.
![st](images/14_formData.png)
looking at the form data, it shows that the BF attack is coming from IP start with 23
### Q.8 What is the name of the executable uploaded by Po1s0n1vy?
since we know it's an exe file, just search the whole index with `.exe`
![st](images/15_maliciousexe.png)

### Q.9 What is the MD5 hash of the executable uploaded?
This is actually a very tricky question as it requires us to know what logs hold the hash of the file, normally network logs will not generate hash of the file. Lets see what logs do we have that is related to the exe with the query `index=botsv1 3791.exe`. 
We see that there is a sysmon log, we might get lucky with sysmon.
![st](images/16_MaliciousHash.png)
![st](images/17_MaliciousHash.png)
and indeed sysmon does hold a hash for us.

### What was the correct password for admin access to the content management system running "imreallynotbatman.com"?

Before solving the question I would like to look at the key fields of a request.
![st](images/19_Password.png)

For this question, we have 2 approach. Logically, if the attacker got the password, he won't be attempting to brute force again, thus, the last password attempt made would be the correct password. However, this method is not bulletproof. 
the second approach is to see what password is being used over 1 times, if a password is used more then once, that it is safe to say that it is the correct password.

Thus, we will use the query `index=botsv1 sourcetype=stream:http dest_ip="192.168.250.70" http_method=POST uri=/joomla/Administrator/Index.php | rex field=form_data "passwd=(?<password>\w+)" | stats count by password | sort -count`

The query says: I want to result from index botsv1 with only http sourcetype with the specific destination address, the request must be POST and is to the page with the log in form. Then I want to extract all passwd used to a field name password, and count the number of times these password are used, and then sort them by putting the largest number at the top.
![st](images/20_correctpassword.png)

The result is batman, used twice.

### What was the average password length used in the password brute forcing attempt?
So to do this, we can use a similar query, first extract all the password, and use the eval, len(), and stats avg() function. Eval() will create a new field called length by using the len() we get the length of each password. we then plug the list of lengths into the avg() and we will get the average length of all passwords.
![st](images/21_Gettingmeanpwlen.png)

