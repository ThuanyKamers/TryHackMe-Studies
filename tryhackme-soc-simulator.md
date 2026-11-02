# TryHackMe - SOC Simulator

## Objective
Close all true positive alerts.
- Monitor and analyze real-time alerts
- Identify and document critical events such as suspicious emails and attachments
- Create detailed case reports based on your observations to help your team understand the full scope of alerts and malicious activity

## Task: Alert 8815, suspicious Amazon email
When the SOC Simulator starts, this is what I see:

![SOC Simulator dashboard for "Introduction to Phishing", showing 0 total alerts, 0 closed alerts, 0 closed as TP and 0 closed as FP](images/image-67.png)

I go to the Alert Queue, but there's nothing there yet.

![Alert queue page, with a message saying I haven't picked up any alert yet](images/image-68.png)

So I open the documentation to read it.

![Documentation page, Welcome tab, explaining the SOC Simulator and linking to the Introduction to Phishing room](images/image-69.png)

In the documentation I find the first step-by-step guide, so I follow it.

![Alert triage tab of the documentation, listing the Alert Triage Playbook: Initial Alert Review, Investigate in the SIEM, Resolution and Closure](images/image-70.png)

There are 2 alerts, so I open the most recent one.

![Open alerts list showing two "Inbound Email Containing Suspicious External Link" alerts, IDs 8815 and 8814, both Medium severity](images/image-71.png)

![Alert queue showing alerts 8815 and 8814 awaiting action, with 3 more alerts incoming](images/image-72.png)

8815 is the most recent one, so I click the little person icon with a + to assign it to myself and read the description.

![Assigned alert 8815 detail, "Inbound Email Containing Suspicious External Link", showing the full email: sender urgents@amazon.biz, recipient h.harris@thetrydaily.thm, subject "Your Amazon Package Couldn't Be Delivered"](images/image-73.png)

The alert came from a suspicious link in an email, which looks like a phishing attempt. The first thing that catches my attention is the sender address: it says urgent, and after the @ there's amazon.biz. What is that .biz doing there? It should normally be .com.

The email was sent to h.harris@thetrydaily.thm.

It also catches my attention that the body starts with "dear customer" instead of the actual client's name. Urgency is another common phishing trait, which shows up both in the sender address and in the body, warning that the package will be returned if the recipient doesn't click the link within 48 hours.

I click the Playbook link, which opens this page:

![Phishing Email Analysis playbook diagram: Assign Alert, Alert Triaging Process, Analyze Email Artifacts (Check Sender Reputation, Review Attachments), Investigate Related Alerts, Case Reporting](images/image-74.png)

Now I understand what my next steps are.

Back in the documentation, it says to open the SIEM.

![Sidebar navigation showing Dashboard, Alert queue, SIEM, TryDetectThis, and Documentation highlighted](images/image-75.png)

Clicking on SIEM opens this page:

![Splunk Enterprise search page, 87 of 87 events matched, showing a list of raw security events](images/image-76.png)

Back in the Alert Queue, I see the alert happened at 5:06pm, so I search the SIEM for this email around that time, and find it.

![Splunk event at 5:06:18.543 PM matching the alert: the Amazon package phishing email, sender urgents@amazon.biz, recipient h.harris@thetrydaily.thm](images/image-77.png)

I open TryDetectThis, as the documentation suggested, and see I can analyze links, IPs and files there. I go back to the alert and copy the link.

![Zoomed in email content showing the bit.ly shortened link in the body of the Amazon phishing email](images/image-78.png)

![TryDetectThis URL/IP Check page with the bit.ly link pasted in, ready to analyze](images/image-79.png)

The result comes back malicious, as I suspected.

![TryDetectThis result: URL/IP Analysis Complete, Status: MALICIOUS](images/image-80.png)

The documentation says I should correlate the alert data with other data sources to validate its credibility, but I'm still not sure how to do that.

Now I need to click write case report on the alert and follow the alert reporting documentation.

![Alert Reporting tab of the documentation, listing what a case report should include and best practice report examples](images/image-81.png)

![Alert Classification tab of the documentation, listing when alert escalation is and isn't required](images/image-82.png)

The documentation says phishing isn't escalated if it was quarantined before users had a chance to access the malicious email. I don't know yet whether the user opened it, but I think I can check that in the SIEM. I search again from 5:06pm onward and find no access to the phishing site from this email.

I mark the case report as True positive.

![Blank incident report form for 8815, with fields for incident classification, case report and escalation](images/image-83.png)

The incident data is shown above.

![Case report summary for event ID 8815, showing the alert rule, description, incident type Phishing, severity Medium, and detection time Nov 2nd 2026 at 18:08](images/image-84.png)

It's strange that the detection time says 6:08pm, more than an hour after the actual activity. That must mean the system takes a while to detect this kind of phishing.

I fill out the report this way:

![Incident report for 8815 filled in: Time of activity 17:06, Reason for Classifying as True Positive explaining TryDetectThis flagged the link as malicious and the phishing characteristics in the email, escalation set to No](images/image-85.png)

I'm still not entirely sure how to fill everything in. Even with the documentation open, it wasn't very clear to me.

I click submit and close alert.

![Confirmation screen: "Nice work! You closed your first alert."](images/image-86.png)

I continue the training.

## Task: Alert 8816, blocked firewall connection
I check the Alert queue.

![Alert queue showing 8818, 8817, 8816 and 8814 awaiting action, and 8815 closed](images/image-87.png)

I pick up the one with high severity.

![Assigned alert 8816 detail, "Access to Blacklisted External URL Blocked by Firewall", High severity, showing the same bit.ly URL blocked by the firewall](images/image-88.png)

Right away I notice it's the same link from the previous alert, which means the employee had actually clicked it, something I hadn't picked up on in the SIEM. The timestamp is just 1 minute after the email was received.

I go back to the SIEM and find the access attempt.

![Splunk event at 5:07:32.543 PM showing the firewall blocking the connection to the bit.ly URL](images/image-89.png)

I hadn't caught this before. It went unnoticed, I wasn't paying enough attention.

I click write case report.

![Close alert modal for event ID 8816, with True positive selected and a Write case report button](images/image-90.png)

I mark it true positive, and this time I set it to escalate, since the employee did click the link.

![Incident report for 8816 filled in with escalation set to Yes](images/image-91.png)

Then it occurs to me that it might not need escalation after all, since the incident report says the firewall or proxy blocked the outbound request and prevented the connection. Checking the documentation again, escalation is only needed if a bigger incident resulted from this access, and since it was blocked beforehand, it doesn't apply here. So I switch the escalation back to No.

![Incident report for 8816 with escalation switched to No](images/image-92.png)

With this incident closed, I pick up the next most recent alert.

## Task: Alerts 8818 and 8814, HR onboarding phishing
This one looks like another phishing attempt.

![Assigned alert 8818 detail, "Inbound Email Containing Suspicious External Link", an onboarding email from onboarding@hrconnex.thm to j.garcia@thetrydaily.thm with a profile setup link](images/image-93.png)

There seems to be a way to check the proxy and firewall logs, to see whether there were any access attempts to the link and whether those connections were allowed or blocked. I need to find where to do that.

At first glance, the email looks like it's from an HR employee. The body even mentions the employee's name, which is a point against it being phishing, though not enough to rule it out entirely.

I copy the URL, which is apparently for a training.

![Zoomed in email content showing the hrconnex.thm onboarding link](images/image-94.png)

I try to open TryDetectThis, but the page won't load.

![TryDetectThis page stuck on a loading message](images/image-95.png)

I do a quick Google search.

![Google search for "trydetectthis not opening" with an AI overview suggesting it's a local script loading issue](images/image-96.png)

I manage to get TryDetectThis working through the link, and there's no detection or record of this URL in the tool.

![TryDetectThis result: "No results found, no cached reports found for https://hrconnex.thm/onboarding/15400654060/j.garcia"](images/image-97.png)

I find the email in the SIEM.

![Splunk event at 5:09:04.543 PM matching the onboarding phishing email](images/image-98.png)

The documentation has a list of employee emails, so I look for the sender from this incident, but it isn't on the list.

![Company information tab of the documentation, Employees table with name, department, email, logged-in host and IP address](images/image-99.png)

I can also search TryDetectThis by IP, but I can't find the sender's IP anywhere. Back in the SIEM, there's an arrow I can click on the alert for more detail.

![Splunk event expanded, showing the full field list for the onboarding phishing email](images/image-100.png)

That opens up a lot more information, but I still can't find the IP I need, and I don't fully understand everything that's shown.

I click on sender to see more about the supposed HR email.

![Splunk "sender" field popup, showing top values including onboarding@hrconnex.thm](images/image-101.png)

Clicking on onboarding@hrconnex.thm, the sender behind the supposed HR email, opens another page in the SIEM.

![Splunk search for sender="onboarding@hrconnex.thm", 2 of 3 events matched, showing the same onboarding email at 5:09:04.543 PM and 5:03:05.543 PM](images/image-102.png)

This sender has 2 recorded activities, one at 5:03pm and one at 5:09pm. The 5:03pm email also shows up among the alerts.

![Assigned alert 8814 detail, the same onboarding phishing email sent at 17:03:05.543, still awaiting action](images/image-103.png)

I open the Splunk server and click on the IP.

![Splunk "splunk_server" field popup showing a single value, ip-10-10-40-195, 100% of events](images/image-104.png)

There's nothing there besides the same two emails from the supposed HR sender.

I still can't find the IP, but this email is clearly pretending to be HR: it isn't on the employee list, our real HR has a different address on file, and the same message was sent twice. Based on all of that, I mark it as phishing.

![Incident report for 8818 filled in: List of Attack Indicators noting the email pretends to be HR but isn't in the employee list, and there are 2 identical emails from the same sender](images/image-105.png)

Since the other alert is the same situation, I close it too.

![Assigned alert 8814 detail again, ready to be closed with the same reasoning](images/image-106.png)

## Task: Alert 8817, Microsoft sign-in phishing
The last alert is this one:

![Assigned alert 8817 detail, "Unusual Sign-In Activity on Your Microsoft Account", sender no-reply@m1crosoftsupport.co, recipient c.allen@thetrydaily.thm](images/image-107.png)

This one is obviously phishing right away: the sender address uses @m1crosoft, with a 1 standing in for the i. This time the body does use the employee's actual name, which suggests whoever sent it is a bit more experienced at phishing.

I copy the link to TryDetectThis, but it isn't on the list of known malicious links yet.

![TryDetectThis result: "No results found, no cached reports found for https://m1crosoftsupport.co/login"](images/image-108.png)

The email doesn't show up in the SIEM either, and there's no record of anyone accessing the link.

Based on the phishing characteristics I already identified, I close it as a true positive.

![Incident report for 8817 filled in: List of Attack Indicators noting the sender pretends to be Microsoft, using a 1 instead of an i: m1crosoft](images/image-109.png)

## Task: Reviewing my feedback
After closing all the incidents, I get some feedback:

![True positives summary table: alerts 8818 and 8814 marked Incorrect, alerts 8815, 8816 and 8817 marked Correct, with an AI overall analysis saying the reports are missing key details](images/image-110.png)

As I expected, my incident reports were missing information. Here's the detailed feedback on one of them:

![Incident report for 8815, same as before, shown again alongside the detailed AI feedback](images/image-85.png)

I should have noted that the email was signed "Amazon Delivery" but came from a domain that isn't amazon.com. I mentioned urgency, but didn't spell out that the email threatened to return the package unless the recipient replied within 48 hours. I need to be explicit about the exact mechanisms used to pressure the victim. The link itself is a bit.ly shortened URL, and shortened links hiding the real destination is a red flag I should call out directly. I also need to explain the attacker's actual goal: getting the user to confirm the delivery, or hand over personal information, through that link. The firewall logs matter too: the recipient's workstation tried to open the link, and the firewall blocked the connection, which is exactly what determines whether escalation is needed. On top of that, I should check the sender's email logs to confirm whether anyone else received the same message; in this case, only the one recipient did, which is useful scope information. Overall, I need to cover the 5 Ws properly. I only reported When (17:06), but left out Who was affected (h.harris@thetrydaily.thm), Who sent it (urgents@amazon.biz), What the subject line was, and What the malicious link actually was. These are details every report needs.

I assumed the information already in the alerts would be enough, and that I wouldn't have to write everything out again, but it turns out I do.

The next reports will definitely be better than this one.

I also found out there's a dedicated room for learning this SOC Simulator, focused on phishing, so I'm going to do that one next.

![SOC Simulator: Introduction to Phishing room card, "Play your first SOC Simulator scenario and learn to triage phishing alerts, one alert at a time", 45 min](images/image-111.png)

## What I learned
- A phishing report needs the 5 Ws spelled out explicitly (who, what, when, where, why/how), even when the alert description already contains some of that information. Writing "it was flagged as malicious" isn't enough; the report has to state exactly what was malicious, who sent it, who received it, and when.
- A shortened URL (like a bit.ly link) is itself a red flag worth calling out in a report, because it hides the real destination from the recipient.
- Escalation isn't automatic just because an alert is a true positive. If a control like a firewall or proxy already blocked the malicious connection, the alert can stay closed without escalation, but if the employee actually reached the malicious link, that's a reason to escalate.
- Cross-referencing an email sender against the official employee list is a fast way to catch HR or internal-sounding phishing that doesn't come from a real employee address, especially when the real department already has a different address on file.
- Typosquatted domains (like m1crosoftsupport.co or amazon.biz instead of a .com address) are often enough on their own to classify an alert as phishing, even without a detection hit in a tool like TryDetectThis.
- Case reports are judged on completeness, not just on getting the classification right. Closing an alert correctly as a true positive still scored as "incorrect" when the report itself was missing the required details.
