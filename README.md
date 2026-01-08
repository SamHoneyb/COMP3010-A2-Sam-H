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








