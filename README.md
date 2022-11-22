# Intune Speedrun

A collection of tools to streamline deployment of a complete Intune environment
  - Default AutoPilot profile
  - Compliance Policies
  - Device Configuration Profiles
  - Default Intune filtering groups

This toolset is based on the work of several individuals
  - Alex Fields: https://github.com/vanvfields/Microsoft-365
  - Ben Reader: https://github.com/tabs-not-spaces/Intune-App-Deploy and http://powers-hell.com
  - Various contributors: https://github.com/microsoftgraph/powershell-intune-samples

## Using the toolset

Everything is built on PowerShell scripts being called by VSCode tasks. PowerShell 7 with VSCode plus the PowerShell extension is the minimum required to get started. GitHub Pull Requests and/or GitHub Repositories extensions will make it easier to work from the latest code in the repo.

Once installed and the workspace is open, press **F1** and type **Run Task**, then select the task you want to run.

### Initialize Environment

This task will configure the development environment by installing necessary PowerShell modules (powershell-yaml, AzureADPreview, Microsoft.Graph.Intune, and MSAL.ps) and download the Win32 Content Prep Tool. Task only needs to be run once.

### Intune Speedrun

This task will call Setup-Intune.ps1 and will prompt for the Account ID. Authentication to the tenant will be done via the devicecode method due to limitations with MSAL authentication via credentials to the Graph API. On first run, you will be prompted to grant permission to the Microsoft.Graph.Intune Enterprise Application.

### Build

Packages contents of the application directory into a Win32 *.intunewin. For most "standard" apps, this will only need to be done if the deployment script updates or a unique application is required. Automate Agents will need to be packaged per client. See **APPINFO.md** in 2_app_deploy/ for further info.

### Publish

Uploads the application package to Intune as a win32 application deployment. 

### Build & Publish

Streamlines the Build & Publish tasks into one task.

### PublishMSI

Creates LOB app deployment by calling Add-LOBApp.ps1, which will prompt for the MSI filename, Publisher, and Description. **NOTE:** All MSIs should be saved in the 2_app_deploy/_MSI-LOB/MSIs directory.

## Current limitations

The Intune Speedrun tool is not presently able to do the following:
  - Ingest ADMX template data as XML doesn't natively work inside JSON
  - Configure Windows Store For Business
  - Set MDM Authority for tenants created prior to 1909
  - MacOS MAM/MDM policy options are present but not in use
  - Windows Update Rings configuration options are present but not in use
  - Administrative Template deployment
  - Assign licenses to groups
    - Will be added, SKUIDs are here - https://docs.microsoft.com/en-us/azure/active-directory/enterprise-users/licensing-service-plan-reference
    - Graph endpoint locations here - https://docs.microsoft.com/en-us/azure/active-directory/enterprise-users/licensing-ps-examples
