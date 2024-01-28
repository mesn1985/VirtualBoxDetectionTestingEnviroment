# Mitre Caldera server setup
Mitre Caldera is an adversary emulation platform that, among other things, can be used to simulate attacks. This can be useful when testing the reaction of IDS and IPS configurations of a system, and therefore, it can be used for red team training and blue team training. Caldera aligns with the MITRE ATT&CK framework, allowing for emulations of specific attack patterns and behaviors.

In this guide, we will set up the Mitre Caldera Server, hosted on an Ubuntu server.

## prerequisites 
You should have completed the instructions for [installing VirtualBox](./EnviromentSetup.md#install-virtual-box), [Setting up the Network](./EnviromentSetup.md#setting-up-the-network), and [Setting Up Ubuntu Server VM](./EnviromentSetup.md#setting-up-ubuntu-server-vm).

You should setup a new Ubuntu server with the IP 10.0.2.7.

All the configuration instructions are for ubuntu server, but it should work with most Debian distributions.
  
Requirements for Caldera server are 8GB and 2 CPUs

## Caldera Server Dependencies Setup

Caldera server is a Python application and, therefore, needs a Python interpreter to execute. Furthermore, Caldera server also requires runtime support for the Go language. Both of these runtime requirements need to be configured in the executing OS before running Caldera server.

The version control tool Git is also required to clone the Caldera server source code from the official Caldera server repository.

Here are three guides for setting up each of the requirements.

### Golang
The Go language is a Google-supported programming language, for which Caldera server requires runtime support. To install Go language on an Ubuntu server, follow the instructions below:

1. Update the apt package manager package list by executing `apt update`.
2. Upgrade all installed packages by executing the command `apt upgrade`. (Strictly speaking, this is not necessary, but it does guarantee that there are no outdated dependencies).
3. Install the `Golang` package by executing the command `apt install golang-go`.

### Git
The default installation of Ubuntu server should have Git installed by default, but if this is not the case, simply install Git using the command `apt install git`.

### Python 3
The default installation of Ubuntu server should have Python 3 installed by default, but if this is not the case, simply install Python 3 using the command `apt install python3`.

Furthermore, the Python 3 package manager `pip3` needs to be installed by executing the command `apt install python3-pip`.
  
## Setting up caldera server
The Caldera server default setup is straightforward; it essentially requires you to clone the repository and execute the Caldera server as a Python script.

The Caldera server provides a GUI accessible through a website. The website is exposed on port 8888, and to ensure accessibility from the VirtualBox host, we will begin by configuring port forwarding from host port 8888 to the VM's port 8888.

### Port Forwarding with VirtualBox Network Manager

1. Navigate to _File -> Tools -> Network Manager_ in VirtualBox.
2. Click on the `NAT Networks` pane.
3. Double-click on the NAT network `DefaultVMNet`.
4. In the bottom menu at the bottom of the screen, click the `Port Forwarding` pane.
5. Click the green plus icon to add a new port forwarding rule.
6. Provide the rule with the following properties shown in the table below:

  | Name        | Protocol | Host IP    | Host Port | Guest IP | Guest Port |
  | ----------- | -------- | ---------- | --------- | -------- | ---------- |
  | Caldera GUI | TCP      | 127.0.0.1  | 8888      | 10.0.2.7 | 8888       |

  Once you have completed the next section, you will be able to access the Caldera server GUI from the host at `127.0.0.1:8888`.

### Cloning the Source Code

1. **Clone the Repository:**
   ```bash
   git clone https://github.com/mitre/caldera.git --recursive --branch 4.2.0
   ```

   This command clones the Caldera repository from GitHub, specifying the branch (release 4.2.0) and including submodules (`--recursive`).

### Installing Dependencies

2. **Navigate to Cloned Directory:**
   After cloning, navigate to the directory with the cloned source code.

3. **Install Python Dependencies:**
   ```bash
   pip3 install -r requirements.txt --break-system-packages
   ```

   Install Python dependencies using `pip3`. The `--break-system-packages` option indicates that the installed packages are system-wide.

### Starting the Server

4. **Start Caldera Server:**
   ```bash
   python3 server.py
   ```

5. **Verification:**
   Wait for the server to start and verify that it has started correctly.

6. **Access the Server:**
   Enter `127.0.0.1:8888` in the host system's browser to access the Caldera server.

7. **Shutdown the Server:**
   Press `ctrl+c` to shut down the server.

### Configuring Authentication

8. **Open Configuration File:**
   ```bash
   nano <Caldera source path>/conf/local.yml
   ```

   Open the configuration file (`local.yml`) using the `nano` text editor.

9. **Change Passwords:**
   Change the passwords for users 'red', 'admin', and 'blue' in the configuration file.

   Save changes by pressing `ctrl+s`, then exit by pressing `ctrl+x`.

### Restarting the Server with Updated Configuration

10. **Restart Caldera Server:**
    Execute the Caldera server again:
    ```bash
    python3 server.py
    ```

11. **Authenticate:**
    Enter the URL `127.0.0.1:8888` in the host system's browser and authenticate using the updated credentials from step 9.

# Configuring the server to start on boot
It is inconvenient to manually start the server every time you restart the VM, so we will configure the Ubuntu server to automatically start up the server upon boot. In order to start the server automatically upon boot, we need to add it as a [Systemd](https://www.linux.com/training-tutorials/understanding-and-using-systemd/) service. Follow the instructions below to configure the server as a systemd service:

1. Create and edit a service file named `caldera_startup.service` in the directory `/etc/systemd/system/` by opening the text editor nano with the command: `nano /etc/systemd/system/caldera_startup.service`.
2. Insert the text below. The placeholders represented with `<...>` need to be replaced with values specific to your configuration:

```
[Unit]
Description=Script for starting up Caldera server

[Service]
WorkingDirectory=<path to caldera source code directory>
User=<user name, Not root!!! Remember the principle of least privilege!>
ExecStart=/usr/bin/python3 <path to caldera source code directory>/server.py
ExecStop=/bin/kill -s SIGINT $MAINPID
Restart=always

[Install]
WantedBy=multi-user.target
```

3. Save and exit nano by first pressing `Ctrl+S` and then `Ctrl+X`.
4. Enable the service by executing the command: `systemctl enable caldera_startup`.
5. It's recommended to reload systemd after creating a new service file. After saving and exiting nano, execute the following command to reload systemd: `systemctl daemon-reload`.
6. Verify a successful server startup by viewing the system logs that involve the created Caldera service with the command: `journalctl -b | grep caldera`.
7. Enter the GUI from the host's browser to again verify the correct working of the service.

Now you can just start the VM, and the Caldera server will automatically be started as a service.

## Graceful shutdown (Important to read!!!)
"It is important to ensure the graceful shutdown of the Caldera server. The line `ExecStop=/bin/kill -s SIGINT $MAINPID` in the service configuration ensures that the server is closed with `SIGINT` when the VM is gracefully shut down (e.g., shutting down the server with the `shutdown` command). If you simply power off or otherwise ungracefully shut down the server, you may experience errors and lose data that has not been persisted."

## viewing logs of the caldera service
The easiest way to view Caldera server logs is by using `journalctl`. For example, the command:

```bash
journalctl -r -u caldera_startup.service
```

This command fetches the application logs for the `caldera_startup.service` unit, displaying them in reverse order, starting with the newest logs first. It provides a convenient way to review recent logs for troubleshooting and monitoring purposes.