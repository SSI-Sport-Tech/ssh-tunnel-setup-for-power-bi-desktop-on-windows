# SSH Tunnel Setup for Power BI Desktop on Windows

## Documentation

### Introduction
This setup will allow you to securely connect to a remote data source via SSH tunneling while using Power BI Desktop on your Windows PC.

### Important
**<u>Do not download Power BI from Microsoft Store, as the solution proposed is not compatible, download from the executable from Microsoft Website instead!</u>**

### Solution
#### Step 1: Download and Install Power BI Desktop from Microsoft Website
1. Go to the [Power BI Desktop download page](https://www.microsoft.com/en-us/download/details.aspx?id=58494).
2. Click on the "Download" button to download the `PBIDesktopSetup_x64.exe.exe` file.
3. Run the downloaded `PBIDesktopSetup_x64.exe.exe` file and follow the installation instructions.
4. After installation, a shortcut for Power BI Desktop should appear on your desktop. If it does not, follow the steps below to create one manually.

#### Step 2: [OPTIONAL] Create a Shortcut Manually (if needed)
1. Navigate to the installation directory of Power BI Desktop. By default, it is usually located at:
   ```
   C:\Program Files\Microsoft Power BI Desktop\bin\PBIDesktop.exe
   ```
2. Right-click on `PBIDesktop.exe` and select `Send to > Desktop (create shortcut)`.

#### Step 3: [OPTIONAL] Create ssh key pair (if needed)
Proceed with ssh key pair generation if this is not generated prior. Ask the system admin to add your public key into compute so that you can utilize ssh tunneling. If your account was provision by admin before hand, you can skip this step

```sh
ssh-keygen -t rsa -b 4096
```
By default, if you click Enter to skip all prompts, the ssh key pair should be generated in `%USERPROFILE%` directory. Example, if username is user1, `%USERPROFILE%` will point to `C:\Users\user1` (this is assuming your home drive is set to `C:\`)

#### Step 4: Configure SSH Tunnel Command
1. Open notepad and copy following command.
2. The following command is used to create an SSH tunnel:
   ```sh
   ssh -i "%USERPROFILE\.ssh\ssh_private_key" username@ssh_server -L local_port:remote_host:remote_port -N
   ```
   Replace `ssh_private_key`, `local_port`, `remote_host`, `remote_port`, `username`, and `ssh_server` with your specific details. Remote port will be 3306 for connecting to MySQL database. `-N` argument is to specify do not execute remote commands. This will prevent ssh interactive session.

#### Step 4: Inject SSH Tunneling Command into Shortcut
1. Right-click on the Power BI Desktop shortcut on your desktop and select `Properties`.
2. In the `Target` field, use the following command, replace the `ssh_tunneling_command`with the command you edited on notepad. For example:
   ```
   C:\Windows\System32\cmd.exe /K "start /b ssh_tunneling_command && start "" "C:\Program Files\Microsoft Power BI Desktop\bin\PBIDesktop.exe""
   ```
3. Click `Apply` and then `OK`.

#### Step 5: Launch Power BI Desktop
Double-click the modified shortcut to launch Power BI Desktop with the SSH tunnel configuration. You should see a command prompt pop up. If no errors on command prompt, this means that ssh tunneling is successful.
