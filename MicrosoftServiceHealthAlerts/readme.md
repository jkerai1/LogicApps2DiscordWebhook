# Service Health Alerting - Microsoft

The idea is to bring the Service Health Alerts from Microsoft to discord webhooks. The native UI only supports email. There are other solutions but I believe I have come up with a simple one.    
This has some challenges - service support admin is required to see these alerts and you can only see health alerts for products you're licensed for.

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjkerai1%2FLogicApps2DiscordWebhook%2Frefs%2Fheads%2Fmain%2FMicrosoftServiceHealthAlerts%2Fazuredeploy.json)

> You can also use Teams Channels as mailboxes and send alerts there for native easy solution within Teams without the need for a logic app

# Final Result  

![image](https://github.com/user-attachments/assets/fa36c5cc-024d-4630-922d-ed2499ef37cf)

# The Idea

Use a dummy Mailbox to send emails too (in my case I spun up a new personal Outlook mailbox with an email alias), use a mailbox rule to forward the emails to a folder, monitor the folder for new emails from o365mc@microsoft.com then fire off webhook   

From a tenant licensed with all the features you need navigate to https://admin.microsoft.com/adminportal/home#/servicehealth/:/shdpreferences

https://admin.microsoft.com/adminportal/home#/servicehealth/:/shdpreferences

Input the email you want to use  

> Optional: Set a mailbox rule ![image](https://github.com/user-attachments/assets/f9640c4c-a1c9-4b37-a561-5e96bfbe14db)

From Logic app we can create a new trigger - in my case I use outlook v2 as its a personal account, as my emails go into the ServiceHealth folder from the mail rule I select this as my folder
![image](https://github.com/user-attachments/assets/d6c293d3-8f90-43d3-8990-8e90a6840d2b)

I then create a get secret as my webhook is in keyvault, [see guidance on keyvault here](https://github.com/jkerai1/LogicApps2DiscordWebhook/tree/main?tab=readme-ov-file#store-webhooks-in-keyvault)
> Don't forget to secure input/output the keyvault secret

Next we can create the webhook with POST - there is a gotcha logic app gui may escape the backlash - we need to head to the code view to fix it

![image](https://github.com/user-attachments/assets/d321f470-7915-49fa-8427-a3a78d46fb6f)

Subscribe Body - we need to duplicate the body preview and i use the position of "\r\n\r\n" + 4 to find the position I need to crop the string from  
```
{
  "content": "@{triggerBody()?['Subject']} -  [Link to ServiceHealth](https://admin.microsoft.com/Adminportal/Home#/servicehealth)",
  "embeds": [
    {
      "title": "Description",
      "description": "@{substring(triggerBody()?['BodyPreview'],add(indexOf(triggerBody()?['BodyPreview'], '

'),4))}",
      "color": 5814783
    }
  ]
}
```
The final result (from Code View)| Final Result (Design view)
:-------------------------:|:-------------------------:
![image](https://github.com/user-attachments/assets/6b9f6f70-8c46-4360-a16a-6f9bbe4e195f)|![image](https://github.com/user-attachments/assets/ecd67164-8a2c-48e8-9cbb-b955b7cde937)

Finally set the HTTP timeout if you don't require an upstream response:
![image](https://github.com/user-attachments/assets/d0190afc-04d2-42f9-bacb-6b651a93543a)

You can also use a terminate to force all runs to return green:  
![image](https://github.com/user-attachments/assets/b3fce2f8-487c-472d-89c3-48172bd05a35) |![image](https://github.com/user-attachments/assets/e93946b1-4d9b-4942-b3bb-6b805816714c)


# Gotchas

## Outlook
My example uses a personal mail account so I am using outlook v2, if you're using a microsoft 365 account then you need to use Outlook v3. I created a new email account and assigned it as the email alias  

I would recommend using a shared mailbox account then for the m365 route as this doesn't require an additional license.  

![image](https://github.com/user-attachments/assets/90661a0f-1471-4f1a-8244-4ae5abada9fd)

> If you need to recreate the trigger - the properties are here:  
![image](https://github.com/user-attachments/assets/9490f74e-ef76-4e30-8245-3fdb3506b94c)  


## Backslash character
Logic app GUI will natively escape backlashes, if you need to add backslash literal you need to use CodeView to edit and add in.  

# Logic App Full

![image](https://github.com/user-attachments/assets/e5802cf2-adfb-438e-909c-8a0f23ba6686)  




# To Improve  

Full parse body rather than just parse body preview
