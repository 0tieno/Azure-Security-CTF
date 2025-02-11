# Azure-Security-CTF

## ctf_1-CloudS3C_H4x

### 🚀 **Task 1:** Exposed Storage - Retrieve the Secret

- **Points:** 100
- **Category:** Storage Security
- **Time Limit:** 60 minutes
- **Level:** Easy

---

### **Scenario**

One of your storage accounts has been accidentally misconfigured, exposing sensitive information. However, the flag is not immediately obvious. You need to **find the vulnerable storage container, retrieve the hidden flag**, and be careful—**some files may mislead you!**

---

### 🔎 **Hints**

- **Hint 1:** There are two storage accounts, but only one is vulnerable.
- **Hint 2:** The exposed file **does not have a predictable name**. Think like an attacker—enumerate first.
- **Hint 3:** The correct flag is **not directly visible** inside the file—check the metadata!
- **Hint 4:** Not every file is useful. Some may contain **misleading flags** or security hints.
- **Hint 5:** If part of the flag is missing, check **who accessed the storage from logs**—it may be there!
- **Hint 6:** Sometimes, UI tools don’t show everything. You may need **Azure CLI or PowerShell**.

---

### 🎯 **Steps**

1️⃣ **Identify the Public Storage Account**

- One of the storage accounts is publicly accessible.
- Its name contains `"storagehackathon"`.

2️⃣ **Enumerate Storage Containers and List All Files**

- The vulnerable storage account has a **non-obvious** public container name.
- List all storage containers to find it.

3️⃣ **Find the Correct File Containing the Real Flag**

- There are **multiple files**, but not all contain the flag.
- Some files contain **misleading** or **partial** information.

4️⃣ **Extract the Hidden Flag from Metadata**

- The **correct file** does **not display the flag inside its content**.
- Use **Azure CLI or PowerShell** to extract metadata.
- The flag is stored under a **non-obvious key**, like `access_code_93ds7a`.

5️⃣ **Reconstruct the Full Flag (if needed)**

- The **last part of the flag** might be missing from metadata.
- Check **Azure Monitor logs** (hint: look at `"User-Agent"` or `"Access Key Used"`).
- Combine the retrieved parts to form the **final flag**.

---

### 🔑 **Flag Format:**

`CTF{secure_storage_fixed}`

✅ **Bonus:** If you identify **who accessed the storage from logs**, earn an extra **10 points!**

---

### 🚀 **Task 2: Secure the Storage**

Your objective is to **lock down** the misconfigured storage account and prevent unauthorized access.

### **Steps to Secure the Storage Account**

1️⃣ **Disable Public Access**

- Navigate to **ctfhackstorage → Configuration**.
- Find **AllowBlobPublicAccess** and set it to **Disabled**.
- Confirm changes and save.

💡 **Hint:** If public access is already disabled, verify that all containers in the storage account are private.

---

2️⃣ **Restrict SAS Token Permissions**

- Navigate to **Storage Explorer or Azure CLI**.
- Check the existing **SAS tokens** (any active or expired links?).
- **Regenerate a new SAS token** with the following settings:
    - **Permissions:** Read-only
    - **Expiry Time:** Set it to expire within **30 minutes**
    - **IP Restrictions:** Allow only a specific IP (your own)

💡 **Hint:** SAS tokens **can be leaked**. If an attacker has an old SAS URL, they might still access files until it expires.

---

3️⃣ **Enable Azure Monitor Logs**

- Go to **ctfhackstorage → Monitoring → Diagnostic Settings**.
- Click **+ Add diagnostic setting** and select:
    - ✅ **Blob Read & Write Events**
    - ✅ **Delete Events** (to track file tampering)
    - ✅ **Access Logs** (to see who accessed the storage)
- **Send logs to Log Analytics Workspace**.

💡 **Hint:** Logs help track **who accessed the flag before you**! Find the attacker’s IP from logs for bonus points.

---

4️⃣ **Deploy Microsoft Defender for Storage**

- Go to **Microsoft Defender for Cloud → Storage Accounts**.
- Turn on **Defender for Storage** for **ctfhackstorage**.
- Configure an alert for **public access attempts**.

💡 **Hint:** Defender for Storage automatically detects **anonymous access attempts and suspicious activity**.

---

### **🔗 Submission Requirements**

- Submit the **correct flag**.
- Provide **screenshots of securing the storage**.
- **Bonus:** Find the **attacker’s IP** from logs (`+10 points`).

---

### SUMMARY (Hidden from Participants)

---

### **🛠️ What This Adds to the Challenge**

### Task 1

This version makes **retrieving the flag more challenging**, forcing participants to:

- **Enumerate storage** (instead of guessing container names).
- **Analyze metadata** (instead of expecting the flag inside a file).
- **Cross-check logs** (to reconstruct missing flag parts).
- **Use CLI tools** (as UI tools might not display metadata properly).

🔥 This will make them **think like a cloud security analyst** rather than just running a basic scan.

### Task 2

✅ More **realism** (similar to real-world misconfigurations).

✅ Adds a **red-herring effect** (fake files and multiple storage accounts).

✅ Forces participants to explore **Azure logs and Defender for Storage**.

✅ Encourages **Azure CLI usage** rather than just UI-based solutions.

---

### **🏅 Points Breakdown by Category**

- **Basic Enumeration & Discovery (40 Points)**
- **Flag Extraction & Submission (30 Points)**
- **Security Hardening & Remediation (20 Points)**
- **Bonus Investigation (10 Points)**

### **🎯 Scoring Strategy**

- **Partial completion:** Award **half** the points for attempting but not fully completing a step.
- **Exceptional documentation & findings:** Grant **bonus** points if a participant provides extra insights or defense recommendations.
