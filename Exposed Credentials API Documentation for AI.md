# **Exposed Credentials API**

**Base URL:** https://platform-api.securityscorecard.io/v1

## **Overview**

The Exposed Credentials API provides fast, scalable access to an extensive database of compromised credentials. It empowers security teams to proactively stop account takeover attacks and prevent breaches by identifying credential exposure across their organization and supply chain.

## **Definitions**

* **Target (Domain/Organization):** The system or domain being investigated for leaks (e.g., vendorX.com).  
* **Credential Owner (Domain/Organization):** The domain to which a leaked email address belongs (e.g., mycompany.com for the leaked credential user@mycompany.com).

## **Customer Impact & Key Use Cases**

* Secure Your Own Organization (1st Party Risk):  
  Instantly answer the critical question: "Show me every credential associated with my company's domains (@mycompany.com) that has been exposed in any breach, anywhere." This complete visibility allows security teams to force immediate password resets and neutralize threats before an attacker can use them.  
* Manage Your Supply Chain Risk (3rd Party Risk):  
  Go beyond scores to see hard evidence. Ask: "Show me all exposed credentials for my critical vendors (e.g., @vendorcorp.com)." This provides objective, data-driven proof of vendor risk, enabling you to enforce stricter security controls and protect your supply chain.

## **Authentication**

All requests to this API must include an Authorization header. The header value must be formatted as Token \<YOUR\_API\_TOKEN\>, replacing \<YOUR\_API\_TOKEN\> with your SecurityScorecard API token.

Header Example:  
Authorization: Token \<YOUR\_API\_TOKEN\>

## **Endpoints**

The API provides three key endpoints to drill down from a high-level view to specific, actionable data.

### **1\. Global Summary \- Companies**

Gives you the 10,000-foot view. This endpoint shows all breached companies and sites (Targets) that have exposed credentials belonging to your organization or vendor (Credential Owner). You can specify the credential\_owner\_domain to globally search where that owner's credentials are leaked across all other domains.

For example, to see where your organization's credentials are leaked across the entire internet, you would enter your own domain as the credential\_owner\_domain.

**Endpoint:** GET /exposed-credentials/summary

#### **Query Parameters**

| Parameter | Type | Description |
| :---- | :---- | :---- |
| credential\_owner\_domain | string | **(Required)** The domain of the credential owner to filter results. |
| page | integer | Page number, starting from 1\. (Default: 1\) |
| size | integer | Number of entries per page. (Min: 20, Max: 500, Default: 100\) |
| date\_from | string | (Optional) The start of the date range for the query (e.g., YYYY-MM-DD). |
| date\_to | string | (Optional) The end of the date range for the query (e.g., YYYY-MM-DD). |

#### **Success Response (200 OK)**

{  
  "total": 5,  
  "page": 1,  
  "size": 100,  
  "entries": \[  
    {  
      "domain": "google.com",  
      "affected\_subdomain\_count": 3,  
      "exposed\_credentials\_count": 150  
    },  
    {  
      "domain": "microsoft.com",  
      "affected\_subdomain\_count": 2,  
      "exposed\_credentials\_count": 75  
    }  
  \]  
}

### **2\. Company Summary \- Parent Domains**

Lets you drill down into a specific breached site (Target Domain, e.g., salesforce.com) to see which of its parent domains or subdomains contain exposed credentials belonging to the Credential Owner(s) you specify.

For example, to see where your company’s leaked credentials are on salesforce.com, you would use salesforce.com as the target\_domain in the path and provide your company's domain in the credential\_owner\_domains request body. You can also provide a list of vendor domains as credential\_owner\_domains to find their exposure on the target\_domain.

**Endpoint:** POST /exposed-credentials/summary/{target\_domain}

#### **Path Parameters**

| Parameter | Type | Description |
| :---- | :---- | :---- |
| target\_domain | string | **(Required)** The single target company domain to investigate (e.g., google.com). |

#### **Query Parameters**

| Parameter | Type | Description |
| :---- | :---- | :---- |
| page | integer | Page number, starting from 1\. (Default: 1\) |
| size | integer | Number of entries per page. (Min: 20, Max: 500, Default: 100\) |
| date\_from | string | (Optional) The start of the date range for the query (e.g., YYYY-MM-DD). |
| date\_to | string | (Optional) The end of the date range for the query (e.g., YYYY-MM-DD). |

#### **Request Body**

The request body must be a JSON object specifying the credential\_owner\_domains (as an array) to search for.

{  
  "credential\_owner\_domains": \["mycompany.com", "myvendor.com"\]  
}

#### **Success Response (200 OK)**

{  
  "total": 3,  
  "page": 1,  
  "size": 100,  
  "entries": \[  
    {  
      "domain": "firebase.com",  
      "exposed\_credentials\_count": 50  
    },  
    {  
      "domain": "gmail.com",  
      "exposed\_credentials\_count": 25  
    }  
  \]  
}

### **3\. Search Exposed Credentials**

Retrieves the specific, actionable exposed credential records (e.g., employee@mycompany.com) from a given breached site (Target Domain), so your team can take immediate action.

You specify the target\_domain (the breached site) in the URL path and provide one or more credential\_owner\_domains in the request body.

**Endpoint:** POST /exposed-credentials/records/{target\_domain}

#### **Path Parameters**

| Parameter | Type | Description |
| :---- | :---- | :---- |
| target\_domain | string | **(Required)** The target domain or subdomain (e.g., admin.firebase.com). |

#### **Query Parameters**

| Parameter | Type | Description |
| :---- | :---- | :---- |
| page | integer | Page number, starting from 1\. (Default: 1\) |
| size | integer | Number of entries per page. (Min: 20, Max: 500, Default: 100\) |
| date\_from | string | (Optional) The start of the date range for the query (e.g., YYYY-MM-DD). |
| date\_to | string | (Optional) The end of the date range for the query (e.g., YYYY-MM-DD). |

#### **Request Body**

The request body must be a JSON object specifying the credential\_owner\_domains and whether to exclude\_subdomains.

* credential\_owner\_domains: An array of domains to filter credentials by.  
* exclude\_subdomains: A boolean. If true, only credentials matching the exact target\_domain are returned. If false, credentials for subdomains of target\_domain are also included.

{  
  "credential\_owner\_domains": \["mycompany.com"\],  
  "exclude\_subdomains": false  
}

#### **Success Response (22 OK)**

{  
  "total": 30,  
  "page": 1,  
  "size": 100,  
  "entries": \[  
    {  
      "channel\_title": "Telegram",  
      "domain": "admin.firebase.com",  
      "filename": "credentials.txt",  
      "password": "password123",  
      "processed\_date": "2024-01-15T10:30:00Z",  
      "username": "user@mycompany.com",  
      "source\_type": "telegram",  
      "email": "user@mycompany.com",  
      "leak\_date": "2024-01-15T10:30:00Z",  
      "leaked\_on\_domain": "admin.firebase.com"  
    },  
    {  
      "channel\_title": "Discord",  
      "domain": "admin.firebase.com",  
      "filename": "leaked\_data.txt",  
      "password": "securepass456",  
      "processed\_date": "2024-01-14T15:45:00Z",  
      "username": "admin@mycompany.com",  
      "source\_type": "discord",  
      "email": "admin@mycompany.com",  
      "leak\_date": "2024-01-14T15:45:00Z",  
      "leaked\_on\_domain": "admin.firebase.com"  
    }  
  \]  
}  
