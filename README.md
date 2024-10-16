# Investigating-a-Security-Incident-with-Chronicle

In this project, I will use Chronicle (now known as Google Security Operations), a cloud-native tool, to investigate a security incident involving phishing in a hypothetical scenario.

## Scenario
I am a security analyst at a financial services company. I receive an alert that an employee received a phishing email in their inbox. You review the alert and identify a suspicious domain name contained in the email's body: **signin.office365x24.com**. I need to determine whether any other employees have received phishing emails containing this domain and whether they have visited the domain.

To begin, I will go to the [Google SecOps website](https://cloud.google.com/security/products/security-operations) that provides a search utility. Entering **signin.office365x24.com** into the search bar and selecting search will produce a drop-down list. Beneath the "Domains" title, **signin.office365x24.com** is listed, showing that the domain exists within it's ingested data.
![image](https://github.com/user-attachments/assets/c1c6357e-b35c-4508-806b-72e97431b496)

Clicking on **signin.office365x24.com** in the drop-down list will complete the search.

After performing a domain search, you'll be in the domain view:
1. **VT CONTEXT**: This section provides the VirusTotal information available for the domain. 
2. **WHOIS**: This section provides a summary of information about the domain using WHOIS, a free and publicly available directory that includes information about registered domain names, such as the name and contact information of the domain owner. In cybersecurity, this information is helpful in assessing a domain's reputation and determining the origin of malicious websites. 
3. **Prevalence**: This section provides a graph which outlines the historical prevalence of the domain. This can be helpful when you need to determine whether the domain has been accessed previously. Usually, less prevalent domains may indicate a greater threat. 
4. **RESOLVED IPS**: This insight card provides additional context about the domain, such as the IP address that maps to signin.office365x24.com, which is 40.100.174.34. Clicking on this IP will run a new search for the IP address in Chronicle. Insight cards can be helpful in expanding the domain investigation and further investigating an indicator to determine whether there is a broader compromise.
5. **SIBLING DOMAINS**: This insight card provides additional context about the domain. Sibling domains share a common top or parent domain. For example, here the sibling domain is listed as login.office365x24.com, which shares the same top domain office365x24.com with the domain you're investigating: signin.office365x24.com.
6. **TIMELINE**. This tab provides information about the events and interactions made with this domain. Click EXPAND ALL to reveal the details about the HTTP requests made including GET and POST requests.  A GET request retrieves data from a server while a POST request submits data to a server.
7. **ASSETS**. This tab provides a list of the assets that have accessed the domain.
![Screenshot 2024-10-16 144810](https://github.com/user-attachments/assets/4f1bf6b9-f347-43fe-927a-515be0c86e2e)

Now that I have access to this information, I will start by utilizing the available threat intelligence information to determine if the domain is malicious.
Click on **VT CONTEXT** to analyze the available VirusTotal information about this domain, we see that twelve security vendors have flagged this domain as malicious.
![image](https://github.com/user-attachments/assets/9a284f2c-613b-460d-9647-bedf5321f888)

Following this, I will investigate the affected assets and events.
Information about the events and assets relating to the domain are separated into the two tabs: **Timeline** and **Assets**. **Timeline** shows the timeline of events that includes when each asset accessed the domain. **Assets** list hostnames, IP addresses, MAC addresses, or devices that have accessed the domain.
Under the **Assets** tab, we see that six assetts have accessed the domain, along with their date and time of access.
Under the **Timeline** tab and clicking **EXPAND ALL**, reveals the details about the HTTP requests made, including GET and POST requests. The POST information is especially useful because it means that data was sent to the domain. It also suggests a possible successful phish. For more details about the connections, open the raw log viewer by clicking the open icon.
![image](https://github.com/user-attachments/assets/380b6950-a4dc-4a50-a822-48ab4b198329)

Attackers sometimes reuse infrastructure for multiple attacks. In these cases, multiple domain names resolve to the same IP address. To investigate this, I will investigate the IP address(es) found under the **RESOLVED IPS** insight card to identify if the signin.office365x24.com domain uses another domain.
For IP address **40.100.174.34**, I will evaluate the search results for this IP address and take note of the following:
**TIMELINE**: For any additional POST request. A new POST suggests that an asset may have been phished.
![image](https://github.com/user-attachments/assets/02343ebe-7ffb-424b-9354-e8770b857edd)

**ASSETS**: For any additional affected assets.
![image](https://github.com/user-attachments/assets/58395105-12f1-4f17-9ec6-276f29d9c939)

**DOMAINS**: For any additional domains associated with this IP address.
![image](https://github.com/user-attachments/assets/1af4e476-5540-4890-b473-3c78e741455f)

After reviewing the threat intelligence available, including the VirusTotal information, multiple domain names associated with the IP address, additional POST requests, and additional affected assets, I am able to determine that it is a malicious domain. Further investigation will be conducted to determine whether any other employees have received phishing emails containing this domain and whether they have visited the domain.
