Name: Chaitra E
Batch: July 2026
Date: 19th August 2026

// Aprove or Deny Stage
Approve Stage: Approve/Deny was not working when clicked, so I added a Choice Parameter for the approval decision and installed the Pipeline: Input Step plugin for this purpose.

// Mail Sending part

1. Installed Email Extension Plugin
2. Configuring the SMTP

Go to:

Manage Jenkins → System → E-mail Notification

Configure your SMTP server. For example, Gmail SMTP:

SMTP server: smtp.gmail.com
SMTP port: 465 or 587

3. Adding the gmail credentials

Go to:

Jenkins → Manage Jenkins → Credentials → System → Global credentials → Add Credentials

Select:

Kind: Username with password
Username: yourgmail@gmail.com
Password: Refer Password Discription below
ID: gmail-jenkins
Description: Gmail SMTP

Password Description

**First, tried multiple Jenkins SMTP configurations using the Gmail password, but it was not working. Then created a Gmail App Password named Jenkins SMTP and used the generated 16-character App Password.**

4. Again in E-mail Notification

Go to:

Manage Jenkins → System → E-mail Notification

In Advance setting select created credential
Check TLS option
