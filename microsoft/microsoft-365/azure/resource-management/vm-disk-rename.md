# VM-Disk Rename

**Requirements:**

Make sure you have the latest version of PowerShellGet installed

`Install-Module -Name PowerShellGet -Force`

Install and update to the latest Az PowerShell module

`Install-Module -Name Az -AllowClobber -Force`

Connect to AzureAD:

`Connect-AzAccount`

\-> Mit Azure-Administrator anmelden!

**Script example:**

```powershell
$location = "WestEurope" $resourceGroup = "RG-BEFR-MobileIron" $originalOsDiskName = "VM_BeFr_micore_vhd-VM_BeFr_micore" 
$newOsDiskName = "VM-BeFr-micore" 
$virtualMachineName = "OSDiskRename" 

#Get a list of all the managed disks in a resource group. 
Get-AzDisk -ResourceGroupName $resourceGroup | Select Name 

#Get the source managed disk. 
$sourceDisk = Get-AzDisk -ResourceGroupName $resourceGroup -DiskName $originalOsDiskName

#Create the disk configuration. 
$diskConfig = New-AzDiskConfig -SourceResourceId $sourceDisk.Id -Location $sourceDisk.Location -CreateOption Copy -DiskSizeGB 127 -SkuName "Premium_LRS"

#Create the new disk.
New-AzDisk -Disk $diskConfig -DiskName $newOsDiskName -ResourceGroupName $resourceGroup

#Swap the OS Disk out for the renamed disk.
$virtualMachine = Get-AzVm -ResourceGroupName $resourceGroup -Name $virtualMachineName 
$newOsDisk = Get-AzDisk -ResourceGroupName $resourceGroup -Name $newOsDiskName 
Set-AzVMOSDisk -VM $virtualMachine -ManagedDiskId $newOsDisk.Id -Name $newOsDisk.Name 
Update-AzVM -ResourceGroupName $resourceGroup -VM $virtualMachine 

#Delete the original disk. 
Remove-AzDisk -ResourceGroupName $resourceGroup -DiskName $originalOsDiskName -Force
```



