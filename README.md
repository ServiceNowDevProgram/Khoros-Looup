# Khoros-Looup

A ServiceNow application for looking up user information from the Khoros API.

## Table of Contents
1. [Initial Setup and Access](#initial-setup-and-access)
2. [Viewing Existing Data](#viewing-existing-data)
3. [Loading New Records for Lookup](#loading-new-records-for-lookup)
4. [Running API Lookups](#running-api-lookups)
5. [Exporting Data](#exporting-data)

## Initial Setup and Access

1. Log into your ServiceNow instance
2. Navigate to the Khoros data table:
   - Check your favorites dropdown first - the table may already be saved there
<img width="522" height="252" alt="image" src="https://github.com/user-attachments/assets/f3c81de5-402a-4015-a88f-a0503caa57a7" />

   - If not in favorites, search for "Khoros data" in the application navigator

## Viewing Existing Data

The Khoros data table contains all user records. Key fields include:

- **Looked up**: `true` indicates the API has already checked this record
- **Email**: The user's email address
- **First name**: Retrieved from Khoros API
- **Last name**: Retrieved from Khoros API  
- **Login**: Khoros login username
- **ID**: Khoros user ID
- **SSO ID**: Single sign-on ID
- **View href**: Direct link to user's Khoros profile

## Loading New Records for Lookup

To add new email addresses for API lookup:

### 1. Prepare CSV File
- Create a CSV file (or use the one provided by Earl)
- First row should contain just: `email` (no quotations)
- Add all email addresses in column A, one per row

Example CSV format:
```
email
user1@company.com
user2@company.com
user3@company.com
```

### 2. Import Data
1. Navigate to **System Import Sets > Load Data**
   - *If you don't see this option, ask Earl to grant you access*
<img width="318" height="263" alt="image" src="https://github.com/user-attachments/assets/8968551c-bad5-413c-929a-006a538fee64" />


2. Fill out the Load Data form:
   - **Import set table**: Existing table
   - **Table**: u_khoros_data_import [x_snc_khoros_looku_u_khoros_data_import]
   - **Source of the import**: File
   - **File**: Upload your CSV file
   - **Sheet number**: Leave as 1
   - **Header row**: Leave as 1
<img width="1025" height="374" alt="image" src="https://github.com/user-attachments/assets/1e533d00-1fc6-4364-9843-ad89463cb964" />


3. Click **Submit**

### 3. Transform Data
1. Wait for the ImportProcessor to complete
2. Click **Run Transform**
<img width="756" height="410" alt="image" src="https://github.com/user-attachments/assets/0fbce1fc-1136-4d88-a6cc-d6d346c633a3" />

3. The system should auto-select "Khoros emails - x_snc_khoros_looku_khoros_data" under "Selected maps, run in order"
4. Click **Transform**
<img width="544" height="264" alt="image" src="https://github.com/user-attachments/assets/158274c0-5cc8-4af4-973a-67f829247239" />



### 4. Verify Import
- Return to the Khoros data table
- Sort by the "Created" column to see new records
<img width="304" height="167" alt="image" src="https://github.com/user-attachments/assets/7f1ca275-a838-42ea-8505-326ce43c60d4" />

- *Note: If records appear missing, they may already exist in the table*

## Running API Lookups

### Start the Lookup Process
1. Click the **"Begin lookups"** button (top right of the table view)
<img width="463" height="293" alt="image" src="https://github.com/user-attachments/assets/1a9b4328-b200-4781-b53b-192c2bd11a4d" />

2. The application will attempt to find ALL people with "Looked up = false" in the Khoros API

### How the Lookup Works
- The system tries to find each user in the Khoros API
- If a user isn't found initially, it tries once more (handles API timing issues)
- After two attempts OR when a user is found, "Looked up" changes to `true`
- **The lookup process is asynchronous** - your page won't auto-update

### Viewing Results
- Right-click the table header and select **Refresh** to see updated data
<img width="631" height="228" alt="image" src="https://github.com/user-attachments/assets/9a12afb0-9c83-4ae8-825c-a596d4cfd1db" />

- Successfully found users will have their information populated:
  - First name
  - Last name
  - Login
  - ID
  - SSO ID
  - Profile URL (view href)

### Re-running Lookups
To lookup a user again:
1. Change their "Looked up" field back to `false`
<img width="196" height="140" alt="image" src="https://github.com/user-attachments/assets/6c3e2d55-61e3-40ff-831d-0c9b71cd9e4a" />

2. Click **"Begin lookups"** button again

## Exporting Data

To export table data:
1. Right-click the table header
2. Select **Export**
3. Choose **Excel** format
<img width="419" height="471" alt="image" src="https://github.com/user-attachments/assets/1f76d72e-bb60-418d-8daa-5ce741386ec0" />


The exported file will contain all visible records and their current data.
