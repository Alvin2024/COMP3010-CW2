# COMP3010-CW2
COMP3010 – CW2 

Student: Alvin Mparutsa.

Module: COMP3010 Security Operations & Incident Management.

# 1.Introduction:

This report is used to document an investigation carried out in Splunk, This is done using the BOTSv3 dataset. The dataset is used to simulate a security scenario within a fictional organisation. This coursework is set in a SOC (Security Operations Centre) context using the dataset in Splunk as a realistic pre-indexed set of windows, network and security telemetry that stimulates normal enterprise activity which is mixed with attacker behaviour. The aims are to show that I can, set up a working SIEM environment. Ingest and validate security data and use Splunk searches to answer investigation questions in such a way that reflects real SOC workflows and decision making.

## Objectives:

-	The objectives of this investigation were to download and set up a working Splunk environment on Ubuntu.
-	Ingest and validate the BOTSv3 dataset and confirming that the key indexes and source types are searchable 
-	Use the Splunk to help answer the question set and each answer should be supported with evidence
-	Present the findings as a SOC-style report which is led by the evidence.

My investigation environment is Linux host which I ran Splunk enterprise on with the dataset installed as an add on and validated through successful search results in Splunk. The installation evidence screenshots show the key setup process and stages. Downloading the dataset, installing splunk via package installation, starting Splunk and confirming web service on port 8000, setting credentials, restating Splunk and confirming the dataset is available after setup, I used Splunk search to run the targeted queries against the botsv3 index and relevant source types to build findings from host telemetry.

# Scope and assumptions:

-	The analysis is limited to events contained within the BOTSv3 dataset, so findings represent what is observable in those logs and not a live environment.
-	The time range is set to all time within Splunk to avoid missing historical events.
-	Host and domain fields may not be present consistently across source types, so the approach includes validating available fields and using the most reliable identifiers such as host, Computer Name, DND Domain, Domain) where present.
-	The focus is on SOC-relevant outcomes: identifying anomalies, validating endpoints/data sources, and producing defensible evidence (queries + output) rather than purely “getting the answer”.

Overall, the objective is to demonstrate the full workflow: SEIM setup to dat validation to investigation searches to evidence backed conclusions aligned with SOC operations and incident handling practice. 

# SOC Roles and Incident Handling Reflection:
A SOC’s job is to turn raw telemetry into action. In practice this is split across different tiers and roles, The BOTSv3 exercise maps well how a real SOC would work because it forces the same habits: 
-	Validating data sources 
-	Scoping an investigation 
-	Pivoting between hosts and source types 
-	Documenting evidence

## How the SOC tiers map to what I did:

## Tier 1 (Triage/Monitoring):

Tier 1 analysis main focus is on the “what looks unusual” aspect of things and “do we have enough data to investigate?” in my work, the equivalent step was confirming telemetry coverage ( e.g. verifying event volume and identifying which source types exist for a host). A good example is using broad searches and simple stats to confirm that the logs exist and are searchable before trying to draw conclusions. This is exactly what Tier 1 should do to avoid false negatives caused by missing data.

## Tier 2 (Investigation/Analysis):

Tier 2 takes a triage signal and turns it into a defensible finding. My approach for question 8 is a clear example I began from the hint, endpoint OS information from winhostmon. Then counted OS editions across endpoints and found the odd one out running a different windows edition. Then pivoted into the specific host and used telemetry fields to derive the FQDN. This mirrors a real workflow:

1.	Spot an anomaly 
2.	Confirm its real 
3.	Pivot to enrich the identity of the asset
4.	Document the trail
   
## Tier 3 /Engineering (Detection and Content):

While in my work I wasn’t writing production detections, parts of the workflow reflect Tier 3 thinking such as validating which fields exist, adjusting searches when a field isn’t populated as expected, and using safe logic. For example combining potential domain fields. To make the query resilient. This is the difference between a one-off search and something that would work reliably.

# Incident handling phases and SOC relevance:

## Preparation:

The installation and dataset validation steps are preparation in an incident response sense. Without reliable ingestion a SOC can’t effectively respond. From correct time ranges, working authentication and access.

## Detection and Analysis:

The Splunk searches represent the detection/analysis phase. Counting OS editions by host is lightweight detection as this can reveal misconfigurations, unmanaged builds, or suspicious drift. Inconsistent endpoint baselines are a common precursor to security gaps.

## Containment/Eradication/Recovery (Reflection). 

In a real SOC, once the outlier is identified (by FQDN), the next steps would be to check whether it is authorised, validate patch level and security tooling presence, review authentication events, and apply containment if there are signs of compromise. Even when the task only wants identification, thinking in IR phases keeps the outcome actionable, not just descriptive.

## What I learned:

The biggest takeaway is that good investigations are as much about data quality and method as they are about the answer. Things like screenshots, query outputs, data source, how the anomaly was identified and endpoint identity confirmation show reasoning chain and are the standard expected in SOC work. repeatable steps, evidence-backed conclusions, and clear communication that a non-technical stakeholder can trust. 


# Evidence: 

## Question 1:

Step 1 – Opened Splunk search and reporting section and confirmed that the dataset was available. Checked the botsv3 index contained events by setting the range to ALL time so that any missing older vlogs are avoided. 

Step 2 – Filtered the dataset to AWS CloudTrail logs. This was done by searching in the BOTS index and specifying the CloudTrail source type.

Step 3 – Restricted results to IAM user activity. This was done by filtering events where the user identity type equals IAM user. This makes sure results only have actions done by IAM users.

Step 4 – Extracted the usernames to collect the unique IAM usernames that appeared in the cloud trail events.

Step 5 – Formatted the output to align with the requirements of the quiz.

## Question 2:

Step 1 – Excluded the console login events by “NoteventNmae=ConsoleLogin” this made the results only cover API activity and not sign ins.

Step 2 – a keyword search was used for MFA related fields, this is so that fields containg mfa context would be returned.

Step 3 – opened the events in order where the log shows False.

Step 4 – Recorded the full JSON path for the field using the order that I opened the event. 

## Question 3:

Step 1 – First checked what source types were available in the dataset by using the source count query. This then confirmed there is hardware source type available.

Step 2 – I checked which web hosts have hardware inventory data, by searching the hardware source type and counted events by host. This then confirmed which hosts actually have hardware records available in the dataset.

Step 3 – I then clicked one of the “gaccrux” hardware events and clicked view events and show more which revealed the processor number used on the web server under the CPU_TYPE field.

## Question 4:

Step 1 – Restricted the search to AWS cloud trail logs so that I was only looking at API audit events.

Step 2 – I searched specifically for API that are able to make a bucket public by filtering the S3 ACL change action.

Step 3 – Located the events and scrolled down to eventide and copied and pasted. First ID was incorrect but second which I highlighted in the screenshot was correct.

## Question 5:

Step 1 – I had gone back to question 1 where I had restricted the search to user activity and extracted user names. That is where I found Buds username.

## Question 6:

Step 1 – Restricted the dataset to Cloud Trail events by using “aws:cloudtrail” as the source type as well as filtering specifically for the S3 persmission change by using “eventName=PutBucketAcl”

Step 2 – I extracted nested cloud trail JSON fields by using “spath” to ensure the nested fields were parsed correctly and accessible in the results.

Step 3 – I used the “table” command to display only the required fields to help answer the question. Which made it easier for me to read the bucket name without having to dig through the full raw event.

Step 4 – The bucket name was displayed under “requestParameters.bucketName”  which I entered.

## Question 7:

Step 1 -  Searched the S3 access logs dataset. The keyword “frothlywebcode” was used to filter the results to requests that are related to that bucket.

Step 2 – This search confirmed the bucket name is appearing in events and helped as somewhat of a sanity check however the search was too broad and showed several results.

Step 3 – Made a tighter search to isolate upload events and extract the filename. I added “REST.PUT.OBJECT” which will identify object upload (PUT) operations.

Step 4- This reduced the results and only displayed events where there was an attempt to upload something to the bucket.

Step 5 – Checked the HTTP  status code to look for successful uploads the code being 200 and 2 of the events had the code. I then copied and pasted the filenames until the highlighted one in the screen shot “OPEN_BUCKET_PLEASE_FIX.txt.” came up as the correct filename.

## Question 8:

Step 1- Confirmed the right data source and useful fields by using the source type winhostmon and looking for OS related keywords. This then helped confirm that the source type contains operating system telemetry and helped narrow the fields to focus on. 

Step 2- Finding the odd one out by pivoting the operating system records and summarising OS host using a stats view. The results the show most endpoints are Microsoft windows 10 pro except BSTOLL-L which is Microsoft windows 10 enterprise. This then shows BSTOLL-L as the endpoint using a different windows edition.

Step 3 – After finding BSTOLL-L, I Identified where FQDN is visible in other sources by  checking what other source types contained relevant naming data done by counting events by host/source type. This showed syslog data available via “host=splunkhwf.froth.ly” which is useful because it includes host naming details. 

Step 4 – Clicked on view event and the events display the endpoint as “BSTOLL-L.froth.ly”. This then provides the required FQDN.

