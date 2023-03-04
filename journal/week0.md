# Week 0 — Billing and Architecture

## Required Homework

- Created AWS Admin account
- Enabled MFA for Root Account and Admin Account
- Created Alias for account ID - to make it easier for loggin in as the admin account and not the root account.
- Created Billing Alarm 
- Created Policy to allow Adm user to view Billing Console.
- Generated Access key for AWS CLI

## Created Conceptual Diagram on Lucid App or napkin. (Extra point for mentioning Napkin please).
![Image of Screen shot of Lucid chart](assets/Conceptual.png)
## Created Lucid Chart 
![Image of Screen shot of Lucid chart](assets/Screenshot%202023-02-28%20at%208.51.56%20PM.png)

##  Created Bill for KMS

![Image of Screen shot of Bill](assets/Bill%20for%20KMS.png)


## Applied AWS Credits

![Image of Screen Shot of AWS Credit](assets/AwsCredits.png)


## AWS CLI

Launched cloudshell and enabled AWS Auto Prompt

Command line options enable or disable auto-prompt for a single command. Use --cli-auto-prompt to call auto-prompt and --no-cli-auto-prompt to disable auto-prompt.

How it works
If enabled, the auto-prompt enables you to use the ENTER key to complete a partially entered command. After pressing the ENTER key, commands, parameters, and resources are suggested based on what you continue to type. The suggestions list the name of the command, parameter, or resource on the left and a description of it on the right. To select and use a suggestion, use the arrows keys to highlight a row, and then press the SPACE key. When you've finished entering in your command, press ENTER to use the command. The following example demonstrates what a suggested list from auto-prompt looks like.


##Install AWS CLI within Gitpod
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64-2.0.30.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

Sudo means = Super user DO


How to set Environment Variables


        export AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
        export AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
        export AWS_DEFAULT_REGION=us-west-2
