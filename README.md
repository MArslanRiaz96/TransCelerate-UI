# Running SDR UI Application in local
### Pre-requisites

1. Install latest version of [Node.js](https://nodejs.org).

2. After installing Node.js, install the Angular CLI globally in cmd prompt using the command below:

3. Active User Account in Azure Active Directory 
```
npm install -g @angular/cli
```
3. Validate if node and npm is installed using below commands in cmd prompt. If it has installed properly, it will show the version installed.
```
node -v
```
```
npm -v
```
4. Try repeating the installation, if there is any issue.
### How to setup code

1. Clone the repo into a local directory using below git command.

```shell
git clone "repo_url"
```
Once repo is cloned, open a terminal window and go to the project root folder using the command as mentioned below.

```shell
cd SDR%20UI/SDR-WebApp
```

2. After navigation to the root folder path as mentioned above, run `npm install` to install the required libraries.
3. Create `environment.development.ts` file under environments folder, and replace the values of the keys with values of the target environment.
And this file is not committed, as it is ignored in `.gitignore` file.
SDR API URL and other secrets are configured in `src/environments/environment.ts` file as shown below. While hosting SDR UI on Azure Platform the values of the keys will be replaced with the environment specific values from DevOps during deployment.

```
export const environment = {
  production: true,
  bypassAuth: false,
  tenantId: '{#AzureAd-TenantId#}', // Azure AD Tenant ID
  authority: '{#AzureAd-Authority#}', // Azure AD login URL (e.g https://login.microsoftonline.com/<tenantId>/)
  clientId: '{#AzureAd-ClientId#}', // Azure App Registration Client ID for UI Application
  Audience: '{#AzureAd-Audience#}', // Audience Scope for UI Application

  redirectUrl: '{#AzureAd-RedirectUrl#}', // HomePage URL of UI application (e.g. : https://localhost:4200/home)
  loginUrl: '{#AzureAd-LoginUrl#}', // Login Page/Root URL of UI application  (e.g. : https://localhost:4200)

  BASE_URL:'{#Apim-BaseUrl#}', // Backend API Root URL (e.g. : https://sdr-apim.azure-api.net/)
  envName: '{#Env-Name#}', // Environment Name (e.g. : Dev/QA/Prod)
};
```

### Build and Run Application

1. Once code setup is done, run the project locally using the below command.

```shell
ng serve
```

2. After code is compiled successfully, open `https://localhost:4200/` in browser to run the application.

For any changes in source code, angular terminal recompiles the code and the browser will reload the application.

3. Use the below commands to commit and push the changes to repo. 

```shell
git commit -m 'message for commit'
git push
```

# Base solution structure

The solution has the following structure:

```
  .
  ├── src
      ├── app
      │   ├── core
      │   ├── features
      │   └── shared
      ├── environments
      │   ├── environment.ts
      ├── styles

```
**core** - contains files related to core angular structure - app.component.ts, app.module.ts, app-routing.module.ts.

**features** - contains application feature modules - dashboard,login,search-study,admin,reports,study-compare,soa.

**shared** - contains application shared modules - audit-trail, breadcrumb, error-component, footer, header, menu, modal-component, study-element-description, version-comparison, simple-search.

**environment.ts** - contains files to configure service URLs and other environment specific secrets.

**styles** - contains common CSS stylesheets.

**Application Authentication :**
- The application uses  Microsoft Authentication Library (MSAL) for user authentication.
- Users with valid credentials (registered in cloud active directory) can login to the application.
- For MSAL integration, the secrets should be configured in environment file as mentioned in the above section.
- A flag 'bypassAuth' is added in environment.ts which allows the user to disable authentication while running in local environment.

**Application Features:**
- After successful login, user will be navigated to home screen 
- Home screen will have *Recent activity widget*, showing list of study documents updated in last 30 days.
- On click of *Study Definitions -> Search*, user will be navigated to *Search page* to search specific study documents based on certain study parameters.
- On click of any Study document, user will be navigated to *Study details page*.
- From Study details page, user  can click  "View Revision History" to view the complete audit trail data for the study document.
- In Study details page, "View SOA Matrix" will be visible for the studies whose usdmVersion is V1.9 or above.
- From Study details page, user  can click  "View SOA Matrix" to view the Schedule of Activities for each Timeline under study designs for a study document.
  - In SoA Matrix, user can export the table to excel which includes the information of particular timeline including schedule timeline table, SoA table having activities data with accordian component inside each activity that has procedures, biomedical concepts and timeline profile data, and additional information using footnotes table also included.
- In *Audit trail page*, user  can select any two versions and click on "Version Comparison" to compare the changes.
- On click of *Study Definitions -> Compare*, user will be navigated to *Compare page* to select two study documents based on certain study parameters and to compare the changes.
- User will be logged out from application on click of logout link in the header.

**Admin Role Application Features:**
- On click of *Reports -> System Usage*, user will be navigated to *Usage Report* to view lists of all the API calls made to the SDR application for a given duration.
  - In System usage report, user can export the report to csv format with a maximum downloaded records limit of 1500 records. This value is configurable.
- On click of *Manage -> Group*, user will be navigated to *Group Management* to view lists of all groups created.
- On click of Add Group button from group management screen, user will be navigated to *Add Group* to create or edit new groups.
- On click of *Manage -> User*, user will be navigated to *User Management* to view lists of all User association with groups created.
- On click of Add User button from user management screen, user will be navigated to *Add User* to create or edit User association with groups.


**URL List**

- Default page (URL: /# )
  - Has login link
  - Uses MSAL oauth authentication
  - Has auth token generation link for accessing SDR APIs
- Home page (URL: /home )
  - Dashboard page with recent activity widget and Menu bar to navigate to search
- Search page (URL: /search )
- Study Details page(URL: /#/details;studyId="";versionId="";usdmVersion="")
- Revision History page (URL: /#/details/audit;studyId="")
- SoA Matrix page (URL: /#/details/soa;studyId="";versionId="";usdmVersion="")
- Version Comparison page(URL: /#/details/audit/compare;studyId="";verA="";verB="";usdmVerA="";usdmVerB="")
- Study Compare page(URL: /comparison)
- System Usage Report(URL: /reports)
- User Management page(URL: /admin/userMap)
- Group Management page(URL: /admin)
