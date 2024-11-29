### Step-by-Step Tutorial for DNS and CNAME Exercises on DC-1 and Client-1 in Azure (These are VMs I have previously set up)

#### **Pre-Requisite: Turn on VMs**
1. **Log into the Azure Portal**:
   - Open a browser and go to the [Azure Portal](https://portal.azure.com/).
   - Log in with your credentials.

2. **Start the VMs if they are off**:
   - In the left-hand menu, click on **"Virtual Machines"**.
   - Find **DC-1** and **Client-1** in the list of virtual machines.
   - If either VM is turned off, click on the **VM name**, then click **Start** to turn it on.

---

#### **A-Record Exercise (Mapping a Hostname to an IP Address)**

1. **Log into DC-1 as Domain Admin**:
   - On your computer (or through Remote Desktop), open the **Remote Desktop Connection (RDP)** tool.
   - Type the **IP address** or **DNS name** of DC-1 (e.g., `DC-1.mydomain.com`) and click **Connect**.
   - Enter the **username** and **password** for the domain admin account (e.g., `mydomain.com\jane_admin`).

2. **Log into Client-1 as Admin**:
   - Similarly, RDP into **Client-1** by entering its IP address or DNS name.
   - Use the same domain admin credentials to log in (e.g., `mydomain\jane_admin`).

3. **Try to Ping “mainframe” from Client-1**:
   - On **Client-1**, open **Command Prompt**:
     ```cmd
     ping mainframe
     ```

4. **Use nslookup to Verify DNS Issue**:
   - On **Client-1**, in **Command Prompt**:
     ```cmd
     nslookup mainframe
     ```

5. **Create an A-Record for “mainframe” on DC-1**:
   - On **DC-1**, open **DNS Manager**:
     - Press **Windows + R**, then type:
       ```cmd
       dnsmgmt.msc
       ```
     - In **DNS Manager**, expand **Forward Lookup Zones** and right-click on **mydomain.com**.
     - Select **New Host (A or AAAA)...**.
     - In **Name**, type `mainframe`.
     - In **IP address**, enter **DC-1's Private IP**.
     - Click **Add Host**, then **Done**.

6. **Go Back to Client-1 and Ping “mainframe”**:
   - On **Client-1**, open **Command Prompt** again:
     ```cmd
     ping mainframe
     ```

---

#### **Local DNS Cache Exercise**

1. **Change the DNS Record for “mainframe” on DC-1**:
   - On **DC-1**, open **DNS Manager** again (`dnsmgmt.msc`).
   - Locate the **mainframe** A-record, right-click it, and select **Properties**.
   - Change the IP address for **mainframe** to `8.8.8.8` (Google's DNS).
   - Click **OK**.

2. **Try to Ping “mainframe” Again from Client-1**:
   - On **Client-1**, in **Command Prompt**:
     ```cmd
     ping mainframe
     ```

3. **View the Local DNS Cache**:
   - On **Client-1**, type the following command:
     ```cmd
     ipconfig /displaydns
     ```

4. **Flush the DNS Cache**:
   - On **Client-1**, type:
     ```cmd
     ipconfig /flushdns
     ```

5. **Check the DNS Cache Again**:
   - After flushing the cache, view the DNS cache again:
     ```cmd
     ipconfig /displaydns
     ```

6. **Ping “mainframe” Again**:
   - On **Client-1**, ping **mainframe**:
     ```cmd
     ping mainframe
     ```

---

#### **CNAME Record Exercise**

1. **Create a CNAME Record for “search” on DC-1**:
   - On **DC-1**, open **DNS Manager** (`dnsmgmt.msc`).
   - In **Forward Lookup Zones**, right-click on **mydomain.com** and select **New Alias (CNAME)...**.
   - In the **Alias name** field, type `search`.
   - In the **Fully qualified domain name (FQDN)** field, type `www.google.com`.
   - Click **OK** to create the CNAME record.

2. **Ping “search” from Client-1**:
   - On **Client-1**, in **Command Prompt**:
     ```cmd
     ping search
     ```

3. **Use nslookup to Verify the CNAME Record**:
   - On **Client-1**, in **Command Prompt**:
     ```cmd
     nslookup search
     ```

---

#### **Finish the Lab**

1. **Stop the VMs in Azure** (if you're done for the day):
   - Go back to the **Azure Portal**.
   - Click on **"Virtual Machines"** in the left-hand menu.
   - Find **DC-1** and **Client-1**, select each VM, and click **Stop** to turn them off. This will save you costs, but the VMs will still exist for future labs.
