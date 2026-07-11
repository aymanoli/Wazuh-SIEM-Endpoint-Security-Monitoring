# Brute-force attempt investigation

Firstly, I entered multiple times to log in to the **Windows agent** with the wrong password to see what kind of alert was sent to me by Wazuh. After that, I got an alerts which was **"An account failed to log on."** 
<img width="1920" height="955" alt="Screenshot From 2026-07-11 10-08-45" src="https://github.com/user-attachments/assets/b499dfa1-71ef-4df6-869e-3da973a77ee4" />

Then I created a custom rule to investigate the brute force attack in **Server Management > Rules > Manage Rules Files > local_rules.xml** 

``` bash
<group name="windows,authentication,custom,">

  <rule id="100200" level="12" frequency="3" timeframe="120">
    <if_matched_group>authentication_failed</if_matched_group>
    <same_field>win.eventdata.ipAddress</same_field>
    <description>
      Custom: Windows brute-force detected (3 failures in 2 minutes)
    </description>
    <mitre>
      <id>T1110</id>
    </mitre>
  </rule>

</group>
```
<img width="1920" height="955" alt="Screenshot From 2026-07-11 23-52-57" src="https://github.com/user-attachments/assets/5a540901-62f5-4565-aa09-10ee57830841" />

### Code Explanation
* rule id = "100200" --> All custom rules start with 100xxx
* frequency="3" --> When the trigger has more than 3 failures happen
* timeframe="120" --> between 120s
* <if_matched_group> --> Refers to the main vendor rule authentication_failed_login section
<hr>

Now I tried again on my Agent machine three times with the wrong password in 2 minutes, after that I got an alert which was **Brute Force**
<img width="1920" height="955" alt="Screenshot From 2026-07-11 11-48-09" src="https://github.com/user-attachments/assets/5bbe3d08-b19f-4c67-a4df-1583397efc3d" />
