# LogicApps2DiscordWebhook
Use Logic Apps To Push to Discord Webhooks


# Generic Guidance

## Store Webhooks in Keyvault  

Store Webhooks in Key Vault (Secrets - Get) and use secure Input in logic app to hide the keys between runs 
> If you know what key you need to pull you don't need to assign Secrets - List. This is up to you for convienence. 

When creating the secret use tags to identify discord and usage  

![image](https://github.com/user-attachments/assets/40ec29e0-c7cc-44fb-8516-29e422061180)

You can leverage a Managed Identity (system or user assigned is up to you - pros and cons. If you are building at scale then I'd go User Assigned) and grant azure access policy to the key vault  

Assign Managed Identity            |  Key Vault Access Policy
:-------------------------:|:-------------------------:
![image](https://github.com/user-attachments/assets/22c7e66e-9dec-4ce9-b636-cb0845894c99)|![image](https://github.com/user-attachments/assets/db7ce2ec-7d6f-4ec6-bb90-1fb38881651d)
> My preference remains Access Policy over Azure RBAC I need to more clear

Creating the connection from Logic app by selecting get Secret and switching the drop down menu to Managed Identity  

![image](https://github.com/user-attachments/assets/85f11c73-b8a3-4d3d-95b7-42ee2b43e1e7)  

## Use secure Input/Output in logic app to hide the keys between runs 
![image](https://github.com/user-attachments/assets/669c260c-ef79-4eb8-8f07-9322b07e7863)

## Try-Catch

Because the logic app may not get a response from Discord you may want to do a manual terminate so runs do not appear failed.  

![image](https://github.com/user-attachments/assets/28c61c09-c91e-443c-a198-7e83179f6aa7)

# Parallel Branching

Need to push to multiple locations? use parallel branches!  

![image](https://github.com/user-attachments/assets/42c123c1-5cb0-4d45-bd13-45acb48bb651)  

## Network restrictions

As logic app consumption does not support private endpoints you can set the outbound IPs of the logic app as an allowed IP on the key vault, this isn't a true security control but better than nothing.  

## Use Parse Json to clear immutables  

[See Damien Bird - Create variables in your Power Automate Flow or Canvas Power App](https://youtu.be/_YiKCWl_kf0?t=483)  

![image](https://github.com/user-attachments/assets/b3483c43-3198-4220-a3a9-c3467d9eef68)



