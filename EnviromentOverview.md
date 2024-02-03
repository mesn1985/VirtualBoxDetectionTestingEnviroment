# Environment Overview

This section provides an overview of the environment. Note that the IP addresses are only valid if you have chosen to use suggested IP addresses in the guides.

## Environment Subnet 10.0.2.0/24

The subnet is as shown below.

| Description                | Value           |
| -------------------------- | --------------- |
| Total number of IP addresses | 256           |
| Number of usable IP addresses | 253           |
| Network                     | 10.0.2.0        |
| Gateway                     | 10.0.2.1        |
| Broadcast                   | 10.0.2.255      |
| First usable IP address     | 10.0.2.2        |
| Last usable IP address      | 10.0.2.254      |

### Hosts on the Subnet
The table below shows all hosts with IP addresses as specified in the guide. Please ignore the `service` and `host mapped ports` columns. These columns serve a development purpose for my convenience.

| Name                  | OS                      | IP          | Services                          | Host Mapped Ports |
| -------------------- | ----------------------- | ----------- | --------------------------------- | ------------------ |
| Kali                 | Kali Linux              | 10.0.2.2    | Default Kali services            | None               |
| Metasploitable       | Ubuntu 8                | 10.0.2.3    | Default Metasploitable services  | None               |
| Ubuntu Server Host   | Ubuntu Server 23.10     | 10.0.2.4    | Linux Cockpit                     | 9090 -> 9090       |
| Windows 11 Host      | Windows 11 (Developer)  | 10.0.2.5    | No services added initially       | None               |
| Wazuh Server         | Amazon Linux 2          | 10.0.2.6    | Wazuh manager + all required services | 443 -> 443    |
| Caldera Server       | Ubuntu Server 23.10     | 10.0.2.7    | Caldera server                    | 8888 -> 8888       |