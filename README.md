# LogicApps2DiscordWebhook
Use Logic Apps To Push to Discord Webhooks


# Generic Guidance

## Store Webhooks in Keyvault  

Store Webhooks in Key Vault (Secrets - Get) and use secure Input in logic app to hide the keys between runs 
> If you know what key you need to pull you don't need to assign Secrets - List

You can leverage a Managed Identity and grant azure access policy to the key vault

## Use secure Input/Output in logic app to hide the keys between runs 
![image](https://github.com/user-attachments/assets/669c260c-ef79-4eb8-8f07-9322b07e7863)

## Try-Catch

Because the logic app may not get a response from Discord you may want to do a manual terminate so runs do not appear failed.  

![image](https://github.com/user-attachments/assets/28c61c09-c91e-443c-a198-7e83179f6aa7)

## Network restrictions

As logic app consumption does not support private endpoints you can set the outbound IPs of the logic app as an allowed IP on the key vault, this isn't a true security control but better than nothing

## Use Parse Json to clear immutables  

[See Damien Bird - Create variables in your Power Automate Flow or Canvas Power App](https://youtu.be/_YiKCWl_kf0?t=483)  

![image](https://github.com/user-attachments/assets/b3483c43-3198-4220-a3a9-c3467d9eef68)
