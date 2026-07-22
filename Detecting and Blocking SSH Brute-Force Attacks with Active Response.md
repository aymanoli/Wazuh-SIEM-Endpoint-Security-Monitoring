# Detecting and Blocking SSH Brute-Force Attacks with Active Response

To complete this task, we have to know a few things first.
1. What is an SSH Brute Force Attack? <br>
SSH brute-force attack is a hacking technique that attackers use to try different combinations of usernames and passwords to gain access to the remote server.
2. How Wazuh can detect it, respond to it, and stop it? <br>
**Detect:** Wazuh's Active Response module executes a script to block the IP address of the attacker when rule 5763 - SSHD brute force trying to get access to the system triggers. <br>
**Respond:** The Wazuh Manager instantly matches that Rule ID to an <active-response> block configured in the Wazuh server's ossec.conf file. <br>
**Stop:** The Wazuh agent executes the firewall-drop script (Linux iptables) or netsh command (Windows) to instantly ban the attacker's source IP address. <br>

## Detecting a brute-force attack

To detect a brute-force attack, we need three **Endpoints** attacker, victim and server machine. For an active response, you need a RHEL or Ubuntu system (recommended: 3 Ubuntu machines)
| Users | Endpoints | Description |
|-------|-----------|-------------|
| Attacker | Ubuntu | Attacker endpoint that performs brute-force attacks. |
| Victim | RHEL / Ubuntu | Victim endpoint of SSH brute-force attacks. |
| Victim | Windows | Victim endpoint of SSH brute-force attacks. |
| Wazuh Server | Windows / Ubuntu | Setup Wazuh server in this machine |

### Configuration brute-force
<hr>

<span>1.</span>&ensp; On the attacker endpoint, install Hydra
```bash
sudo apt update
sudo apt install -y hydra
```
<span>2.</span>&ensp; Checking a ping victim machine from the attacker machine 
```bash
ping <victim_ip>
```
<img width="1191" height="487" alt="Screenshot from 2026-07-22 15-46-44" src="https://github.com/user-attachments/assets/e85373a4-eb24-4916-bd64-4412840e60d6" /><br><br>

<span>3.</span>&ensp; On the attacker machine, have to create a text file with 10 random passwords.
```bash
nano password.txt
```
<img width="857" height="487" alt="Screenshot from 2026-07-22 15-45-59" src="https://github.com/user-attachments/assets/421a6712-94b6-4a57-b065-5927a7fe9b07" /><br><br>

<span>4.</span>&ensp; Create a user on the victim machine to execute the attack on this specific user
```bash
sudo adduser <select any name>
```
<img width="1676" height="665" alt="Screenshot from 2026-07-22 15-50-54" src="https://github.com/user-attachments/assets/ff20811a-8bc6-470a-9c5e-bd63074533d6" /><br><br>


<span>5.</span>&ensp; Run Hydra brute-force attack from the attacker endpoint
```bash
sudo hydra -l badguy -P <PASSWD_LIST.txt> <VICTIM_IP> ssh
```
<img width="1920" height="955" alt="Screenshot from 2026-07-22 15-54-38" src="https://github.com/user-attachments/assets/f9a66797-9b4b-4e74-815b-8939cc978f99" /><br><br>

<span>6.</span>&ensp; Now visualize the alert data in the Wazuh dashboard from the **Threat Hunting** modules.
<img width="1920" height="955" alt="Screenshot From 2026-07-22 15-55-21" src="https://github.com/user-attachments/assets/d10b2fb8-f09d-4dc2-846d-b39c186c19ef" />
Rule ID 5760: indicates an SSH authentication failure. system is showing an alert. <br>
Rule ID 5763: SSH brute-force attack is in progress, multiple failed login attempts from the same source IP address in a short time. <br>
Rule ID 5503: System detects a failed user login attempt with Linux PAM (Pluggable Authentication Module) <br>

<br><br>
## Blocking SSH Brute-Force Attacks with Active Response


<span>1.</span>&ensp; Open the Wazuh server machine and paste the command below in the command line in the &lt;command> block called **firewall-drop**. Don't need to do anything
```bash
sudo nano /var/ossec/etc/ossec.conf
```
<img width="1920" height="955" alt="Screenshot From 2026-07-22 15-58-14" src="https://github.com/user-attachments/assets/5ab9c24d-2f02-4a3b-9054-1c34f725f621" /><br><br>


<span>2.</span>&ensp; Now add the &lt;active-response> block at the bottom of the Wazuh server
```bash
<ossec_config>
  <active-response>
    <disabled>no</disabled>
    <command>firewall-drop</command>
    <location>local</location>
    <rules_id>5763</rules_id>
    <timeout>180</timeout>
  </active-response>
</ossec_config>
```
<img width="1920" height="955" alt="Screenshot From 2026-07-22 15-58-51" src="https://github.com/user-attachments/assets/39f99764-d1ab-49d7-8f9f-ee522f419194" /><br><br>


<span>3.</span>&ensp; Restart the Wazuh manager service to apply the changes.
```bash
sudo systemctl restart wazuh-manager
```

<span>4.</span>&ensp; Now check ping from the attack machine to the victim machine to ensure the connection is all right.
```bash
ping <victim_IP>
```


<span>5.</span>&ensp; Run the hydra brute-force attack from the attack machine to the victim machine
```bash
sudo hydra -t 4 -l <victim_username> -P <PASSWD_LIST.txt> <victim_ip> ssh
```
<img width="1920" height="705" alt="Screenshot from 2026-07-22 16-04-48" src="https://github.com/user-attachments/assets/51e8b9bd-ee7e-470d-b040-18f96624b209" /><br><br>


<span>6.</span>&ensp; Now the Wazuh dashboard rule ID 5763 get brute-force access and rule ID 651 blocked the brute-force by firewall-drop with active response
<img width="1920" height="955" alt="Screenshot From 2026-07-22 16-05-55" src="https://github.com/user-attachments/assets/2637f668-b04b-448d-b689-64b71b8cffe9" /><br><br>


<span>7.</span>&ensp; The active response triggers a corresponding alert and gets the attacker's IP address on the Wazuh dashboard.
<img width="1920" height="955" alt="Screenshot From 2026-07-22 16-18-37" src="https://github.com/user-attachments/assets/4bbeb079-7717-43ed-922d-279d6995ed03" /><br><br>


<span>8.</span>&ensp; In the victim machine, automatically generate the active response log in the **sudo nano /var/ossec/etc/ossec.conf**
```bash
<localfile>
  <log_format>syslog</log_format>
  <location>/var/ossec/logs/active-responses.log</location>
</localfile>
```
<img width="1920" height="714" alt="Screenshot from 2026-07-22 16-11-11" src="https://github.com/user-attachments/assets/35422bf7-6ffe-4d35-afe1-4d2700e17572" />





















