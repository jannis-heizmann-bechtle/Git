# Migrate local VM to Azure

### Prerequisites

* Use Windows Server VM on local Hypervisor
* Install Hyper-V (nested) on that windows server
  * Install or import the target VM on that Hyper-V (the nested one)

_ACHTUNG:_ Beim Anlegen des Projects die richtige Location wählen!!!

### Discover

* Verify user account permissions
* Setup the Azure Migrate project
* Install the Azure Migrate: Discovery and assessment agent on the nested Hyper-V
* Use the local browser to configure Discovery
* Start discovery and see Agent and discovered servers in Azure Migrate project

!\[\[Pasted image 20230830160022.png]]

https://learn.microsoft.com/en-us/azure/migrate/tutorial-discover-hyper-v

### Assessment (Optional)

\-> Create an assessment on the same page to check if VMs are Azure ready

### Prepare the Migration

* In Azure Migrate create the Migration and modernization config
* Download the provider EXE with keyvault credential file and copy to nested Hyper-V
* Install the Provider and select the credential file
* Wait for the provider to show the discovered VMs in Azure

!\[\[Pasted image 20230830160330.png]]

https://learn.microsoft.com/en-us/azure/migrate/tutorial-migrate-hyper-v?tabs=UI

**Unregister Server from Azure Site Recovery**

Execute this script on the Hyper-V to remove it from the existing Azure Site Recovery:

```powershell
 pushd .
 try
 {
      $windowsIdentity=[System.Security.Principal.WindowsIdentity]::GetCurrent()
      $principal=new-object System.Security.Principal.WindowsPrincipal($windowsIdentity)
      $administrators=[System.Security.Principal.WindowsBuiltInRole]::Administrator
      $isAdmin=$principal.IsInRole($administrators)
      if (!$isAdmin)
      {
         "Please run the script as an administrator in elevated mode."
         $choice = Read-Host
         return;       
      }

     $error.Clear()    
 	"This script will remove the old Azure Site Recovery Provider related properties. Do you want to continue (Y/N) ?"
 	$choice =  Read-Host
 
 	if (!($choice -eq 'Y' -or $choice -eq 'y'))
 	{
 	"Stopping cleanup."
 	return;
 	}
 
 	$serviceName = "dra"
 	$service = Get-Service -Name $serviceName
 	if ($service.Status -eq "Running")
     {
 		"Stopping the Azure Site Recovery service..."
 		net stop $serviceName
 	}
 
     $asrHivePath = "HKLM:\SOFTWARE\Microsoft\Azure Site Recovery"
 	$registrationPath = $asrHivePath + '\Registration'
 	$proxySettingsPath = $asrHivePath + '\ProxySettings'
     $draIdvalue = 'DraID'
 
     if (Test-Path $asrHivePath)
     {
         if (Test-Path $registrationPath)
         {
             "Removing registration related registry keys."	
 	        Remove-Item -Recurse -Path $registrationPath
         }

         if (Test-Path $proxySettingsPath)
     {
 	        "Removing proxy settings"
 	        Remove-Item -Recurse -Path $proxySettingsPath
         }

         $regNode = Get-ItemProperty -Path $asrHivePath
         if($regNode.DraID -ne $null)
         {            
             "Removing DraId"
             Remove-ItemProperty -Path $asrHivePath -Name $draIdValue
         }
 	    "Registry keys removed."
     }

     # First retrive all the certificates to be deleted
 	$ASRcerts = Get-ChildItem -Path cert:\localmachine\my | where-object {$_.friendlyname.startswith('ASR_SRSAUTH_CERT_KEY_CONTAINER') -or $_.friendlyname.startswith('ASR_HYPER_V_HOST_CERT_KEY_CONTAINER')}
 	# Open a cert store object
 	$store = New-Object System.Security.Cryptography.X509Certificates.X509Store("My","LocalMachine")
 	$store.Open('ReadWrite')
 	# Delete the certs
 	"Removing all related certificates"
     foreach ($cert in $ASRcerts)
     {
 	    $store.Remove($cert)
     }
 }catch
 {	
     [system.exception]
     Write-Host "Error occured" -ForegroundColor "Red"
     $error[0] 
     Write-Host "FAILED" -ForegroundColor "Red"
 }
 popd
```

More information: [azure-content/articles/site-recovery/site-recovery-manage-registration-and-protection.md at master · Huachao/azure-content · GitHub](https://github.com/Huachao/azure-content/blob/master/articles/site-recovery/site-recovery-manage-registration-and-protection.md)

### Migration

#### Prerequisites

* Check the correct location for all affected objects
* Check that the Migration tool detected the target VM

!\[\[Pasted image 20230830165959.png]]

#### Migrate

* Ensure the VM is turned off
* In Azure Migrate project create the replication
* Define azure VM parameters
* Wait for the replication to finish
* Start test migration
* Start productive migration
* Verify server funcionalitiy
