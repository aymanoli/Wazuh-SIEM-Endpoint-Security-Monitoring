# Configure and Execute FIM Using Custom Code 

## Step By Step

After setup Wazuh agent on Windows. Its default path is located  **C:\Program Files (x86)\ossec-agent** here the main file is ossec.cof 
* First of all, duplicate the file ossec.cof
* Run the file with the administrator user in any software like Notepad ++
* In the "File integrity monitoring" section, add your custom directory location where you want to perform the FIM
* Below is an image example
<img width="1920" height="691" alt="Capture" src="https://github.com/user-attachments/assets/e2771703-f7a3-42e2-a52b-b817b7a34438" />

* "My Custom Modification," I added another line where I want to perform the **FIM**
* Navigate to your path, create a text file, and write something inside it
<img width="1029" height="464" alt="Captue" src="https://github.com/user-attachments/assets/37445962-3a2e-4894-a49f-bad176780fce" />

* After that, you go to your Ubuntu Wazuh manager
* and run the Wazuh server, navigate to the File Integrity Monitoring
* Inside the dashboard, you can see the file add, modify, and deleted
<img width="1920" height="1080" alt="Screenshot From 2026-07-11 22-32-42" src="https://github.com/user-attachments/assets/c776335a-28c8-446a-a12c-31bd926d5d67" />

* Also, if you go to Events, you can see which file add, modified, and deleted
<img width="1920" height="1080" alt="Screenshot From 2026-07-11 16-10-07" src="https://github.com/user-attachments/assets/400440e9-6125-471e-a88e-0098351dacea" />

* in Inventory also you cas see the file Details
<img width="1920" height="1080" alt="Screenshot From 2026-07-11 22-33-12" src="https://github.com/user-attachments/assets/b98440f0-b4e7-4e9b-b76e-33ea36ab287d" />


