### Step-by-Step Tutorial for DNS and CNAME Exercises on DC-1 and Client-1 in Azure

#### **Pre-Requisite: Turn on VMs**
1. **Log into the Azure Portal**:
   - Open a browser and go to the [Azure Portal](https://portal.azure.com/).
   - Log in with your credentials.

2. **Start the VMs if they are off**:
   - In the left-hand menu, click on **"Virtual Machines"**.
   - Find **DC-1** and **Client-1** in the list of virtual machines.
   - If either VM is turned off, click on the **VM name**, then click **Start** to turn it on.
     
     ![image](https://github.com/user-attachments/assets/277a6f0b-b5eb-4714-82e4-8d3e053eea23)


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
   - On **Client-1**, open **Powershell**:
     ```cmd
     ping mainframe
     ```
     ![image](https://github.com/user-attachments/assets/e1b83cb4-2767-4a85-bc9a-67abc3e713b3)



4. **Use nslookup to Verify DNS Issue**:
   - On **Client-1**, in **Powershell**:
     ```cmd
     nslookup mainframe
     ```
     ![image](https://github.com/user-attachments/assets/21481263-a14a-416c-88f3-628ee844f294)

5. **Create an A-Record for “mainframe” on DC-1**:
   - On **DC-1**, open **DNS Manager**:
     - Press **Windows + R**, then type:
       ```cmd
       dnsmgmt.msc
       ```
       ![image](https://github.com/user-attachments/assets/81d07b11-b806-4fe0-9bcb-d2b990437b2e)

     - In **DNS Manager**, expand **Forward Lookup Zones** and right-click on **mydomain.com**.
       ![image](https://github.com/user-attachments/assets/f7d1cfc5-52ba-4217-9abc-d6c7d7d325d3)
       ![image](https://github.com/user-attachments/assets/2c609d72-a926-497a-91c4-df3603579f41)
       ![image](https://github.com/user-attachments/assets/50f0911a-9b25-44b8-a7fb-c3398d493a44)


     - Select **New Host (A or AAAA)...**.
       ![image](https://github.com/user-attachments/assets/25b20ab4-cdb8-4ce4-adf6-fc635d4f9c6e)

     - In **Name**, type `mainframe`.
     - In **IP address**, enter **DC-1's Private IP**.
     - Click **Add Host**, then **Done**.
         ![image](https://github.com/user-attachments/assets/704d550e-f064-4111-badc-39b1ef7f3a01)

       

6. **Go Back to Client-1 and Ping “mainframe”**:
   - On **Client-1**, open **Powershell** again:
     ```cmd
     ping mainframe
     ```
   
   ![image](https://github.com/user-attachments/assets/680539ba-29d1-460e-8c89-e23b5571c0c8)

---

#### **Local DNS Cache Exercise**

1. **Change the DNS Record for “mainframe” on DC-1**:
   - On **DC-1**, open **DNS Manager** again (`dnsmgmt.msc`).
   - Locate the **mainframe** A-record, right-click it, and select **Properties**.
   - Change the IP address for **mainframe** to `8.8.8.8` (Google's DNS).
   - Click **OK**.
     
     ![image](https://github.com/user-attachments/assets/2bc64027-ab9b-4e47-9f89-55baa8d17a39)

     ![image](https://github.com/user-attachments/assets/73cd1ece-ded0-4198-b9e3-9b4b6e0ea6e2)


2. **Try to Ping “mainframe” Again from Client-1**:
   - On **Client-1**, in **Powershell**:
     ```cmd
     ping mainframe
     ```
     - Although we have changed the IP address the computer located (10.0.04) in the Local DNS Cache

       ![image](https://github.com/user-attachments/assets/ba132c12-f54f-4f9e-ac59-b0e09028be86)
     


3. **View the Local DNS Cache**:
   - On **Client-1**, type the following command:
     ```cmd
     ipconfig /displaydns
     ```
     ![image](https://github.com/user-attachments/assets/5573c8f8-5ea6-470c-a23d-787d2475508f)


4. **Flush the DNS Cache as an Admin**:
   - On **Client-1**, type:
     ```cmd
     ipconfig /flushdns
     ```
     ![image](https://github.com/user-attachments/assets/3a1a6579-45de-4fe6-80ff-8778343e3de8)


5. **Check the DNS Cache Again**:
   - After flushing the cache, view the DNS cache again:
     ```cmd
     ipconfig /displaydns
     ```
     ![image](https://github.com/user-attachments/assets/6da9e07e-88b3-4207-9a3b-be91031cd3aa)


6. **Ping “mainframe” Again**:
   - On **Client-1**, ping **mainframe**:
     ```cmd
     ping mainframe
     ```
     ![image](https://github.com/user-attachments/assets/d671863c-2588-4f6e-82e3-8d3033afe84c)


---

#### **CNAME Record Exercise**

1. **Create a CNAME Record for “search” on DC-1**:
   - On **DC-1**, open **DNS Manager** (`dnsmgmt.msc`).
   - In **Forward Lookup Zones**, right-click on **mydomain.com** and select **New Alias (CNAME)...**.

      ![image](https://github.com/user-attachments/assets/36198c37-1663-45fb-b0a3-06041dfef48b)

   - In the **Alias name** field, type `search`.
     ![image](https://github.com/user-attachments/assets/662ec83f-3350-4b4b-8154-35094e86bb58)

   - In the **Fully qualified domain name (FQDN)** field, type `www.google.com`.
     ![image](https://github.com/user-attachments/assets/f2e41db0-7082-4106-bf17-f303e737b5f7)

   - Click **OK** to create the CNAME record.
     ![image](https://github.com/user-attachments/assets/90f7d849-dfa9-4c46-84b9-e1ea04bf09f6)


2. **Ping “search” from Client-1**:
   - On **Client-1**, in **Powershell**:
     ```cmd
     ping search
     ```
     ![image](https://github.com/user-attachments/assets/7f8331d1-d57f-4c83-8fb5-46557e5737ed)


3. **Use nslookup to Verify the CNAME Record**:
   - On **Client-1**, in **Powershell**:
     ```cmd
     nslookup search
     ```
     ![image](https://github.com/user-attachments/assets/402a98d4-21bc-42c7-a5a8-e610e130915e)


---

#### **Finish the Lab**

1. **Stop the VMs in Azure** (if you're done for the day):
   - Go back to the **Azure Portal**.
   - Click on **"Virtual Machines"** in the left-hand menu.
   - Find **DC-1** and **Client-1**, select each VM, and click **Stop** to turn them off. This will save you costs, but the VMs will still exist for future labs.
