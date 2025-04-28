# EasyForMe

## Overview

### Method of Infection
EasyForMe (EFM) is a group that sells a `.jar` stealer in a MaaS model. Often this `.jar` file is hidden as a Minecraft mod or Minecraft server plugin.

### Malware Behavior

#### Execution:
When the malware is launched, it contacts a Command and Control (C2) server to download the main stealer payload. It runs with the parameter(s):

- Minecraft Username \*
- Minecraft UUID \*
- Minecraft SSID/Access Token \*
- Affiliate' s RSA-encrypted Discord webhook

(* Not every version of the stealer uses all these parameters.)

#### Payload Behavior:
After launching, the payload searches the victim's computer for sensitive files, like those related to Sonoyuncu, Craftrise, crypto wallets, and victims's Discord token. The payload downloads another .jar file called EFM_Decrypt from the C2 server that is run. Once EFM_Decrypt finishes the payload sends two exfiltrations, one to the affiliate (via a Discord webhook), and one directly to the devs'.

#### EFM_Decrypt:
EFM_Decrypt is a browser stealing module that collects saved passwords and cookies from the victim’s browsers, then packs all the stolen information into a ZIP file to a hardcoded location that is later read by the main payload.

# Indicators of Compromise

## URLs:

- **C2 Server:**  
  hxxps://log.easyfor.me | IP: 2606:4700:3036::ac43:b8b2:

- **Affiliate Data Exfiltration:**  
  hxxps://discord.com/api/webhooks/{webhookID}/{webhook_token} | (IP address varies due to Discord's infastructure)

## Hashes:

- **Main Payload:**  
  SHA-256: 78029f3b271cef3fa1e110086df7e8a3cbc15d5c3ad19e9e54ce0d86c8568159

- **Browser Stealer (EFM_Decrypt):**  
  SHA-256: 1dae30054b01fe2d78addcbb5a44acb9bcd585da3823a76b2038e1f3d73a23b5

# Dynamic Analysis

## Triage Reports:

- **EFM_Decrypt Behavioral Report:**  
  https://tria.ge/250426-bkmtss1xdy/behavioral1

- **Runtime Payload Behavioral Report:**  
  https://tria.ge/250426-bvqnfavkw5/behavioral1 (Fails to launch properly due to a custom entry point.)

## Network Traffic:

- **EFM_Decrypt Download:**  
  hxxps://log.easyfor.me/JarFiles/EFM_Decrypt.jar  

- **Runtime Payload Download:**  
  hxxps://log.easyfor.me/JarFiles/Runtime/Runtime-Free.jar  

- **Discord Webhook Exfiltration:**  
  (Stolen data is sent to the affiliate through their Discord webhook URL.)

- **Developer Data Exfiltration:**  
  hxxps://log.easyfor.me/Runtime  
  (Victim data is also uploaded directly to the developers' server, and is likely sent to a telegram due to the User-Agent being "TelegramBot/1.0")
