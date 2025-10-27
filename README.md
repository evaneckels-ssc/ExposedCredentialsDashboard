# SecurityScorecard Exposed Credentials Dashboard

A powerful, standalone web application for analyzing exposed credentials data using the SecurityScorecard API. This dashboard provides two analysis modes with an intuitive, professional interface for discovering and managing credential exposure across your organization and supply chain.

## 🌟 Key Features

### **📊 Dual Analysis Modes**
- **Global View**: Discover where your organization's credentials are leaked across the internet (3-panel flow)
- **Search View**: Targeted investigation of specific domains and vendors (2-panel flow)

### **🎯 Advanced Data Management**
- **Real-time API Integration**: Live data from SecurityScorecard's Exposed Credentials API
- **Smart Pagination System**: Configurable page sizes (20, 50, 100, 500 records per page)
- **Date Range Filtering**: Filter by leak date (date_from, date_to) across all API calls
- **Subdomain Control**: Exclude subdomains toggle for exact domain matching
- **Left-to-Right Flow**: Intuitive cascading navigation through data hierarchy
- **Complete Dataset Access**: Full pagination controls for browsing large result sets
- **Password Security**: Masked passwords by default with granular show/hide controls

### **🔍 Powerful Filtering & Search**
- **Date Range Filtering**: Filter credentials by leak date (date_from, date_to)
- **Real-time Search**: Search within each panel (breached sites, subdomains, credentials)
- **Multi-domain Support**: Search View supports multiple credential owner domains

### **📈 Export Capabilities**
- **CSV Export**: RFC 4180 compliant with proper field escaping
- **JSON Export**: Complete data structure with metadata
- **Sensitive Data Included**: Exports include password data for remediation
- **Professional File Naming**: Automatic date-stamped file names

### **🎨 Professional Interface**
- **Material Design Icons**: Google Material Symbols with optimized loading
- **Responsive Layout**: Mobile-friendly design with grid-based columns
- **Mode Toggle**: Easy switching between Global and Search views
- **Warning Banners**: Clear visual indicators for sensitive data
- **Loading States**: Real-time progress indicators during data loading

### **🔒 Security & Privacy**
- **Session-only Storage**: API keys stored in memory only (cleared on page refresh)
- **No Persistent Data**: Zero browser storage for maximum security
- **Password Masking**: All passwords masked by default with explicit show controls
- **Secure API Communication**: Direct HTTPS communication with SecurityScorecard

## 🚀 Getting Started

### **Prerequisites**
- Modern web browser (Chrome 90+, Firefox 88+, Safari 14+, Edge 90+)
- Valid SecurityScorecard API token with Exposed Credentials access
- Access to credential owner domains or target domains for investigation

### **Quick Setup**
1. **Download & Open**: Save `index.html` and open in your web browser
2. **Add API Key**: Click "Add API Key" → Enter your SecurityScorecard API token
3. **Choose Mode**: Select "Global View" or "Search View"
4. **Analyze Data**: Follow the workflow to discover exposed credentials

### **First-time Usage**
```
Open index.html → Add API Key → Choose Mode → Enter Domains → Explore Data
```

## 📖 Detailed Usage Guide

### **🌐 Global View Mode (1st Party Risk)**

**Purpose:** Answer the critical question: "Where are MY organization's credentials exposed across the entire internet?"

#### **Workflow:**

1. **Setup**
   - Enter your organization's domain in "Credential Owner Domain" (e.g., mycompany.com)
   - Optionally set date range filters
   - Select page size (default: 100)
   - Click "Load"

2. **Left Panel: Breached Sites**
   - View all external sites/domains where your credentials were leaked
   - See total breached sites and total exposed credentials
   - Search/filter the list
   - Click a breached site to drill down

3. **Center Panel: Subdomains**
   - Automatically loads when you select a breached site
   - Shows subdomains of the selected breached site containing your credentials
   - See credential count per subdomain
   - Click a subdomain to view specific credentials

4. **Right Panel: Credentials**
   - Displays actual leaked credentials (email/username, password, source)
   - Passwords masked by default with show/hide controls
   - **Exclude Subdomains**: Toggle to only show credentials for exact domain match
   - View leak dates, channels (Telegram, Discord, etc.), and filenames
   - Export to CSV/JSON for remediation
   - View complete JSON data for each credential

#### **Use Case Example:**
```
1. Enter: mycompany.com
2. See: 15 breached sites found (google.com, microsoft.com, etc.)
3. Click: google.com
4. See: 8 subdomains (firebase.com, gmail.com, etc.)
5. Click: firebase.com
6. See: 23 employee credentials leaked
7. Export: CSV for password reset campaign
```

### **🔎 Search View Mode (3rd Party Risk)**

**Purpose:** Targeted investigation: "Show me all exposed credentials for my vendors on a specific domain"

#### **Workflow:**

1. **Setup**
   - Enter target domain to investigate (e.g., google.com)
   - Enter one or more credential owner domains (comma-separated for vendors)
     - Example: mycompany.com, vendorA.com, vendorB.com
   - Optionally set date range filters
   - Select page size
   - Click "Search"

2. **Left Panel: Subdomains**
   - Shows subdomains of the target domain containing specified credentials
   - See credential count per subdomain
   - Click a subdomain to view credentials

3. **Right Panel: Credentials**
   - Same features as Global View
   - Shows credentials for ALL specified credential owners
   - **Exclude Subdomains**: Toggle to only show credentials for exact domain match
   - Export and remediation capabilities

#### **Use Case Example:**
```
1. Target: salesforce.com
2. Credential Owners: mycompany.com, criticalvendor.com
3. See: 12 subdomains on salesforce.com
4. Click: login.salesforce.com
5. See: 8 credentials (5 from mycompany.com, 3 from criticalvendor.com)
6. Export: Evidence for vendor security review
```

## 🔍 Filtering & Search Options

### **Date Range Filtering**
- **date_from**: Start date for leak date filtering (format: YYYY-MM-DD)
- **date_to**: End date for leak date filtering (format: YYYY-MM-DD)
- Applied to all three API endpoints
- Filters credentials based on when they were leaked/discovered
- Optional - leave blank to see all dates

### **Exclude Subdomains Toggle**
- **Purpose**: Control whether credentials from subdomains are included
- **When disabled (default)**: Shows credentials from selected domain AND all its subdomains
  - Example: If viewing `google.com`, includes credentials from `mail.google.com`, `drive.google.com`, etc.
- **When enabled**: Shows only credentials from the exact domain specified
  - Example: If viewing `google.com`, shows ONLY `google.com` credentials, excludes subdomains
- **Use Case**: Enable when investigating a specific service/subdomain without noise from related domains
- **Note**: Triggers new API call when toggled (resets to page 1)

### **Pagination**
- **page**: Current page number (starts at 1)
- **size**: Records per page (options: 20, 50, 100, 500)
- All three panels (Breached Sites, Subdomains, Credentials) have independent pagination
- Page controls: Previous/Next buttons with current page indicator
- Automatically hides when total results fit on one page

### **Search Filters (Client-Side)**
- Each panel has a search box for real-time filtering
- Searches domain names, email addresses, source types
- Does NOT trigger new API calls (filters already-loaded data)
- Case-insensitive matching

## 🔧 Technical Architecture

### **Core Technologies**
- **Frontend**: Pure HTML5, CSS3, Vanilla JavaScript
- **API**: SecurityScorecard Exposed Credentials API v1
- **Icons**: Google Material Symbols font
- **Styling**: Custom CSS with CSS Grid layout
- **Security**: Session-only data storage, no frameworks

### **API Integration**
```javascript
// Global Summary (Global View)
GET /exposed-credentials/summary?credential_owner_domain={domain}

// Parent Domains (Both Views)
POST /exposed-credentials/summary/{target_domain}
Body: {"credential_owner_domains": ["domain1", "domain2"]}

// Credentials Records (Both Views)
POST /exposed-credentials/records/{subdomain}
Body: {
  "credential_owner_domains": ["domain1"],
  "exclude_subdomains": false  // true = exact domain only, false = include subdomains
}

// Query Parameters (All Endpoints)
?page=1                    // Page number (default: 1)
&size=100                  // Records per page (20-500, default: 100)
&date_from=2024-01-01     // Optional: Start date filter
&date_to=2024-12-31       // Optional: End date filter

// Authentication
Authorization: Token {your_api_key}
```

### **Layout Structure**

**Global View:** 3-column grid layout
```
┌─────────────┬─────────────┬──────────────────┐
│   Breached  │  Subdomains │   Credentials    │
│    Sites    │   (Parent   │    (Records)     │
│  (Global    │   Domains)  │                  │
│   Summary)  │             │                  │
└─────────────┴─────────────┴──────────────────┘
  280px          280px           flex-grow
```

**Search View:** 2-column grid layout
```
┌─────────────┬──────────────────────────────┐
│  Subdomains │        Credentials           │
│   (Parent   │         (Records)            │
│   Domains)  │                              │
└─────────────┴──────────────────────────────┘
  320px              flex-grow
```

### **Data Processing**
- **Pagination**: Configurable (20-500 records per page)
- **Client-side Search**: Real-time filtering within each panel
- **State Management**: Global JavaScript variables for mode, selections, filters
- **Error Handling**: Graceful API error handling with user alerts
- **Loading States**: Visual feedback during all API operations

## 🛡️ Security Features

### **Data Privacy**
- **No Persistent Storage**: API keys cleared on browser refresh/close
- **Memory-only State**: All sensitive data stored in JavaScript variables
- **Session Isolation**: Each browser session requires fresh API key entry
- **No Cookies**: Zero browser cookie usage

### **Password Security**
- **Default Masking**: All passwords displayed as ••••••••
- **Granular Controls**: Show/hide per individual password
- **Global Toggle**: "Show All Passwords" for bulk operations
- **Visual Warnings**: Orange warning banner on credentials view
- **Export Awareness**: CSV/JSON exports include actual passwords for remediation

### **XSS Protection**
- **Input Sanitization**: All user inputs sanitized via escapeHtml()
- **HTML Escaping**: Domain inputs properly escaped before display
- **CSV Escaping**: RFC 4180 compliant field escaping for exports
- **No Eval**: Zero use of eval() or similar dangerous functions

### **API Security**
- **Token Authentication**: Secure token-based authentication
- **HTTPS Only**: All API communication over HTTPS
- **No Token Logging**: API tokens never logged to console
- **Error Message Sanitization**: API errors don't leak sensitive data

## 📊 Data Schema & Export Format

### **Credential Record Structure**
```json
{
  "email": "user@mycompany.com",
  "username": "user@mycompany.com",
  "password": "password123",
  "domain": "admin.firebase.com",
  "leaked_on_domain": "admin.firebase.com",
  "source_type": "telegram",
  "channel_title": "Leaked Credentials",
  "filename": "credentials.txt",
  "leak_date": "2024-01-15T10:30:00Z",
  "processed_date": "2024-01-15T10:30:00Z"
}
```

### **CSV Export Columns**
- Email/Username
- Password (actual password, not masked)
- Domain (leaked_on_domain)
- Source Type (telegram, discord, etc.)
- Channel Title
- Filename
- Leak Date
- Processed Date

### **CSV Export Features**
- **RFC 4180 Compliant**: Proper quoting and escaping
- **UTF-8 BOM**: Byte Order Mark for proper character encoding
- **Special Characters**: Handles commas, quotes, newlines
- **Professional Naming**: `exposed-credentials-{subdomain}-{date}.csv`

### **JSON Export Structure**
```json
{
  "total": 23,
  "subdomain": "firebase.com",
  "exported_at": "2024-10-27T12:00:00Z",
  "entries": [
    { /* credential record */ },
    { /* credential record */ }
  ]
}
```

## 🎯 Use Case Support

### **1st Party Risk (Secure Your Own Organization)**

**Objective:** Proactively identify and remediate exposed employee credentials

**Workflow:**
1. Use Global View mode
2. Enter your organization's domain
3. Discover all breached sites with your credentials
4. Drill down to specific credentials
5. Export CSV for IT team to force password resets
6. Track remediation progress over time

**Benefits:**
- Complete visibility across all breach sources
- Actionable data for immediate response
- Evidence for security metrics and reporting

### **3rd Party Risk (Supply Chain Monitoring)**

**Objective:** Assess and manage vendor credential exposure

**Workflow:**
1. Use Search View mode
2. Enter target domain (e.g., vendor's primary service)
3. Enter multiple vendor domains as credential owners
4. Compare exposure across vendors
5. Export evidence for security reviews
6. Enforce stricter security controls

**Benefits:**
- Objective, data-driven proof of vendor risk
- Support vendor risk scoring and decisions
- Evidence for compliance and audit requirements

### **Incident Response**

**Objective:** Quickly investigate potential credential compromise

**Workflow:**
1. Receive alert about credential exposure
2. Use appropriate mode based on alert details
3. Quickly locate affected credentials
4. View leak dates, sources, and channels
5. Export data for forensic analysis
6. Coordinate remediation actions

**Benefits:**
- Fast, comprehensive investigation
- Context-rich data (source, date, channel)
- Export for incident documentation

## 📁 Project Structure

```
ExposedCredentialsDashboard/
├── index.html              # Complete dashboard application
├── README.md               # This documentation
└── .gitignore              # Excludes temporary files (if using git)
```

## 🔄 Development & Customization

### **No Build Process Required**
- Single HTML file with embedded CSS and JavaScript
- Open directly in browser for immediate use
- Easy deployment to any web server or S3 bucket
- No npm, webpack, or build tools needed

### **Customization Points**
- **Styling**: Modify CSS variables and rules in `<style>` section
- **API Endpoints**: Update `API_BASE_URL` constant if needed
- **Page Sizes**: Modify `pageSizeSelect` options
- **Date Formats**: Customize `formatDate()` function
- **Export Formats**: Extend `exportToCsv()` or `exportToJson()` functions

### **Color Scheme**
```css
Primary Purple: #594ef3
Secondary Purple: #512b70
Text Dark: #231f20
Text Muted: #8d9a9e
Border: #e3e3e3
Background: #f5f5f5
```

## 🎯 Best Practices

### **Optimal Usage**
1. **Start Broad**: Use Global View to understand full exposure landscape
2. **Drill Down**: Follow left-to-right flow for systematic investigation
3. **Export Evidence**: Leverage CSV/JSON exports for documentation
4. **Verify Sources**: Review source URLs and channels for validation
5. **Regular Monitoring**: Schedule periodic checks (monthly, quarterly)

### **Performance Tips**
- **Page Size**: Start with 100 records, increase to 500 for bulk operations
- **Date Filters**: Use date ranges to focus on recent leaks
- **Exclude Subdomains**: Enable to filter credentials to exact domain match only (reduces result set)
- **Search Functions**: Use sidebar search to filter large result sets
- **Pagination**: Navigate pages systematically for comprehensive review

### **Security Tips**
- **API Key Management**: Never share or commit API keys to version control
- **Session Planning**: Plan work sessions around session-only storage
- **Export Security**: Treat CSV/JSON exports as highly sensitive
- **Password Visibility**: Use show/hide controls judiciously
- **Browser Privacy**: Consider private/incognito mode for extra security

## 📞 Support & Resources

### **SecurityScorecard API**
- [API Documentation](https://securityscorecard.readme.io/)
- [API Token Management](https://platform.securityscorecard.io/)
- [Exposed Credentials API Guide](https://platform-api.securityscorecard.io/)

### **Browser Compatibility**
- **Recommended**: Chrome 100+, Firefox 95+, Safari 15+, Edge 100+
- **Required Features**: ES6+, Fetch API, CSS Grid, Material Symbols font

### **Common Issues**

**Q: API calls failing with 401 Unauthorized**
- A: Verify your API key is correct and has Exposed Credentials permissions

**Q: No data showing after Load/Search**
- A: Check domain spelling, verify API key, ensure domains exist in SecurityScorecard

**Q: Passwords not showing when toggled**
- A: Some credentials may have null/empty password fields (will show "N/A")

**Q: Export not working**
- A: Ensure browser allows downloads, check that data is loaded in detail pane

**Q: API key lost after page refresh**
- A: This is by design for security - re-enter API key each session

---

**Built by Evan Eckels** - SecurityScorecard

*Designed and developed to provide enterprise-grade exposed credentials analysis through an intuitive web interface*

For questions, issues, or feature requests, contact: **evan.eckels@securityscorecard.io**

---

## 🏆 Built for Professional Security Analysis

**Secure • Fast • Comprehensive • User-friendly**

**Protect your organization and supply chain from credential-based attacks**
