---
title: Creating and Using SSH Keys in Windows OS
categories:
- SSH 
# feature_image: ""
---
---

  There are several ways to create SSH keys in Windows. Follow the instructions below for the SSH client you use.

##### Generating SSH keys with OpenSSH (Windows 10 and newer)  
1. Press the Windows key or open up the Start Menu. Type “cmd”.  
2. Under “Best Match”, click “Command Prompt”.  
3. In the command prompt, use the ssh-keygen command:  
![](/docs/assets/ssh-keygen-cmd.png)
By default, the system will save the keys to [your home directory]/.ssh/id_rsa.  Unless you are an expert you should use the default option and press Enter.  
4. The system will now generate the key pair and display the key fingerprint and a random art image. These fingerprints are   
   not needed in day-to-day use of your keys but can be saved to your notes to identify your keys later if needed.  
5. Open your file explorer.  You can now navigate to the hidden “.ssh” directory in your home folder. You should see two new  
   files. The identification is saved in the id_rsa file and the public key is labeled id_rsa.pub. This is your SSH key pair. They are both saved in plain text.  
![](/docs/assets/ssh-folder.png)  
  For usage of your new keys with a remote host, see “Copying your public key to a host” below.  


##### Generating SSH keys with PuTTY  
1. PuTTY is Free and Open-Source software. It can be obtained from the PuTTY latest release page.  
2. Once PuTTY is installed, press the Windows key or open the Windows and type “puttygen” and open the “PuTTYgen” app.  
3. In the PuTTY Generator window, make sure that “RSA” is selected at the bottom of the window and click “Generate”. Move your 
   mouse cursor over the gray area to fill the green bar.  
![](/docs/assets/putty-keygen.png)  
4. You need the public key written at the top of the window for your authorized_keys file (see “Copying your public key to a 
   host” below). PuTTY does not save the public key for you. You can copy and paste it directly to your authorized_keys file or copy and paste this key into a notepad document for safe keeping to copy later.
![](/docs/assets/putty-keygen-generated.png)  
5. Now the private key needs to be saved. Click the “conversions” menu at the top and select “Export OpenSSH Key”. Generally 
   you want to save this without a passphrase, so click “Yes” in the next dialog box. Choose a location to save the key and give your key a name (e.g. putty_key).  
![](/docs/assets/putty-keygen-export.png)  
6. Your keys are generated and you can close the PuTTY key generator. To use your new key with PuTTY, you need open 
   “Connection” and “Auth” in the PuTTY configuration. Under “Private Key file for authentication” choose the private key you just saved.  
![](/docs/assets/putty-config.png)  
You will need to copy your public key from Step 4 above to the host you wish to use your keys with.  See “Copying your public key to a host” below.  

##### Copying your public key to a host  
Public keys are in text format and copying them to a remote host can be done with cut and paste commands. The public key file you created can be opened with a text editor and it will look something like this *:  
![](/docs/assets/ssh-authkeys.png)

The key can contain numbers, letters, or symbols like the one above. On remote Unix, Linux, or MacOS machines the public key needs to be placed into a file called ~/.ssh/authorized_keys file using your favorite text editor. There can be multiple public keys in the authorized_keys file. If the file does not exist it needs to be created. Your authorized_keys file needs to be set to owner read/write only (mode 600). When using your key file with a Windows 10 or 11 host you similarly put your key into a text file called authorized_keys in a hidden .ssh folder in your user folder.  

> If using PuTTY the public key is shown in the window and not in a separate file. See step 4 of "Generating SSH keys with  PuTTY" above. That will be the key needed for your cut and paste

##### Conclusion
  We have seen quick and easy steps to generate SSH keys in Windows OS that you can use access remote machines like AWS EC2 instance.

---
##### References:
  [Putty-Download](https://www.putty.org/)  