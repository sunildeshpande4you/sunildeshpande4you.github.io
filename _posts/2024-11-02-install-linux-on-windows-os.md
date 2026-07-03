---
title: Install Linux on Windows OS [WSL]
categories:
- Windows
- Linux 
# feature_image: ""
---
---

  *No virtual machine or dual-boot setup required!*

##### What is Windows subsystem for Linux (WSL)?  
  Its a feature of Windows, allows you to run a full-fledged Linux environment directly on your Windows OS for seamless and productive experience for anybody who want to use both Windows and Linux at the same time. WSL was introduced in Windows 10 version 1607 and has since received various updates and improvements.

  WSL supports several Linux distributions, as Ubuntu, Debian, and more. These distributions can be installed directly from the Microsoft Store. Each of these distribution runs as a separate, isolated environment, which allows you to install different distributions at the same time.

##### Why to use WSL rather than Linux in a VM?  
  WSL requires fewer resources (CPU, memory, and storage) than a full virtual machine. WSL also allows you to run Linux command-line tools and apps alongside your Windows command-line, desktop and store apps, and to access your Windows files from within Linux. This enables you to use Windows apps and Linux command-line tools on the same set of files if you wish.

##### How can I install WSL on Windows?
  You can install everything you need to run Windows Subsystem for Linux [WSL] by entering this command in an administrator PowerShell or Windows Command Prompt and then restarting your machine.

  This will enable the required optional components, download the latest Linux kernel, set WSL 2 as your default, and install a Linux distribution for you (Ubuntu by default).


1. To see a list of available Linux distributions available for download through the online store, enter: 
  ```
  wsl --list --online
  ```
  or 
  ```
  wsl -l -o
  ```
2. To change the distribution installed, enter: 
  ```
  wsl --install -d <Distribution Name>.
  ```
  Replace <Distribution Name> with the name of the distribution you would like to install.
3. To install default Linux distribution (Ubuntu) enter: 
  ```
  wsl --install
  ```
4. To install additional Linux distributions after the initial install, you may also use the command: 
  ```
  wsl --install -d <Distribution Name>
  ```



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