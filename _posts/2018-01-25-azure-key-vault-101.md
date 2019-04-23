---
layout: post
toc: true
toc_label: "Table of content"
toc_sticky: true
show-avatar: true
comments: true
social-share: true
title: Azure Key Vault 101
date: 2018-01-25 00:00:00 +0000
tags: []
excerpt_separator: ''
share-img: ''
---
# Using Azure Key Vault with PowerShell – Part 1

[Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/) is used to safeguard and manage **cryptographic keys**, **certificates** and **secrets** used by cloud applications and services (you can still consume these on-premise though). This allows other application, services or users in an Azure subscription to store and retrieve these **cryptographic keys**, **certificates** and **secrets**.

Once **cryptographic keys**, **certificates** and **secrets** have been stored in a **Azure Key Vault** access policies can be configured to provide access to them by other users or applications.

**Azure Key Vault** also stores all past versions of a **cryptographic key**, **certificate** or **secret** when they are updated. So this allows easily rolling back if anything breaks.

This post is going to show how:

1. Set up an Azure Key Vault using the [PowerShell Azure Module](https://docs.microsoft.com/en-us/powershell/azure/overview).
2. Set **administration access policies** on the **Azure Key Vault**.
3. Grant other _users_ or _applications_ **access** to **cryptographic keys**, **certificates** or **secrets**.
4. **Add**, **retrieve** and **remove** a **cryptographic key** from the **Azure Key Vault**.
5. **Add**, **retrieve** and **remove** a **secret** from the **Azure Key Vault**.

# Requirements

Before getting started there is a few things that will be needed:

1. An **Azure account**. I’m sure you’ve already got one, but if not [create a free one here](https://azure.microsoft.com/en-us/free/).
2. The **Azure PowerShell module** needs to be installed. [Click here](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps) for instructions on how install it.

# Install the Key Vault

The first task is to customize and install the Azure Key Vault using the following **PowerShell script**.

```powershell

# The name of the Azure subscription to install the Key Vault into
$subscriptionName = 'MySubscription'

# The resource group that will contain the Key Vault to create to contain the Key Vault
$resourceGroupName = 'MyKeyVaultRG'

# The name of the Key Vault to install
$keyVaultName = 'MyKeyVault'

# The Azure data center to install the Key Vault to
$location = 'southcentralus'

# These are the Azure AD users that will have admin permissions to the Key Vault
$keyVaultAdminUsers = @('Joe Boggs','Jenny Biggs')

# Login to Azure
Login-AzureRMAccount

# Select the appropriate subscription
Select-AzureRmSubscription -SubscriptionName $subscriptionName

# Make the Key Vault provider is available
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault

# Create the Resource Group
New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

# Create the Key Vault (enabling it for Disk Encryption, Deployment and Template Deployment)
New-AzureRmKeyVault -VaultName $keyVaultName -ResourceGroupName $resourceGroupName -Location $location `
    -EnabledForDiskEncryption -EnabledForDeployment -EnabledForTemplateDeployment

# Add the Administrator policies to the Key Vault
foreach ($keyVaultAdminUser in $keyVaultAdminUsers) {
    $UserObjectId = (Get-AzureRmADUser -SearchString $keyVaultAdminUser).Id
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ResourceGroupName $resourceGroupName -ObjectId $UserObjectId `
        -PermissionsToKeys all -PermissionsToSecrets all -PermissionsToCertificates all
}
```

But first, the variables in the **PowerShell script** need to be customized to suit. The variables in the **PowerShell script** that needs to be set are:

* **$subscriptionName** – the name of the Azure subscription to install the Key Vault into.
* **$resourceGroupName –** the name of the Resource Group to create to contain the Key Vault.
* **$keyVaultName** – the name of the Key Vault to create.
* **$location** – the Azure data center to install the Key Vault to (use **Get-AzureRMLocation** to get a list of available Azure data centers).****
* **$keyVaultAdminUsers** – an array of users that will be given administrator (full control over **cryptographic keys**, **certificates** and **secrets**). The user names specified must match the full name of users found in the **Azure AD** assigned to the Azure tenancy.

![ss_akv_create](https://dscottraynsford.files.wordpress.com/2017/04/ss_akv_create.gif?w=646 =646x345)

It will take about 30 seconds for the Azure Key Vault to be installed. It will then show up in the Azure Subscription:

![ss_akv_createcompleteportal](https://dscottraynsford.files.wordpress.com/2017/04/ss_akv_createcompleteportal.png?w=646)

# Assigning Permissions

Once the **Azure Key Vault** is setup and an administrator or two have been assigned, other access policies will usually need to be assigned to **users**and/or [application or service principal](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-application-objects).

To create an **access policy** to allow a **user** to _get_ and _list_ **cryptographic keys**, **certificates** and **secrets** if you know the **User Principal Name**:

| --- | --- |
|  | Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ResourceGroupName $resourceGroupName \` |
|  |   -UserPrincipalName 'Joe.Boggs@contoso.com' \` |
|  |   -PermissionsToCertificates list,get \` |
|  |   -PermissionsToKeys list,get \` |
|  |   -PermissionsToSecrets list,get |

[**view raw**](https://gist.github.com/PlagueHO/c430d4f9eafbe4c6d753613365c6b6aa/raw/fa22a5c2f5d93a3c4121600906e9853ebc6d81ed/Set-AzureRmKeyVaultAccessPolicyForUser.ps1)[**Set-AzureRmKeyVaultAccessPolicyForUser.ps1**](https://gist.github.com/PlagueHO/c430d4f9eafbe4c6d753613365c6b6aa#file-set-azurermkeyvaultaccesspolicyforuser-ps1) hosted with ![❤](https://s0.wp.com/wp-content/mu-plugins/wpcom-smileys/twemoji/2/svg/2764.svg) by [**GitHub**](https://github.com/)

_Note: the above code assumes you still have the variables set from the ‘Install the Key Vault’ section._

If you only have the **full name** of the **user** then you’ll need to look up the **Object Id** for the user in the **Azure AD**:

| --- | --- |
|  | $userObjectId = (Get-AzureRmADUser -SearchString 'Joe Bloggs').Id |
|  | Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ResourceGroupName $resourceGroupName \` |
|  |   -ObjectId $userObjectId \` |
|  |   -PermissionsToCertificates list,get \` |
|  |   -PermissionsToKeys list,get \` |
|  |   -PermissionsToSecrets list,get |

[**view raw**](https://gist.github.com/PlagueHO/5900d92a9b97e947bc60ff7fe721fefe/raw/37857f6338651e7b5815e7693a6f6d28723f3527/Set-AzureRmKeyVaultAccessPolicyForUserByObject.ps1)[**Set-AzureRmKeyVaultAccessPolicyForUserByObject.ps1**](https://gist.github.com/PlagueHO/5900d92a9b97e947bc60ff7fe721fefe#file-set-azurermkeyvaultaccesspolicyforuserbyobject-ps1) hosted with ![❤](https://s0.wp.com/wp-content/mu-plugins/wpcom-smileys/twemoji/2/svg/2764.svg) by [**GitHub**](https://github.com/)

_Note: the above code assumes you still have the variables set from the ‘Install the Key Vault’ section._

To create an **access policy** to allow a **service principal** or **application** to _get_ and _list_ **cryptographic keys** if you know the **Application Id** (a GUID):

| --- | --- |
|  | Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ResourceGroupName $resourceGroupName \` |
|  |   -ServicePrincipalName 'e9b1bc3c-4769-4a98-9014-b315fd2adf53' \` |
|  |   -PermissionsToCertificates list,get \` |
|  |   -PermissionsToKeys list,get \` |
|  |   -PermissionsToSecrets list,get |

[**view raw**](https://gist.github.com/PlagueHO/e079674f79ed6cf87b877d6829735917/raw/bea0b4777c36f828a19917bfe02e3d61119141e0/%20Set-AzureRmKeyVaultAccessPolicyForService.ps1)[**Set-AzureRmKeyVaultAccessPolicyForService.ps1**](https://gist.github.com/PlagueHO/e079674f79ed6cf87b877d6829735917#file-set-azurermkeyvaultaccesspolicyforservice-ps1) hosted with ![❤](https://s0.wp.com/wp-content/mu-plugins/wpcom-smileys/twemoji/2/svg/2764.svg) by [**GitHub**](https://github.com/)

_Note: the above code assumes you still have the variables set from the ‘Install the Key Vault’ section._

Changing the values of the **PermissionsToKeys, PermissionsToCertificates**and **PermissionsToSecrets** parameters in the cmdlets above allow different permissions to be set for each policy.

The available permissions for **certificates**, **keys** and **secrets** are:

[![ss_akv_keypermissions](https://dscottraynsford.files.wordpress.com/2017/04/ss_akv_keypermissions.png?w=177&h=377 "ss_akv_keypermissions" =177x377)](https://dscottraynsford.wordpress.com/2017/04/17/using-azure-key-vault-with-powershell-part-1/ss_akv_keypermissions/)

[![ss_akv_secretpermissions](https://dscottraynsford.files.wordpress.com/2017/04/ss_akv_secretpermissions.png?w=461&h=377 "ss_akv_secretpermissions" =461x377)](https://dscottraynsford.wordpress.com/2017/04/17/using-azure-key-vault-with-powershell-part-1/ss_akv_secretpermissions/)

An **access policy** can be removed from **users** or **service principals** using the **Remove-AzureRmKeyVaultAccessPolicy** cmdet:

| --- | --- |
|  | Remove-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ResourceGroupName $resourceGroupName \` |
|  |   -UserPrincipalName 'Joe.Boggs@contoso.com' |

[**view raw**](https://gist.github.com/PlagueHO/058455310cdff17b8bdba6ba6db74f6c/raw/569350c76d49366bebedcfc90487fdb407107118/Remove-AzureRmKeyVaultAccessPolicyForUser.ps1)[**Remove-AzureRmKeyVaultAccessPolicyForUser.ps1**](https://gist.github.com/PlagueHO/058455310cdff17b8bdba6ba6db74f6c#file-remove-azurermkeyvaultaccesspolicyforuser-ps1) hosted with ![❤](https://s0.wp.com/wp-content/mu-plugins/wpcom-smileys/twemoji/2/svg/2764.svg) by [**GitHub**](https://github.com/)

_Note: the above code assumes you still have the variables set from the ‘Install the Key Vault’ section._

# Working with Secrets

**Secrets** can be _created_, _updated, retrieved_ and _deleted_ by users or applications that have been assigned with the **appropriate policy**.

## Creating/Updating Secrets

To create a new secret, use the **Set-AzureKeyVaultSecret** cmdlet:

| --- | --- |
|  | Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name 'MyAdminPassword' \` |
|  |   -SecretValue (ConvertTo-SecureString -String 'P@ssword!1' -AsPlainText -Force) |

[**view raw**](https://gist.github.com/PlagueHO/45d281e3f6dc58107fbb6f982c27b4e8/raw/0f707c08d70c812ff1cf735f139712288e8af4cd/Set-AzureKeyVaultSecretPassword.ps1)[**Set-AzureKeyVaultSecretPassword.ps1**](https://gist.github.com/PlagueHO/45d281e3f6dc58107fbb6f982c27b4e8#file-set-azurekeyvaultsecretpassword-ps1) hosted with ![❤](https://s0.wp.com/wp-content/mu-plugins/wpcom-smileys/twemoji/2/svg/2764.svg) by [**GitHub**](https://github.com/)

_Note: the above code assumes you still have the variables set from the ‘Install the Key Vault’ section._

This will create a secret called **MyAdminPassword** with the value **P@ssword!1** in the **Azure Key Vault**.

The secret can be updated to a new value using the same cmdlet:

| --- | --- |
|  | Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name 'MyAdminPassword' \` |
|  |   -SecretValue (ConvertTo-SecureString -String 'Sup3rS3cr3tP4ss!' -AsPlainText -Force) |

[**view raw**](https://gist.github.com/PlagueHO/fe8da3ca6a0ac7e543e2f9c1de1339f9/raw/2bc1a6d25667f6db66a24868e5fd08de9fd742a0/Update-AzureKeyVaultSecretPassword.ps1)[**Update-AzureKeyVaultSecretPassword.ps1**](https://gist.github.com/PlagueHO/fe8da3ca6a0ac7e543e2f9c1de1339f9#file-update-azurekeyvaultsecretpassword-ps1) hosted with ![❤](https://s0.wp.com/wp-content/mu-plugins/wpcom-smileys/twemoji/2/svg/2764.svg) by [**GitHub**](https://github.com/)

**Additional parameters** can also be assigned to each version of a secret to control how it can be used:

* **ContentType** – the type of content the secret contains (e.g. ‘txt’)
* **NotBefore** – the date that the secret is valid after.
* **Expires** – the date the secret is valid until.
* **Disable** – marks the secret as disabled.
* **Tag** – assigns tags to the secret.

For example:

| --- | --- |
|  | Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name 'MyAdminPassword' \` |
|  |   -SecretValue (ConvertTo-SecureString -String 'Sup3rS3cr3tP4ss!' -AsPlainText -Force) \` |
|  |   -ContentType 'txt' \` |
|  |   -NotBefore ((Get-Date).ToUniversalTime()) \` |
|  |   -Expires ((Get-Date).AddYears(2).ToUniversalTime()) \` |
|  |   -Disable:$false \` |
|  |   -Tags @{ 'Risk' = 'High'; } |

[**view raw**](https://gist.github.com/PlagueHO/20e433002d73a69e2b7ef9318d9b4b9b/raw/2dd94255e9c17df76a1cc0eb69cd9843a8bf587a/%20Update-AzureKeyVaultSecretPasswordwithParameters.ps1)[**Update-AzureKeyVaultSecretPasswordwithParameters.ps1**](https://gist.github.com/PlagueHO/20e433002d73a69e2b7ef9318d9b4b9b#file-update-azurekeyvaultsecretpasswordwithparameters-ps1) hosted with ![❤](https://s0.wp.com/wp-content/mu-plugins/wpcom-smileys/twemoji/2/svg/2764.svg) by [**GitHub**](https://github.com/)

![ss_akv_secretupdatewithparameters](https://dscottraynsford.files.wordpress.com/2017/04/ss_akv_secretupdatewithparameters.png?w=646)

## Retrieving Secrets

To retrieve the **latest (current) version** of a secret, use the **Get-AzureKeyVaultSecret** cmdlet:

| --- | --- |
|  | $secretText = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -Name 'MyAdminPassword').SecretValue |

[**view raw**](https://gist.github.com/PlagueHO/335c7a9420ccdf1c2c0c7559b5d01db9/raw/fb936385ef6e3434114ace464ab2d9b64d1245aa/Get-AzureKeyVaultSecretPassword.ps1)[**Get-AzureKeyVaultSecretPassword.ps1**](https://gist.github.com/PlagueHO/335c7a9420ccdf1c2c0c7559b5d01db9#file-get-azurekeyvaultsecretpassword-ps1) hosted with ![❤](https://s0.wp.com/wp-content/mu-plugins/wpcom-smileys/twemoji/2/svg/2764.svg) by [**GitHub**](https://github.com/)

This will assign the stored secret to the variable **$secretText** as a **SecureString**. This can then be passed to any other cmdlets that require a **SecureString**.

To list **all** the versions of a secret, add the **IncludeVersions** parameter:

| --- | --- |
|  | Get-AzureKeyVaultSecret -VaultName $keyVaultName -Name 'MyAdminPassword' -IncludeVersions |

[**view raw**](https://gist.github.com/PlagueHO/82cc3c622a43673c5b35e9eb5ed0710b/raw/25514af9717687e4bfd12940fc933831221f9d22/%20Get-AzureKeyVaultSecretPasswordAllVersions.ps1)[**Get-AzureKeyVaultSecretPasswordAllVersions.ps1**](https://gist.github.com/PlagueHO/82cc3c622a43673c5b35e9eb5ed0710b#file-get-azurekeyvaultsecretpasswordallversions-ps1) hosted with ![❤](https://s0.wp.com/wp-content/mu-plugins/wpcom-smileys/twemoji/2/svg/2764.svg) by [**GitHub**](https://github.com/)

![ss_akv_secretallhistory](https://dscottraynsford.files.wordpress.com/2017/04/ss_akv_secretallhistory.png?w=646)

To retrieve a **specific version** of a secret, use the **Get-AzureKeyVaultSecret**cmdlet with the **Version** parameter specified:

| --- | --- |
|  | $secretText = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -Name 'MyAdminPassword' -Version '02218af0521749b084bb08bd13184efb') |

[**view raw**](https://gist.github.com/PlagueHO/12b88cde2f6c67cefd2c1a17710b4936/raw/f711815e4bdb57e9f34097b462fbab7c46c940dd/Get-AzureKeyVaultSecretPasswordSpecificVersion.ps1)[**Get-AzureKeyVaultSecretPasswordSpecificVersion.ps1**](https://gist.github.com/PlagueHO/12b88cde2f6c67cefd2c1a17710b4936#file-get-azurekeyvaultsecretpasswordspecificversion-ps1) hosted with ![❤](https://s0.wp.com/wp-content/mu-plugins/wpcom-smileys/twemoji/2/svg/2764.svg) by [**GitHub**](https://github.com/)

## Removing Secrets

Finally, to remove a secret use the **Remove-AzureKeyVaultSecret** cmdlet:

| --- | --- |
|  | Remove-AzureKeyVaultSecret -VaultName $keyVaultName -Name 'MyAdminPassword' -Force |

[**view raw**](https://gist.github.com/PlagueHO/5892377f9952506199c1d3843a028cd8/raw/fc4b627bde823727b3cca263a9d1ab48a206ee08/Remove-AzureKeyVaultSecret.ps1)[**Remove-AzureKeyVaultSecret.ps1**](https://gist.github.com/PlagueHO/5892377f9952506199c1d3843a028cd8#file-remove-azurekeyvaultsecret-ps1) hosted with ![❤](https://s0.wp.com/wp-content/mu-plugins/wpcom-smileys/twemoji/2/svg/2764.svg) by [**GitHub**](https://github.com/)

That pretty much covers managing and using **secrets** in **Azure Key Vault**using **PowerShell.**
