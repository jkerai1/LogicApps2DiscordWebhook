[![GitHub stars](https://img.shields.io/github/stars/jkerai1/LogicApps2DiscordWebhook?style=flat-square)](https://github.com/jkerai1/LogicApps2DiscordWebhook/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/jkerai1/LogicApps2DiscordWebhook?style=flat-square)](https://github.com/jkerai1/LogicApps2DiscordWebhook/network)
[![GitHub issues](https://img.shields.io/github/issues/jkerai1/LogicApps2DiscordWebhook?style=flat-square)](https://github.com/jkerai1/LogicApps2DiscordWebhook/issues)
[![GitHub pulls](https://img.shields.io/github/issues-pr/jkerai1/LogicApps2DiscordWebhook?style=flat-square)](https://github.com/jkerai1/LogicApps2DiscordWebhook/pulls)  

# LogicApps2DiscordWebhook
Use Logic Apps To Push to Discord Webhooks

# General Structure  

This is a very helpful tool for [formatting the JSON](https://discohook.org/)  

But here is the basic Example - we need the HTTP webhook Connector and this json:
> You can use generic HTTP also for single fire use-case  

![image](https://github.com/user-attachments/assets/bd6d0f11-4a82-4691-bb50-45f9a759459c)  

```
{
  "content": "YOUR TEXT",
  "embeds": [
    {
      "title": "<YOUR TEXT>",
      "description": "YOUR TEXT",
      "color": 5814783
    }
  ]
}
```
> Colour is optional  

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

Because I don't allow Secrets List I need to use Custom Value to add in the secret name:

Listing is Forbidden          |  Use Custom Name
:-------------------------:|:-------------------------:
![image](https://github.com/user-attachments/assets/0ab935e1-1fe8-44b4-beca-881f6f298b6b) | ![image](https://github.com/user-attachments/assets/a5f00724-dfe6-407a-a996-d6dfe12073c9)

Make sure to lock input/output from Settings > Security   

![image](https://github.com/user-attachments/assets/c30b86ed-370b-436a-a61f-1ef92aba77fa)

As I put the entire discord webhook URL in the keyvault this is now what my webhook looks like:  

![image](https://github.com/user-attachments/assets/34d896f4-5b85-4ae4-b086-88a7a6b45652)

## Use secure Input/Output in logic app to hide the keys between runs if you aren't using keyvault  
![image](https://github.com/user-attachments/assets/669c260c-ef79-4eb8-8f07-9322b07e7863)
> Otherwise a playbook operator role Azure IAM will be able to see the secrets as they are run. Niche but easy to secure

## Try-Catch

Because the logic app may not get a response from Discord you may want to do a manual terminate so runs do not appear failed.  

![image](https://github.com/user-attachments/assets/28c61c09-c91e-443c-a198-7e83179f6aa7)

# Parallel Branching

Need to push to multiple locations? use parallel branches!  

![image](https://github.com/user-attachments/assets/42c123c1-5cb0-4d45-bd13-45acb48bb651)  

## Network restrictions

As logic app consumption does not support private endpoints you can set the outbound IPs of the logic app as an allowed IP on the key vault, this isn't a true security control but better than nothing.  

## Use Parse Json to declare immutables  

[See Damien Bird - Create variables in your Power Automate Flow or Canvas Power App](https://youtu.be/_YiKCWl_kf0?t=483)  

![image](https://github.com/user-attachments/assets/b3483c43-3198-4220-a3a9-c3467d9eef68)



