# Phishing Email Investigation

## About This Investigation

In this project, I investigated a suspicious email pretending to be from Microsoft 365 Security.

The email claimed that the user's password would expire that day and asked the user to verify the account within 30 minutes to avoid suspension.

My aim was to check the email carefully and decide whether it was legitimate or phishing.

> This was a simulated phishing email used for hands-on SOC investigation practice.

## Initial Email Review

I first reviewed the email as it would appear to the recipient.

The message immediately raised some concerns. It used the subject:

`URGENT: Your Microsoft 365 password expires today`

It also told the user to verify the account immediately and threatened temporary suspension within 30 minutes.

I noticed that the sender address was:

`security-alert@micr0soft-support.example`

The word `micr0soft` used the number `0` instead of the letter `o`.

![Phishing Email](screenshots/Phishing%20Email.png)

## Link Analysis

I did not open the verification link.

Instead, I hovered over it to check where it would actually lead.

The destination was:

`https://microsoft-login.example/verify`

The link did not point to an official Microsoft domain, which made the email more suspicious.

![Suspicious Link](screenshots/Suspicious%20link.png)

## Email Authentication Check

I opened the raw email and reviewed the authentication results.

I found:

- SPF: `fail`
- DKIM: `none`
- DMARC: `fail`

The failed SPF and DMARC checks showed that the email could not be properly authenticated as coming from the claimed sender.

![Authentication Results](screenshots/Authentication%20result.png)

## Sender and Header Analysis

I then looked further into the email headers.

The message showed:

- Sending IP: `203.0.113.45`
- Sending host: `mail.account-verify.example`
- From: `security-alert@micr0soft-support.example`
- Reply-To: `helpdesk@account-verify.example`

The From address and Reply-To address did not match, and the sender domain was made to look similar to Microsoft.

![Sender Analysis](screenshots/Sender%20analysis.png)

## Indicators I Extracted

| Type | Value |
|---|---|
| Sender | `security-alert@micr0soft-support.example` |
| Reply-To | `helpdesk@account-verify.example` |
| Sending IP | `203.0.113.45` |
| Suspicious URL | `https://microsoft-login.example/verify` |
| Subject | `URGENT: Your Microsoft 365 password expires today` |

## My Findings

After reviewing the email, I classified it as phishing.

The main reasons were the lookalike sender domain, the suspicious verification link, failed SPF and DMARC authentication, the different Reply-To address, and the urgency used to pressure the user.

## Recommended Response

If this email was found in a real environment, I would recommend quarantining it and checking whether the same message was delivered to other users.

The sender, domain, URL, and other extracted indicators could also be searched across the environment.

If a user had already entered their password on the phishing page, their account should be investigated for unusual sign-in activity and appropriate account containment should be carried out.

## What I Learned

This investigation helped me understand how different pieces of evidence can be combined when reviewing a suspicious email.

I practiced checking the visible email, inspecting a link without opening it, reviewing SPF, DKIM and DMARC results, checking sender and routing information, extracting indicators, and making a final decision based on the evidence.
