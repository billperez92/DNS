# <h1> Building Intuition for DNS </h1>

<img src="https://github.com/Kelsow96/Building-Intuition-for-DNS/assets/169297569/6bfbaa31-0873-4da5-b0fd-0b950082a531" width="400" />

<h2> What is Active DNS? </h2>

DNS stands for Domain Name System. It's like the internet's address book. When you type a website's domain name (like google.com) into your browser, your computer uses DNS to look up the IP address associated with that domain. This process lets your computer connect to the correct server and load the website. DNS also plays a crucial role in email delivery, network communication, and other internet services by translating human-readable domain names into machine-readable IP addresses.
<br>
<br/>

<h2> Overview </h2>

In this tutorial we'll demonstrate and practice DNS (Domain Name System) management and troubleshooting tasks within a Windows environment by doing a few exercises.

**A-Record Exercise:**

- Establishing connectivity between machines using DNS. Creating an A-record to map a hostname ("mainframe") to an IP address. Verifying successful DNS resolution after creating the A-record.

**Local DNS Cache Exercise:**

- Understanding and observing local DNS caching on client machines. Modifying DNS records and observing the impact on DNS caching. Flushing DNS cache to clear outdated entries and force DNS resolution.

**CNAME Record Exercise:**

- Creating a CNAME (Canonical Name) record to alias one hostname to another. Testing DNS resolution using CNAME records. Verify the CNAME resolution using the ping and nslookup commands.

These exercises cover essential concepts such as DNS record types (A-record, CNAME), DNS resolution, local DNS caching, and troubleshooting DNS issues. They are valuable for IT professionals to understand and manage DNS infrastructure effectively.

<h2> Environments and Technologies Used: </h2>

- Microsoft Azure (Virtual Machines/Compute)
- Domain Controller (DC-1)
- Client Machine (Client-1)
- DNS (Domain Name System) for managing hostname-to-IP address mappings
- Remote Desktop
- PowerShell
- Commands like nslookup, ping, ipconfig, and flushdns for DNS troubleshooting and management

<h2> Operating Systems Used: </h2>

- Windows 10
- Windows Server 2022

<h2> List of Prerequisites: </h2>

- Azure Account/Subscription
- Virtual Machines ([DC-1 and Client-1](https://github.com/Kelsow96/-Implementing-Active-Directory-On-Premises-in-Azure)) from previous tutorial and have Client-1 joined to the domain.

<h2> High-Level Steps: </h2>

**- A-Record Exercise:**
1. Log into DC-1 as your domain admin account.
2. Log into Client-1 as an admin.
3. Attempt to ping "mainframe" from Client-1 (it should fail).
4. Nslookup "mainframe" to confirm there's no DNS record.
5. Create a DNS A-record on DC-1 for "mainframe" pointing to DC-1’s Private IP address.
6. Ping "mainframe" from Client-1 again to verify it works.
 
**- Local DNS Cache Exercise:**
1. Change the mainframe's record address on DC-1 to 8.8.8.8.
2. Ping "mainframe" from Client-1 (it should still ping the old address).
3. Observe the local DNS cache on Client-1 (ipconfig /displaydns).
4. Flush the DNS cache (ipconfig /flushdns) and verify it's empty.
5. Ping "mainframe" again from Client-1 to see the new address.

**- CNAME Record Exercise:**
1. Create a CNAME record on DC-1 pointing "search" to "www.google.com."
2. Ping "search" from Client-1 and observe the CNAME record results.
3. Use nslookup on Client-1 to verify the CNAME record for "search."

<h2> Steps: </h2> 

**- A-Record Exercise:**

1. Log into DC-1 as your domain admin account.

![image](https://github.com/Kelsow96/Building-Intuition-for-DNS/assets/169297569/f7a59d56-1909-4165-bf9f-b9f70fe63e3a)
<br>
<br/>

2. Log into Client-1 as an admin.

![image](https://github.com/Kelsow96/Building-Intuition-for-DNS/assets/169297569/4d783c0b-1dcf-4e11-90c4-a6386709babd)
<br>
<br/>

3. Attempt to ping "mainframe" from Client-1 (it should fail).
 - In Powershell we'll ping "mainframe"
![image](https://github.com/Kelsow96/Building-Intuition-for-DNS/assets/169297569/b2091351-4f78-4cdc-acef-a6d4a7282f27)
<br>
<br/>

4. Nslookup "mainframe" to confirm there's no DNS record.
- In Powershell we'll type nslookup mainframe
![image](https://github.com/Kelsow96/Building-Intuition-for-DNS/assets/169297569/34dd8812-7810-488e-b902-2977f02ccb71)
<br>
<br/>

5. Create a DNS A-record on DC-1 for "mainframe" pointing to DC-1’s Private IP address.
- In DC-1 we'll navigate to "Tools" and then select "DNS". Within the DNS Manager window, select "DC-1", "Forward Lookup Zones" and finally "mydomain.com". Right-click "mydomain.com" and select "New Host (A or AAAA)". Within the New Host window, we'll type "mainframe" in the Name text box and DC-1's private IP address within IP address (10.0.0.4). Once filled out select "Add Host".
![Capture](https://github.com/Kelsow96/Building-Intuition-for-DNS/assets/169297569/a83af703-2bb7-46fc-9166-c6a2c3fa2629)
![Capture](https://github.com/Kelsow96/Building-Intuition-for-DNS/assets/169297569/7f19e1cf-3ba2-4d02-a90d-d6da034d0d41)
![Capture](https://github.com/Kelsow96/Building-Intuition-for-DNS/assets/169297569/719f8112-97fb-4df7-807f-62c3f900e75d)
![image](https://github.com/Kelsow96/Building-Intuition-for-DNS/assets/169297569/ae8edb96-2727-46ad-83da-55f08ef7e96d)
![image](https://github.com/Kelsow96/Building-Intuition-for-DNS/assets/169297569/c2058fd3-4043-4bc5-8f19-00a3cd0a4719)
<br>
<br/>

6. Ping "mainframe" from Client-1 again to verify it works.
- Back in Client-1 we'll ping "mainframe" again via Powershell and we should see a successful ping after creating a new A-record. If we use nslookup we should also see it. 
![image](https://github.com/Kelsow96/Building-Intuition-for-DNS/assets/169297569/32730f80-9414-4a99-8ad5-3277a96c4285)
![image](https://github.com/Kelsow96/Building-Intuition-for-DNS/assets/169297569/79210645-81c7-4268-ac14-9e41af504404)
<br>
<br/>

**- Local DNS Cache Exercise:**
1. Change the mainframe's record address on DC-1 to 8.8.8.8.
- In DC-1, right-click the "mainframe" record and select "Properties." We'll change the IP address on the mainframe's Properties page to "8.8.8.8." Then, select "Apply."

![Capture](https://github.com/Kelsow96/Building-Intuition-for-DNS/assets/169297569/bb583fee-29be-408c-add7-0a2654bbde77)
![Capture](https://github.com/Kelsow96/Building-Intuition-for-DNS/assets/169297569/0f003702-8266-4677-90da-38bb8edec5e3)
<br>
<br/>

2. Ping "mainframe" from Client-1 (it should still ping the old address).
- In Client-1 we'll ping "mainframe" via Powershell. We see it is the old IP address we gave in the previous step.

![Capture](https://github.com/Kelsow96/Building-Intuition-for-DNS/assets/169297569/9138b28a-3af8-4c8e-b25d-4b4dd39f9602)
<br>
<br/>

3. Observe the local DNS cache on Client-1 (ipconfig /displaydns).
- In Client-1 we'll observe the DNS cache by typing "ipconfig /displaydns". We confirm that "mainframe" has not updated to the new IP address we gave it.

![image](https://github.com/Kelsow96/Building-Intuition-for-DNS/assets/169297569/74cf5aef-25c9-451d-9ac9-7f7fb12243f3)
<br>
<br/>

4. Flush the DNS cache (ipconfig /flushdns) and verify it's empty.
- Within Powershell type "ipconfig /flushdns". YOU MUST BE RUNNING POWERSHELL IN ADMINISTRATOR MODE TO DO THIS SUCCESSFULLY. If you're not in administrator mode, close your Powershell window, navigate back to the app icon, right-click the icon, and select "Run as Administrator".

![image](https://github.com/Kelsow96/Building-Intuition-for-DNS/assets/169297569/53131894-88ed-4de0-bd06-7279e79e048a)
<br>
<br/>

5. Ping "mainframe" again from Client-1 to see the new address.
- We'll ping "mainframe" via Powershell. When we do, we see that the mainframe's IP address has been updated to the new IP address, which we gave 8.8.8.8. This is because we "flushed/cleared" our DNS cache, which was storing the old IP address, and when we pinged the mainframe again after "flushing", it retrieved and stored the new IP address within its DNS cache. 

![image](https://github.com/Kelsow96/Building-Intuition-for-DNS/assets/169297569/c44b62d2-eda0-4df6-93b2-ef2952c87cb3)
![image](https://github.com/Kelsow96/Building-Intuition-for-DNS/assets/169297569/e548c8c3-5371-4df9-9171-17e7712eafa0)
<br>
<br/>

**- CNAME Record Exercise:**
1. Create a CNAME record on DC-1 pointing "search" to "www.google.com."
- In DC-1 within the DNS Manager window, right-click "mydomain.com" and select "New Alias (CNAME)". We'll type "search" within the "Alias name" text box and we'll have that target host "www.google.com". Select "OK"

![Capture](https://github.com/Kelsow96/Building-Intuition-for-DNS/assets/169297569/e2dd362d-3133-4fa1-9242-de03f5eca7c3)
![image](https://github.com/Kelsow96/Building-Intuition-for-DNS/assets/169297569/f2a7ea4c-4c5a-414a-b63a-533ee2bdeaa1)
<br>
<br/>

2. Ping "search" from Client-1 and observe the CNAME record results.
- In Client-1, we'll ping "search" via Powershell. We should have a successful ping showing that "search" points towards the target host of www.google.com.

![image](https://github.com/Kelsow96/Building-Intuition-for-DNS/assets/169297569/47cea858-ff9d-4bed-89be-ebee521a8577)
<br>
<br/>

3. Use nslookup on Client-1 to verify the CNAME record for "search."
- In Client-1, we'll use "nslookup search" via Powershell. We see that DC-1 has a CNAME called "search" or "search.mydomain.com" (FQDN).

![image](https://github.com/Kelsow96/Building-Intuition-for-DNS/assets/169297569/91755750-8847-4f8a-84eb-79d6a68c1cd3)
<br>
<br/>

<h2> Summary </h2>

- These DNS exercises provided valuable hands-on experience in managing and troubleshooting DNS records and local DNS cache.
  - Key Learnings:
    - **A-Records:**
      - Essential for mapping domain names to IP addresses.
      - Ensure network devices can be located using human-readable names.

    - **DNS Cache Management:**
      - Understanding the functionality of local DNS cache.
      - Managing the DNS cache is crucial for promptly reflecting network changes.
      - Flushing the DNS cache can resolve issues with outdated IP addresses.

    - **CNAME Records:**
      - Useful for aliasing one domain name to another.
      - Simplifies DNS management and improves flexibility.

- These exercises reinforced the importance of proper DNS configuration and cache management in maintaining a functional and efficient network environment.
