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