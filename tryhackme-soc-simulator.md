# TryHackMe - SOC Simulator

**Objective:** Close all true positive alerts.
- Monitor and analyze real-time alerts
- Identify and document critical events such as suspicious emails and attachments
- Create detailed case reports based on your observations to help your team understand the full scope of alerts and malicious activity

When the SOC Simulator starts, I'm met with this:

![SOC Simulator dashboard for "Introduction to Phishing", showing 0 total alerts, 0 closed alerts, 0 closed as TP and 0 closed as FP](images/image-67.png)

I go to Alert Queue, but there's nothing there yet.

![Alert queue page, with a message saying I haven't picked up any alert yet](images/image-68.png)

So I open the documentation to read it.

![Documentation page, Welcome tab, explaining the SOC Simulator and linking to the Introduction to Phishing room](images/image-69.png)

In the documentation I find the first step by step, so I continue.

![Alert triage tab of the documentation, listing the Alert Triage Playbook: Initial Alert Review, Investigate in the SIEM, Resolution and Closure](images/image-70.png)

I find 2 alerts, so I open the most recent one.

![Open alerts list showing two "Inbound Email Containing Suspicious External Link" alerts, IDs 8815 and 8814, both Medium severity](images/image-71.png)

![Alert queue showing alerts 8815 and 8814 awaiting action, with 3 more alerts incoming](images/image-72.png)

8815 is the most recent, I click the little person icon with a + to assign it to myself.

I read the description.

![Assigned alert 8815 detail, "Inbound Email Containing Suspicious External Link", showing the full email: sender urgents@amazon.biz, recipient h.harris@thetrydaily.thm, subject "Your Amazon Package Couldn't Be Delivered"](images/image-73.png)

I understand that the alert came from suspicious links by email, which looks like a phishing attempt. What catches my attention first is the email saying urgent, and after the @ there's amazon.biz, what would that .biz be? Normally it would be .com.

I see which email it was sent to, h.harris@thetrydaily.thm.

It also catches my attention that the body of the email starts with dear customer, and not the client's name. It's also very common in phishing emails to use urgency, which happens both in the email address and in the body of the email, saying that if the person doesn't click the link within 48 hours, the package will be returned.

I click the Playbook link.

This page opens.

![Phishing Email Analysis playbook diagram: Assign Alert, Alert Triaging Process, Analyze Email Artifacts (Check Sender Reputation, Review Attachments), Investigate Related Alerts, Case Reporting](images/image-74.png)

I understand what my next steps are.

I go back to the documentation and it says to open the SIEM.

![Sidebar navigation showing Dashboard, Alert queue, SIEM, TryDetectThis, and Documentation highlighted](images/image-75.png)

I click on SIEM and this page opens.

![Splunk Enterprise search page, 87 of 87 events matched, showing a list of raw security events](images/image-76.png)

I go back to the Alert Queue and see the alert happened at 5:06pm, so I search for this email in the SIEM with this time.

I found it.

![Splunk event at 5:06:18.543 PM matching the alert: the Amazon package phishing email, sender urgents@amazon.biz, recipient h.harris@thetrydaily.thm](images/image-77.png)

I open TryDetectThis, as the documentation said, and see that I can put in links, IPs and files to analyze.

I go back to the alert and copy the link.

![Zoomed in email content showing the bit.ly shortened link in the body of the Amazon phishing email](images/image-78.png)

![TryDetectThis URL/IP Check page with the bit.ly link pasted in, ready to analyze](images/image-79.png)

Malicious status showed up, as I suspected.

![TryDetectThis result: URL/IP Analysis Complete, Status: MALICIOUS](images/image-80.png)

The documentation says I should correlate the alert data with other data sources to validate the credibility of the alert, but I still don't know how to do this.

Now I need to click write case report on the alert and follow the alert reporting documentation.

![Alert Reporting tab of the documentation, listing what a case report should include and best practice report examples](images/image-81.png)

![Alert Classification tab of the documentation, listing when alert escalation is and isn't required](images/image-82.png)

In the documentation I see that phishing is not escalated if it was quarantined before users had a chance to access the malicious email.

I don't know if the user accessed the email, I believe I might be able to see this in the SIEM.

I opened the SIEM, went again from 5:06pm onward and there was no access to the phishing site from this email.

I put True positive on the case report.

This incident report opened.

![Blank incident report form for 8815, with fields for incident classification, case report and escalation](images/image-83.png)

The incident data is above.

![Case report summary for event ID 8815, showing the alert rule, description, incident type Phishing, severity Medium, and detection time Nov 2nd 2026 at 18:08](images/image-84.png)

Strange that the detection time says it was 6:08pm. It means the system takes longer to detect the phishing, and it was more than 1 hour later.

I fill out the report this way.

![Incident report for 8815 filled in: Time of activity 17:06, Reason for Classifying as True Positive explaining TryDetectThis flagged the link as malicious and the phishing characteristics in the email, escalation set to No](images/image-85.png)

I still don't really know how to fill everything in, even looking at the documentation, it wasn't very clear to me.

I click submit and close alert.

![Confirmation screen: "Nice work! You closed your first alert."](images/image-86.png)

I continue the training.

I look at the Alert queue.

![Alert queue showing 8818, 8817, 8816 and 8814 awaiting action, and 8815 closed](images/image-87.png)

I pull the one with high severity for myself.

![Assigned alert 8816 detail, "Access to Blacklisted External URL Blocked by Firewall", High severity, showing the same bit.ly URL blocked by the firewall](images/image-88.png)

Right away I see it's the same link from the previous alert, so he had actually clicked the link, I hadn't seen that in the SIEM.

The time was just 1 minute after receiving the email.

I go back to the SIEM and find the access.

![Splunk event at 5:07:32.543 PM showing the firewall blocking the connection to the bit.ly URL](images/image-89.png)

I hadn't seen it before, it went unnoticed by me, I lacked attention.

I click write case report.

![Close alert modal for event ID 8816, with True positive selected and a Write case report button](images/image-90.png)

I click true positive.

And this time I'm going to put that it needs to be escalated, because the guy clicked the link.

I fill it out this way:

![Incident report for 8816 filled in with escalation set to Yes](images/image-91.png)

It occurs to me that maybe it didn't need to be escalated, because the incident reports that the firewall or proxy blocked the outbound request, preventing connections.

I go back to the documentation and see it would need to be escalated if there were bigger incidents because of this access, but since the firewall or proxy blocked it before that, it doesn't need to be. So I switch to No on the escalation.

![Incident report for 8816 with escalation switched to No](images/image-92.png)

Incident closed, I take the next most recent one for myself.

Looks like another phishing.

![Assigned alert 8818 detail, "Inbound Email Containing Suspicious External Link", an onboarding email from onboarding@hrconnex.thm to j.garcia@thetrydaily.thm with a profile setup link](images/image-93.png)

I see there seems to be a way to check the proxy and firewall logs to understand if there were attempts to access the link and whether the connections were allowed or blocked. I need to find where I do this.

Right away I see the email was sent from an HR employee, apparently.

The body of the email mentions the employee's name, which is a point toward ruling out a phishing attempt, but not entirely yet.

I copy the URL that is apparently for a training.

![Zoomed in email content showing the hrconnex.thm onboarding link](images/image-94.png)

I try to open TryDetectThis, but the page doesn't load.

![TryDetectThis page stuck on a loading message](images/image-95.png)

I do a quick search on Google.

![Google search for "trydetectthis not opening" with an AI overview suggesting it's a local script loading issue](images/image-96.png)

I manage to use TryDetectThis through the link.

There's no detection/record of this link in TryDetectThis.

![TryDetectThis result: "No results found, no cached reports found for https://hrconnex.thm/onboarding/15400654060/j.garcia"](images/image-97.png)

I find the email in the SIEM.

![Splunk event at 5:09:04.543 PM matching the onboarding phishing email](images/image-98.png)

In the documentation I see there's a list of employee emails. I look for the sender email from the incident and don't find it on the list.

![Company information tab of the documentation, Employees table with name, department, email, logged-in host and IP address](images/image-99.png)

I see I can search TryDetectMe by IP, but I can't find the IP of the email sender.

I go back to the SIEM and see there's an arrow I can click on in the alert:

![Splunk event expanded, showing the full field list for the onboarding phishing email](images/image-100.png)

This opens up a lot more information, but I don't find the IP I need, and I don't understand the information very well.

I click on sender to see information about the supposed HR email.

![Splunk "sender" field popup, showing top values including onboarding@hrconnex.thm](images/image-101.png)

I click on onboarding@hrconnex.thm which is the sender email of the supposed HR.

This opens another page in the SIEM.

![Splunk search for sender="onboarding@hrconnex.thm", 2 of 3 events matched, showing the same onboarding email at 5:09:04.543 PM and 5:03:05.543 PM](images/image-102.png)

Here there are 2 activities from this email, one at 5:03pm and one at 5:09pm.

The 5:03pm email also showed up in the alerts:

![Assigned alert 8814 detail, the same onboarding phishing email sent at 17:03:05.543, still awaiting action](images/image-103.png)

I open the splunk server and click on the IP.

![Splunk "splunk_server" field popup showing a single value, ip-10-10-40-195, 100% of events](images/image-104.png)

There are only the same two emails sent from the supposed HR, nothing more.

Since I can't find the IP, but this email is clearly from an HR account, yet it isn't on the employee email list, and our HR has a different email on the list, and on top of that, there were two identical emails, I'm going to mark it as phishing.

![Incident report for 8818 filled in: List of Attack Indicators noting the email pretends to be HR but isn't in the employee list, and there are 2 identical emails from the same sender](images/image-105.png)

Since there's another alert with the same situation, I'm going to close it too.

![Assigned alert 8814 detail again, ready to be closed with the same reasoning](images/image-106.png)

The last alert is this one:

![Assigned alert 8817 detail, "Unusual Sign-In Activity on Your Microsoft Account", sender no-reply@m1crosoftsupport.co, recipient c.allen@thetrydaily.thm](images/image-107.png)

Right away, I can already see it's really phishing. The sender email uses @m1crosoft with a 1 in place of the i.

This time the body of the email uses the employee's name, indicating they're a bit more experienced at phishing.

I copy the link to TryDetectThis, but it isn't on the list of malicious links yet.

![TryDetectThis result: "No results found, no cached reports found for https://m1crosoftsupport.co/login"](images/image-108.png)

The email isn't in the SIEM.

There's also no access to the link in the SIEM.

I'm going to close it as true positive based on the phishing characteristics analysis already done.

![Incident report for 8817 filled in: List of Attack Indicators noting the sender pretends to be Microsoft, using a 1 instead of an i: m1crosoft](images/image-109.png)

After closing all the incidents, I get some feedback:

![True positives summary table: alerts 8818 and 8814 marked Incorrect, alerts 8815, 8816 and 8817 marked Correct, with an AI overall analysis saying the reports are missing key details](images/image-110.png)

As I imagined, information was missing from the incident report.

I have feedback on this report:

![Incident report for 8815, same as before, shown again alongside the detailed AI feedback](images/image-85.png)

I should have included in this information that the email was signed as "Amazon Delivery", but was sent from a domain that isn't amazon.com. Also, while I mentioned urgency, I didn't specify that the email warned the package would be returned to the sender unless the recipient replied within 48 hours. I have to be explicit about the mechanisms used to put pressure on the victim. The link in the email is a bit.ly shortened URL. These shortened links hide the real destination, which is a red flag I should make explicit. I have to explain what the attacker wanted to do next: the email directed the user to confirm the package delivery or personal information through the link, which was the attacker's goal. I need to check the firewall logs for the link. The recipient's workstation tried to open the link and the firewall blocked the connection. This is critical because it determines whether an escalation is necessary. I also need to search the sender's email logs to confirm whether any other user received the same email. Only the recipient received it, which is important scope information. I need to cover the 5 Ws. I reported When: 17:06, but I didn't identify Who was affected (h.harris@thetrydaily.thm), Who sent it (urgents@amazon.biz), What the subject line was, or What the malicious link was. These are essential details that should appear in every report.

Well, I believed the information provided in the alerts would be enough, and that I wouldn't need to rewrite everything. I see that I do need to.

The next ones will definitely be much better than this one.

Also, I discovered there's a room to learn how to use this SOC simulator, focused on phishing, which I'm going to do right now.

![SOC Simulator: Introduction to Phishing room card, "Play your first SOC Simulator scenario and learn to triage phishing alerts, one alert at a time", 45 min](images/image-111.png)

## What I learned
- A phishing report needs the 5 Ws spelled out explicitly (who, what, when, where, why/how), even when the alert description already contains some of that information. Writing "it was flagged as malicious" isn't enough; the report has to state exactly what was malicious, who sent it, who received it, and when.
- A shortened URL (like a bit.ly link) is itself a red flag worth calling out in a report, because it hides the real destination from the recipient.
- Escalation isn't automatic just because an alert is a true positive. If a control like a firewall or proxy already blocked the malicious connection, the alert can stay closed without escalation, but if the user actually reached the malicious link, that's a reason to escalate.
- Cross-referencing an email sender against the official employee list is a fast way to catch HR or internal-sounding phishing that doesn't come from a real employee address, especially when the real department already has a different address on file.
- Typosquatted domains (like m1crosoftsupport.co or amazon.biz instead of a .com address) are often enough on their own to classify an alert as phishing, even without a detection hit in a tool like TryDetectThis.
- Case reports are judged on completeness, not just on getting the classification right. Closing an alert correctly as a true positive still scored as "incorrect" when the report itself was missing the required details.
