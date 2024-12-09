
<p align="center">
<img src="https://i.imgur.com/MwrkwEQ.png" alt="DNS Photo"/>
</p>

<h1>Understanding DNS</h1>
This lab focuses on DNS and its practical use. DNS is a fundamental IT concept, and while many sources cover the theory, this lab will involve configuring DNS records to see how it functions in practice. It builds on a previous lab where a client is joined to mydomain.com. I am logged in as Jane Doe (an admin account) on both the client and domain controller VMs. For the lab to work, you must be logged in as an administrator. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- Command Prompt

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 Pro (21H2)

<h2>DNS Configuration Steps</h2>


While logged in to the client as an admin, open the command prompt. When you ping "mainframe," it will fail. Running nslookup mainframe will show similar results, indicating no DNS record for it. To resolve this, create a DNS A-record for "mainframe" on the domain controller. On the domain controller, open DNS Manager and navigate to the domain under the Forward Lookup Zones tab (e.g., mydomain.com). Right-click and select New Host. Enter "mainframe" as the name and use the domain controller's IP address. Refresh the DNS server to update the new record. Afterward, return to the client and ping "mainframe" again to confirm it resolves.

![Screenshot 2024-12-06 180455](https://github.com/user-attachments/assets/d387bcdc-528a-4ba2-8004-a760e38fd889)

![Screenshot 2024-12-06 180856](https://github.com/user-attachments/assets/14d9a866-fbec-44cb-808c-45be4baf74c3)

![Screenshot 2024-12-06 180917](https://github.com/user-attachments/assets/882d5334-df4a-4b07-820d-c5d534847197)

In this exercise, we will demonstrate the DNS cache. On the domain controller, I changed the "mainframe" record's address to 8.8.8.8 (Google) and refreshed the DNS server. When pinging "mainframe" from the client, it will still resolve to the old IP address. Running the command ipconfig /displaydns shows that the DNS cache still contains the old IP. To update the cache, we need to clear it. Running ipconfig /flushdns will empty the cache, so the next time we ping "mainframe," the IP address will be updated to the new one. The ping will now show the new IP address for the record.

![Screenshot 2024-12-06 180951](https://github.com/user-attachments/assets/f7945ceb-ccce-45ef-9a1e-cc7fe78fb83a)

![Screenshot 2024-12-06 181109](https://github.com/user-attachments/assets/46817f4f-3d6f-408b-9c60-1125f2a4f941)

![Screenshot 2024-12-06 181158](https://github.com/user-attachments/assets/f4c4b206-863e-412b-9047-abd8a71fc11f)

Next, we will create a CNAME record on the DNS server to point "search" to Google. In the DNS Manager, go to the Forward Lookup Zones tab and select the domain. Create a new CNAME record named "search" and point it to Google. Refresh the server to save the changes. On the client, pinging "search" and running nslookup will return the results of the CNAME record.

![Screenshot 2024-12-06 181252](https://github.com/user-attachments/assets/950be9c0-f4e0-4394-b4e2-a4da0da13a6b)

![Screenshot 2024-12-06 181327](https://github.com/user-attachments/assets/c2dfa258-feef-4ef5-8d8e-8e84faa67d56)

<h2>Lessons Learned </h2>

This lab gave me a clearer understanding of how DNS operates on both a computer and the network. Changes to DNS records can cause connectivity issues if the DNS cache contains outdated information. To update the cache with new records, it must be cleared. While the ipconfig /flushdns command is commonly used in IT troubleshooting, I didn't fully grasp its importance until completing this lab. At the start, when pinging "mainframe," the DNS cache is checked first. If no result is found, the host file is checked, followed by the DNS server. I created a record on the DNS server to resolve "mainframe." A CNAME record maps an alias to another domain name, such as using "search" as an alias for Google.
