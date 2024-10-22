
---

### Step 1: Install OpenSSH Server
Install the OpenSSH server to accept incoming SSH and `scp` connections.

```bash
sudo apt update
sudo apt install openssh-server
```

---

### Step 2: Start the SSH Service
After installation, start and enable the SSH service to ensure it’s running and set to start on boot.

```bash
sudo systemctl start ssh
sudo systemctl enable ssh  # Enable SSH to start on boot
```

---

### Step 3: Check SSH Status
Verify that the SSH server is running:

```bash
sudo systemctl status ssh
```

You should see `active (running)` if the SSH service is working properly.

---

### Step 4: Configure the Firewall
If you have a firewall enabled, allow SSH traffic through the firewall:

```bash
sudo ufw allow ssh
sudo ufw reload
```

---

### Step 5: Determine the EPC/ENB Machine’s IP Address
Check the IP address of the machine running `srsEPC` and `srsENB`:

```bash
ifconfig  
```

Ensure this IP is accessible from the UE machine.
e.g.
172.16.0.1

---

### Step 6: Create a User Account (Optional)
If you prefer not to use the root account, create a new user for file transfers from srsepc device:

```bash
sudo adduser transferuser
sudo passwd transferuser  # Set a password for the new user
```
e.g. 
sudo adduser srsuser
sudo passwd srsuser

---

### Step 7: Perform the File Transfer via SCP
On the UE machine, use `scp` to transfer the file to the EPC/ENB machine:

```bash
scp /path/to/local/file transferuser@<EPC_IP>:/path/to/destination/
```

- Replace `<EPC_IP>` with the IP address of the EPC/ENB machine.
- Replace `/path/to/local/file` with the file you want to transfer.
- Replace `/path/to/destination/` with the directory on the EPC/ENB machine where you want the file to go.

e.g scp testfile.txt 172.16.0.1:/home/katnap/ttransfer/received 
or
scp /home/katnap/ttransfer/sent/testfile.txt 172.16.0.1:/home/katnap/ttransfer/received

**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXddAO65Mt5d-5vjN0OurjyaHNqp6rsDi-5KQ8PpI8wKi4SJEnHJwMu1SLERGpULDemmmlG9NWqBuRQVRDBE-Zkt3K13AzGtDH1ANeau0wD7r5lp86Ohxhyx4ZmrS_Shr3cEUt4XUj5pyiwwSwd_6Ea46rf4?key=R-I2A70sxgR2V7kbMU7JWw)**
---

### Step 8: Verify the File Transfer
After the transfer is complete, log into the EPC/ENB machine and check the destination directory to ensure the file was received:

```bash
ls /path/to/destination/
```

ls /home/katnap/ttransfer/received