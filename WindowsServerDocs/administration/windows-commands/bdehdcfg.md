---
title: bdehdcfg
description: "Windows Commands topic for **bdehdcfg** - Prepares a hard drive with the partitions necessary for BitLocker Drive Encryption."
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4c92cd74-188e-4fec-b7c4-fe4e8903e032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
---
# bdehdcfg

>Applies To: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Prepares a hard drive with the partitions necessary for BitLocker Drive Encryption. Most installations of  Windows 7  will not need to use this tool because BitLocker setup includes the ability to prepare and repartition drives as required.
> [!WARNING]
> There is a known conflict with the **Deny write access to fixed drives not protected by BitLocker** Group Policy setting located in **computer Configuration\Administrative Templates\Windows components\BitLocker Drive Encryption\Fixed Data Drives**.
> 
> if bdehdcfg is run on a computer when this policy setting is enabled, you may encounter the following issues:
> 
> -   if you attempted to shrink the drive and create the system drive, the drive size will be successfully reduced and a raw partition will be created. However, the raw partition will not be formatted. The following error message is displayed: "The new active Drive cannot be formatted. You may need to manually prepare your drive for BitLocker."
> -   if you attempted to use unallocated space to create the system drive, a raw partition will be created. However, the raw partition will not be formatted. The following error message is displayed: "The new active Drive cannot be formatted. You may need to manually prepare your drive for BitLocker."
> -   if you attempted to merge an existing drive into the system drive, the tool will fail to copy the required boot file onto the target drive to create the system drive. The following error message is displayed: "BitLocker setup failed to copy boot files. You may need to manually prepare your drive for BitLocker."
> 
> if this policy setting is being enforced, a hard drive cannot be repartitioned because the drive is protected. If you are upgrading computers in your organization from a previous version of Windows and those computers were configured with a single partition, you should create the required BitLocker system partition before applying the policy setting to the computers.
for examples of how this command can be used, see [Examples](#BKMK_Examples).
## Syntax
```
bdehdcfg [ driveinfo <DriveLetter>] [-target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}] [ newdriveletter] [ size <SizeinMB>] [-quiet]
```
### Parameters
|Parameter|Description|
|-------|--------|
|[bdehdcfg: driveinfo](bdehdcfg-driveinfo.md)|Displays the drive letter, the total size, the maximum free space, and the partition characteristics of the partitions on the drive specified. Only valid partitions are listed. Unallocated space is not listed if four primary or extended partitions already exist.|
|[bdehdcfg: target](bdehdcfg-target.md)|Defines which portion of a drive to use as the system drive and makes the portion active.|
|[bdehdcfg: newdriveletter](bdehdcfg-newdriveletter.md)|assigns a new drive letter to the portion of a drive used as the system drive.|
|[bdehdcfg: size](bdehdcfg-size.md)|Determines the size of the system partition when a new system drive is being created.|
|[bdehdcfg: quiet](bdehdcfg-quiet.md)|Prevents the display of all actions and errors in the command-line interface and directs bdehdcfg to use the "Yes" answer to any Yes/No prompts that may occur during subsequent drive preparation.|
|[bdehdcfg: restart](bdehdcfg-restart.md)|directs the computer to restart after the drive preparation has finished.|
|/?|Displays help at the command prompt.|
## <a name="BKMK_Examples"></a>Examples
The following example depicts bdehdcfg being used with the default drive to create a system partition of 500 MB. Because no drive letter is specified, the new system partition will not have a drive letter.
```
bdehdcfg -target default -size 500
```
The following example depicts bdehdcfg being used with the default drive to create a system partition (P:) of the default size of 300 MB out of unallocated space on the drive. The tool will not prompt the user for any further input nor will any errors be displayed. After the system drive has been created, the computer will automatically restart.
```
bdehdcfg -target unallocated  newdriveletter P: -quiet -restart
```
## additional references
-   [Command-Line Syntax Key](command-line-syntax-key.md)
