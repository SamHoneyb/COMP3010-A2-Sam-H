<img width="675" height="398" alt="image" src="https://github.com/user-attachments/assets/8a5c9cf6-d10c-41e5-bdad-f68a1678b3f0" />

GitHub
https://github.com/SamHoneyb/COMP3010-A2-Sam-H

# 1. Introduction
Security operations centres (SOCs) are vital in the modern world of cyber security. They are a cyber security team which monitors an organisations IT infrastructure 24/7 with the aim of finding, analysing and responding to potential security incidents as they happen. [1] This helps to improve companies’ response time to cyber events and prevent future attacks as the monitoring can find the weak points in the infrastructure that cyber criminals are trying to exploit.
This report will cover the investigation of an event which happened in the BOTSv3 dataset, a publicly available and pre-indexed security dataset with many different security events such as DDoS attacks. This report will specifically focus on a cloud misconfiguration event which could have led to data exposure where a S3 bucket was made public for anyone to interact with.
The analysis of this event will be done with Splunk as the primary security and event management (SIEM) solution and will focus exclusively on cloud related events and work on the assumption that the business is a medium sized company with less than 100 employees working for them.

# 2. SOC roles and incident handling reflection
Within a SOC the analysts are generally split by tiers, these tiers range from tier 1 through to tier 3. The lowest of the 3 tiers is tier 1 which is generally considered the easiest role to perform and requires the least amount of experience to get a role within. The general responsibilities of a tier 1 SOC analyst is to monitor the alerts and traffic on the network to look for any issues or flags which may occur, when they see these errors such as an antivirus flag or an Intrusion detection/ prevention systems rule being breached they need to sort them by priority and if necessary escalate them to the tier 2 analysts. [2]

The tier 2 analysts are the next step up from the tier 1 analysts their role is to review higher priority security incidents which have been escalated to them via the tier 1 analysts and perform a more in-depth analysis on them discovering the affected systems and coming up with a plan to contain and bounce back from the incident. [3]

Finally tier 3 analysts actively hunt for threats trying to discover areas where a network may be weak through things like penetration testing and they aim to suggest improvements to the networks current security to prevent future attacks. In the event a tier 2 analyst can’t handle an issue they will be dealt with by a tier 3 analyst. This tier also must investigate the alerts and other data provided to them by tier 1 and 2. [2]

In context of the BOTSv3 scenario, the tier 1 analysts would generally monitor the infrastructure looking for suspicious activity such as API calls being made without multi factor authentication or suspicious changes to the S3 buckets access controls, when they noticed something like this happening they would then escalate it to  the tier 2 analyst who would look deeper into the incident trying to discover the involved user and as well as the effected systems, such as which S3 bucket was effected and whether anything was uploaded to the bucket in the time that it was public. Finally, the tier 3 analyst would investigate the potential ramifications for the business and recommend setups which could be implemented in the future to prevent incidents like this happening again such as setting up more strict user settings and mandatory multi factor authentication [4]. 

# 3. Installation and data preparation
The primary tool used within the SOC is Splunk, this is due to its centralised nature offering many different threat detection and analysis tools on one platform, it allows for you to search through data on the network as well as write rules which will send alerts when certain events happen among many other things. Being centralised makes it easier to track threats occurring on a network as the analysis isn’t split across multiple different platforms, it can also help to save time as analysts aren’t constantly swapping between platforms. [5] It offers many different tools to automate threat detection such as risk-based alerting, which categorises threats only when they reach a certain level, this can help to reduce alert fatigue, increase true positives and allow for the SOC analysts to spend more time threat hunting. [6] [7] All these tools are critical to improving the overall efficiency of a SOC and help to improve the overall security of the infrastructure.

The most recent version of Splunk will be deployed (10.0.2) as a standalone instance for this investigation as it offers more features than previous versions and improves upon security at the time of installation [8]. The instillation process was successful and tested prior to indexing the data set as can be seen in the bellow screenshots.
<img width="994" height="559" alt="image" src="https://github.com/user-attachments/assets/8c501934-27b2-4578-9c6e-31bfdaa9bebf" />
<img width="940" height="528" alt="image" src="https://github.com/user-attachments/assets/03018d68-4919-404e-b2dd-abc17b1ee2f9" />
These screenshots show the instilation of splunk.
<img width="951" height="532" alt="image" src="https://github.com/user-attachments/assets/e0fb9a0f-5cfa-431e-b709-638bb85054f3" />
This screen shot shows navigating to the Splunk folder and starting Splunk
<img width="940" height="528" alt="image" src="https://github.com/user-attachments/assets/b5f9dc4b-849d-4964-9f7b-560d7d9c32cf" />
The above screenshot shows Splunk running before indexing any data.

The next step was to download the dataset from the BOTSv3 Github and the unzip it. Following that it was placed into the appropriate folder where it could then be indexed from the earliest time possible to ensure that all of the data was available for analysis. This can be seen in the bellow screen shots.
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/16cb8e9b-0111-49f5-b2d8-961da2d467a1" />
The above screenshot shows the data being copied into the appropriate folder.
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/eb942b26-3bf1-4516-a8d1-cc2d96977628" />
The above screenshot shows the data being indexed.

The accuracy of the data was validated by looking at the number of events captured which hits the target 2,083,056 events. This validation step is critical in a SOC context as if an analysis was being coducted on incomplete data it could lead to an innacurate analysis and missed vulnerabilities or threats to the infastructure.

# 4. Guided Questions
The incident occurred 14:01:46 on the 10/08/2018 where an S3 bucket was accidentally misconfigured to public and the returned to private just under an hour later at 14:57:54, see below. 
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/b2b16259-e674-4d41-8534-ad6648984f74" />
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/b5f55d80-4cf4-4669-82cd-dbd377a24a62" />
To begin with a level 1 SOC analyst was looking into which users had accessed or attempted to access amazon web services on the network, in a SOC context this gives a good indication of whether any suspicious people had been using amazon web services. 
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/09ecad01-a8e8-4bb7-8ca0-83fb89ceda44" />
Above is a screenshot showing the users, individually this shows no suspicious activity however these names became critical later with one user appearing frequently.
After this the level 1 analyst investigated events completed without multi factor authentication, this can show suspicious activity which may have occurred on the network and may not have been done by the user who was logged on, for example if there was a credential leak. You can see these events in the screenshot below.
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/8e4d1737-1d8a-490d-89bf-2271519517fc" />
And further confirm this by expanding the events and looking for userIdentity.sessionContext.attributes.mfaAuthenticated = false within that event. 
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/15f15438-e5ee-4131-9e67-a40440d83c15" />
In a SOC context findings like this coupled with other events frequently leads to escalation.

The level 1 analyst then investigated the processers being used on the web servers, this allows for the analyst to familiarise themselves with the hardware on the network which is critical in a SOC environment, although not a direct indicator of attack it can be helpful in incident impact assessment and assisting higher tier SOC analysts in the event of escalation especially in performance related issues. 

<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/aaa92d26-95a4-4180-9c52-75e885e62a9f" />
Further investigation into potential S3 bucket misconfigurations was then carried out by searching for *PutBucketAcl* events within the CloudTrail source type showing events which change the buckets access control list.
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/a171ba5a-1b78-480f-b533-fe0f99437e2a" />
Here an event can be seen where the bucket was put to public viewing with the event ID of ab45689d-69cd-41e7-8705-5350402cf7ac.
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/03411f35-ca5e-4782-9cd3-662ad232bc12" />
Below the screenshot can be seen of the access control list being changed to all users.
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/a4380105-1ce1-40bb-9522-b15c692f2131" />
Upon discovery this issue was escalated to a level 2 analyst due to the potential security ramifications. 

The level 2 analyst furthered the investigation into the exposure of the bucket by discovering who and what was involved. The user “BSTOLL” made the change to the frothlywebcode S3 bucket originating from the IP address 107.77.212.175, the change was made without using MFA increasing the chance that this wasn’t an intentional change and that credentials could potentially have been compromised. Within a SOC context this information is vital as it enables response such as restricting accounts credentials being changed. Below are the screenshots containing the above information.
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/503ded71-59e4-452c-881a-7e2a974bb32d" />
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/226fdf12-15ed-479b-8b40-d9b9887b0d9b" />
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/68e6f42a-3dec-4ecc-b33f-2d8fa5b35b68" />
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/4709354a-030f-4cc7-a113-e0c507b90e29" />
The level 2 analyst then investigated potential file uploads to the bucket when it was set to public by searching for PUT events on the frothlywebcode bucket. 2 uploads were found, the first was a text file called OPEN_BUCKET_PLEASE_FIX.txt, which didn’t appear to be suspicious the second was a tar.gz file called frothly_html_memechached which was a much more suspicious file upload due to compressed files abilities to carry malicious payloads. Determining whether data has been uploaded to a S3 bucket is essential for assessing an incidents potential impact in a SOC context. 
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/de70a1c2-5dd1-4c20-9556-14f7626cdcf8" />
The 2 files uploaded can be seen in the below screenshot.
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/b3e1e5ea-2dde-4fd4-85a6-3a350ae4f251" />
After this the level 2 analyst looked further into what potentially could have come from the uploaded file, this was done by investigating which endpoints were running different operating systems, this helps with vulnerability assessments in the future. Analysis revealed that BSTOLL was running a different OS than the other hosts on the froth.ly FDQN. Although this is not a direct indicator of attack it would be worth further analysis in a SOC context. This can be seen below.  
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/53b555d4-505e-49d9-9f3f-39c42be6be39" />
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/bea129f2-1f62-4543-a853-b6c387dee249" />
The FDQN appears consistently throughout the dataset, it can be seen by filtering for sysvol as well as just under generic events.
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/579392ca-f56b-4c67-8adf-67fa56f1f9aa" />
Given the suspicious file upload, the level 2 analyst investigated potential indicators of cryptojacking which is commonly delivered via tar.gz files. DNS logs were analysed looking for queries containing the key word coin. This revealed frequent resolutions to coinhive domains by both users BSTOLL and MKREAUS. Prior to its deprecation coinhive was widely used by crypto mining malware, [9] and with 21 requests being made within under a minute it is unlikely that this is just general browser activity by the user. On top of this frequent GET requests were made for the file even after the bucket was put back to private. These events can be seen in the below screenshots.  
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/c2593693-3033-448c-9ae1-5edbadcd7e1d" />
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/a614f9f7-e751-4831-9dd2-8fa266a89b5c" />
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/15f564a5-4da8-4baf-9770-16cd7cb754a1" />
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/aa7bfd5c-5db8-4baa-ac97-cc7c0de2d8fe" />
Taking these finding together, it is likely that during the period that then S3 bucket was public a suspicious archive was uploaded and then retrieved multiple times with users showing behaviour consistent with cryptojacking. Within a SOC context this would usually result in containment of the infection as well as an escalation to tier 3 to consider further investigation and post incident remediation as well as prevention methods. 
# 5. Conclusion 
At 14:01:46 on the 10/08/2018 an incident occurred where an S3 bucket was put to public allowing for a suspicious file to be uploaded which likely led to a cryptojacking malware being downloaded onto the network. Within the context of the business this incident could have significant implications as first the malware could lead to systems shutting down due to overconsumption of processing power or through containment in the response to the malware. [10] Having these systems shut down can lead to a loss of money to the company as they may not be able to operate properly. Secondly the company could suffer a reputation and financial hit through GDPR as depending on what was stored on the S3 bucket sensitive information could have been leaked to the public as it is already known unconfirmed people have accessed the bucket. 
From a further improvement point of view, one of the key takeaways from this incident is that MFA should be implemented on all networks. It is possible that the breach occurred because MFA wasn’t present so by including it you can help to prevent things like this happening as even if there are a credential leak people won’t be able to access the account without authenticating it’s them, upon receiving an unusual MFA code users can then report the incident to the SOC. [11] On top of this principle of least privilege would also be a helpful thing to implement as it will reduce the amount of people which can make changes to the S3 bucket if they don’t need to and helps minimise damage in the even of a credential leak. [12] 
Finally, in a SOC context, this event shows the importance of having strict detection rules to help speed up response times, the bucket was set to public for almost an hour so writing rules to detect changes to bucket permissions can help to reduce the time that mistakes like this are allowed to be abused. Also looking at the cryptojacking situation rules to detect common features of these attacks would be helpful such a detecting high CPU usage and spikes in DNS activity. Overall, with some system changes and increased alerts the network security and SOCs response can be greatly improved.   

# References
[1] 	Mark Scapicchio et al, “What is a security operations center (SOC)?,” IBM, N.d.. [Online]. Available: https://www.ibm.com/think/topics/security-operations-center. [Accessed 23 12 2025].
[2] 	Paloalto Networkd, “Security Operations Center (SOC) Roles and Responsibilities,” Paloalto Networkd, N.d.. [Online]. Available: https://www.paloaltonetworks.co.uk/cyberpedia/soc-roles-and-responsibilities. [Accessed 23 12 2025].
[3] 	Connect Wise, “SOC Analyst Tiers 1, 2, and 3 - what's the difference?,” Connect Wise, N.d.. [Online]. Available: https://www.connectwise.com/cybersecurity-center/glossary/tier-1-vs-tier-2-vs-tier-3-cybersecurity. [Accessed 23 12 2025].
[4] 	Amazon web services, “Blocking public access to your Amazon S3 storage,” Amazon, N.d.. [Online]. Available: https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html. [Accessed 23 12 2025].
[5] 	Riversafe, “How Splunk helps organisations improve cybersecurity observability,” Riversafe, N.d.. [Online]. Available: https://riversafe.co.uk/resources/how-splunk-helps-organisations-improve-cybersecurity-observability/. [Accessed 27 12 2025].
[6] 	J. Dominguez, “Laying the Foundation for a Resilient Modern SOC,” Splunk, 7 12 2023. [Online]. Available: https://www.splunk.com/en_us/blog/security/laying-the-foundation-for-a-resilient-modern-soc.html. [Accessed 27 12 2025].
[7] 	J. Bull, “Implementing risk-based alerting,” 1 12 2025. [Online]. Available: https://lantern.splunk.com/Security_Use_Cases/Threat_Investigation/Implementing_risk-based_alerting. [Accessed 27 12 2025].
[8] 	Splunk, “Welcome to Splunk Enterprise 10.0,” Splunk, 14 11 2025. [Online]. Available: https://help.splunk.com/en/splunk-enterprise/release-notes-and-updates/release-notes/10.0/whats-new/welcome-to-splunk-enterprise-10.0. [Accessed 27 12 2025].
[9] 	B. Krebs, “Who and What Is Coinhive?,” Krebsonsecurity, 26 4 2018. [Online]. Available: https://krebsonsecurity.com/2018/03/who-and-what-is-coinhive/. [Accessed 28 12 2025].
[10] 	I. S. Josh Schneider, “What is cryptojacking?,” IBM, N.d.. [Online]. Available: https://www.ibm.com/think/topics/cryptojacking. [Accessed 29 12 2025].
[11] 	National Cyber Secuity Center, “Multi-factor authentication for your corporate online services,” National Cyber Secuity Center, N.d.. [Online]. Available: https://www.ncsc.gov.uk/collection/mfa-for-your-corporate-online-services/why-mfa-matters. [Accessed 29 12 2025].
[12] 	N. Vaideeswaran, “Intro to The Principle of Least Privilege (POLP),” CrowdStrike, 8 1 2025. [Online]. Available: https://www.crowdstrike.com/en-gb/cybersecurity-101/identity-protection/principle-of-least-privilege-polp/. [Accessed 29 12 2025].

# Appendix 
## Student Declaration of AI Tool use in this Assessment

<img width="544" height="759" alt="image" src="https://github.com/user-attachments/assets/e68ed12b-a825-4011-8e00-982e3cccfd58" />







































