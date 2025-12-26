# COMP3010-CW2
COMP3010 – CW2 

Student: Alvin Mparutsa.

Module: COMP3010 Security Operations & Incident Management.

1.Introduction:

This report is used to document an investigation carried out in Splunk, This is done using the BOTSv3 dataset. The dataset is used to simulate a security scenario within a fictional organisation. 
Objectives:
-	The objectives of this investigation were to download and set up a working Splunk environment on Ubuntu and validate the dataset
-	Use the Splunk to help answer the question set and each answer should be supported with evidence
-	Present the findings as a SOC-style report which is led by the evidence.

Evidence: 

Question 1:

Step 1 – Opened Splunk search and reporting section and confirmed that the dataset was available. Checked the botsv3 index contained events by setting the range to ALL time so that any missing older vlogs are avoided. 

Step 2 – Filtered the dataset to AWS CloudTrail logs. This was done by searching in the BOTS index and specifying the CloudTrail source type.

Step 3 – Restricted results to IAM user activity. This was done by filtering events where the user identity type equals IAM user. This makes sure results only have actions done by IAM users.

Step 4 – Extracted the usernames to collect the unique IAM usernames that appeared in the cloud trail events.

Step 5 – Formatted the output to align with the requirements of the quiz.

Question 2:

Step 1 – Excluded the console login events by “NoteventNmae=ConsoleLogin” this made the results only cover API activity and not sign ins.

Step 2 – a keyword search was used for MFA related fields, this is so that fields containg mfa context would be returned.

Step 3 – opened the events in order where the log shows False.

Step 4 – Recorded the full JSON path for the field using the order that I opened the event. 

Question 3:

Step 1 – First checked what source types were available in the dataset by using the source count query. This then confirmed there is hardware source type available.

Step 2 – I checked which web hosts have hardware inventory data, by searching the hardware source type and counted events by host. This then confirmed which hosts actually have hardware records available in the dataset.

Step 3 – I then clicked one of the “gaccrux” hardware events and clicked view events and show more which revealed the processor number used on the web server under the CPU_TYPE field.


