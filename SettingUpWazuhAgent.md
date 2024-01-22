# Setting up a Wazuh agent

## Ubuntu server

Select agents in the Wazuh dropdown menu
![Alt text](image.png)
Fill out the text fields
![Alt text](image-1.png)
Execute the command specified to install the 
agent:
```
wget https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.7.2-1_amd64.deb && sudo WAZUH_MANAGER='10.0.2.6' WAZUH_AGENT_NAME='ubuntu-server-host-1' dpkg -i ./wazuh-agent_4.7.2-1_amd64.deb
```
Execute the command specified to start the agent
```
sudo nano /var/ossec/etc/ossec.conf replace MANAGER_IP with 10.0.2.6
sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```
/var/ossec/etc/ossec.conf
/var/ossec/logs/ossec.log