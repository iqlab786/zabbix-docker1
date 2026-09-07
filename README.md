## Zabbix Setup (Without Additional Modules, With License Module)

This is a Zabbix setup without extra modules, but with the license module (`license-agent` / `license-proxy`) included.

### 1. Start the Full Stack

Bring up the entire stack:

```bash
sudo docker compose up -d
```

### 2. Check Running Containers

Verify all containers are up:

```bash
sudo docker ps
```

### 3. Stop the License Agent and Restart Zabbix Server

Bring down the `license-agent` container and bring `zabbix-server` back up:

```bash
sudo docker compose down license-agent
sudo docker compose up -d zabbix-server
```

---

## OpenStack VM Setup

If the VM is running on OpenStack, bring the stack up one service at a time instead of all at once, so each dependency is confirmed healthy before the next one starts:

### 1. Start MySQL Server Only

```bash
sudo docker compose up -d mysql-server
```

Check its logs:

```bash
sudo docker compose logs -f mysql-server
```

Wait until the logs show it's ready for connections, then continue.

### 2. Start Zabbix Server

```bash
sudo docker compose up -d zabbix-server
```

Check its logs:

```bash
sudo docker compose logs -f zabbix-server
```

The logs are considered ready once around 20–30 lines have been generated.

### 3. Start the Remaining Stack

```bash
sudo docker compose up -d
```

### 4. Stop the License Agent and Restart Zabbix Server

Same as the standard setup — bring down `license-agent` and bring `zabbix-server` back up:

```bash
sudo docker compose down license-agent
sudo docker compose up -d zabbix-server
```

### 5. Enter License Key and Instance ID

Open the `license.lic` file and enter your **License Key** and **Instance ID**:

```bash
sudo nano license.lic
```

Example contents:

```
License Key: <your-license-key>
Instance ID: <your-instance-id>
```

Save and close the file once both values are filled in.
