Restart a service from an Azure Alert using Log Analytics and Azure Automation
==============================================================================

            

 


This sample automation runbook is designed to take the payload from an Azure Alert based on a Log Analytics
query for stopped services. It leverages the change tracking capabilities in Azure Automation to identify services that

are stopped in your environment.
 
This runbook parses out the comptuter name and service name so that it could be extended to actually start the service
on the machine using a hybrid worker, collect more informaiton from the machine on what might have caused the issue, or
escalte the alert to the right system for triage.

The query used from Log Analytics Azure Alert is: 

ConfigurationChange
| where ( ConfigChangeType == 'WindowsServices' )
| where ( SvcChangeType == 'State' )
| where ( SvcState == 'Stopped' )

| where ( SvcDisplayName == 'Print Spooler')


The runbook to start on the hybrid worker is called Restart-ServiceOnHybridWorker with the following code.

Param(
$ServiceName
)
 
Start-Service -Name $ServiceName
Get-Service -Name $ServiceName

 

        
    
TechNet gallery is retiring! This script was migrated from TechNet script center to GitHub by Microsoft Azure Automation product group. All the Script Center fields like Rating, RatingCount and DownloadCount have been carried over to Github as-is for the migrated scripts only. Note : The Script Center fields will not be applicable for the new repositories created in Github & hence those fields will not show up for new Github repositories.
