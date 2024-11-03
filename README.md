# home-storage-server
Create your own home network storage server for streaming, backup and remote access (Local network)
Requirements:
1. Linux Ubuntu System
2. Samba Library
3. Home Router (Via Ethernet for better speed)

   To create a shared network drive that other devices, such as your TV, can access, we'll use Samba—an open-source file-sharing protocol compatible with Linux, Windows, and most other platforms. Here’s a step-by-step guide:


   Step 1 :
     Open a terminal and Update your system. Install Samba (Open-source file-sharing protocol compatible with Linux, Windows, and most other platforms.)
     > sudo apt update && sudo apt upgrade -y
     > sudo apt install samba -y
   
   Step 2 :
     **Set Up a Shared Folder**
     1. Choose or create a folder that you want to share across your network. Create a new directory for shared storage (or use an existing one):
       > sudo mkdir -p /home/<your-username>/shared
     2. Set permissions on this directory:
       > sudo chmod -R 777 /home/<your-username>/shared
       This gives read, write, and execute permissions to all users. This setup works in a trusted home network but can be restrictive for more secure environments.

   Step 3 :
  **Configure Samba**
    Now, configure Samba to make this folder accessible over the network.
    1. Open the Samba configuration file:
       > sudo nano /etc/samba/smb.conf
    2. Scroll to the bottom of the file and add a new entry for your shared folder:
       > [Shared]
       > path = /home/<your-username>/shared
       > available = yes
       > valid users = <your-username>
       > read only = no
       > browsable = yes
       > public = yes
       > writable = yes
    3. Replace <your-username> with your actual Ubuntu username.
    4. Save the file and exit (Ctrl+X, then Y, then Enter).
 
 Step 4 :
 **Create a Samba User**
   To allow network access to the shared folder, create a Samba user:
   1. Add your Ubuntu username to Samba:
    > sudo smbpasswd -a <your-username>
   2. Enter and confirm a password. This will be the password you use to access the shared folder from other devices.

Step 5:
**Restart Samba Services**
  After making these changes, restart the Samba services for them to take effect:
    > sudo systemctl restart smbd
    > sudo systemctl restart nmbd

Step 6:
**Adjust Firewall Settings (if needed)**
  If you have a firewall enabled on Ubuntu, you may need to allow Samba traffic.
  > sudo ufw allow samba

Step 7: 
**Accessing the Shared Folder from Other Devices**
  **Windows**
  Open File Explorer.
  In the address bar, type:

  > \\<IP-ADDRESS>\Shared
  Replace <IP-ADDRESS> with the IP address of your Ubuntu machine. You can find your IP address by running:

  or Right This PC -> Map NetworkDrive

  > ip addr show
  Enter the Samba username and password when prompted.

  **macOS**
  Open Finder.
  Press Cmd + K to open the “Connect to Server” window.
  Type:
  vbnet
  Copy code
  smb://<IP-ADDRESS>/Shared
  Enter your Samba username and password if prompted.

  **Smart TV or Streaming Device**
  Most smart TVs or media players that support DLNA (Digital Living Network Alliance) can stream content from network shares. However, Samba itself is not DLNA-compatible. For this, consider installing MiniDLNA or Plex:

  Install MiniDLNA:
  > sudo apt install minidlna
  Configure it by editing the file:
  > sudo nano /etc/minidlna.conf
  Set media_dir to point to your shared folder:
  > media_dir=V,/home/<your-username>/shared
  Start MiniDLNA:
  > sudo systemctl restart minidlna
  This will allow your TV or other media devices to see your Ubuntu storage server as a DLNA server, letting you stream content easily.

Step 8:
**Testing the Setup**
  Once everything is set up:

  Access the shared folder from another computer on the network to confirm it’s working.
  On your TV, look for a DLNA-compatible media player or network device that can connect to the server and browse available videos.
  Optional: Auto-Mount Shared Folder on Other Computers
  For convenience, you can configure devices to auto-mount this shared folder on startup (e.g., with fstab in Linux or the Map Network Drive feature in Windows).


