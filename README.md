<img width="675" height="398" alt="image" src="https://github.com/user-attachments/assets/8a5c9cf6-d10c-41e5-bdad-f68a1678b3f0" />

GitHub
https://github.com/SamHoneyb/COMP3010-A2-Sam-H

1. Introduction
Security operations centres (SOCs) are vital in the modern world of cyber security. They are a cyber security team which monitors an organisations IT infrastructure 24/7 with the aim of finding, analysing and responding to potential security incidents as they happen. [1] This helps to improve companies’ response time to cyber events and prevent future attacks as the monitoring can find the weak points in the infrastructure that cyber criminals are trying to exploit.
This report will cover the investigation of an event which happened in the BOTSv3 dataset, a publicly available and pre-indexed security dataset with many different security events such as DDoS attacks. This report will specifically focus on a cloud misconfiguration event which could have led to data exposure where a S3 bucket was made public for anyone to interact with.
The analysis of this event will be done with Splunk as the primary security and event management (SIEM) solution and will focus exclusively on cloud related events and work on the assumption that the business is a medium sized company with less than 100 employees working for them.


