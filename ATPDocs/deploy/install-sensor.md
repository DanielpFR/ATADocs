---
title: Install the sensor | Microsoft Defender for Identity
description: Learn how to install the Microsoft Defender for Identity sensors on your domain controllers.
ms.date: 06/21/2023
ms.topic: how-to
---

# Install the Microsoft Defender for Identity sensor

Learn how to install the Microsoft Defender for Identity sensor on domain controllers.

> [!NOTE]
> When installing the sensor on Windows Server Core, or to deploy the sensor via a software deployment system, follow the steps for [silent installation](#defender-for-identity-sensor-silent-installation) instead.

## Prerequisites

Before you start, make sure that you have:

- A downloaded copy of your [Defender for Identity sensor setup package](download-sensor.md) and the access key.

- Microsoft .NET Framework 4.7 or later is installed on the machine. If Microsoft .NET Framework 4.7 or later isn't installed, the Defender for Identity sensor setup package installs it, which may require a reboot of the server.

- Relevant server specifications and network requirements, if you're installing the sensor on an Active Directory Federation Service (AD FS) server. For more information, see [Microsoft Defender for Identity on Active Directory Federation Services (AD FS)](active-directory-federation-services.md).

## Install the sensor

Perform the following steps on the domain controller or AD FS server.

1. Verify the machine has connectivity to the relevant [Defender for Identity cloud service endpoint(s)](configure-proxy.md#enable-access-to-defender-for-identity-service-urls-in-the-proxy-server).

1. Extract the installation files from the zip file. Installing directly from the zip file will fail.

1. Run **Azure ATP sensor setup.exe** with elevated privileges (**Run as administrator**) and follow the setup wizard.

1. On the **Welcome** page, select your language and select **Next**. For example:

    ![Screenshot of the Defender for Identity standalone sensor installation language selection.](../media/sensor-install-language.png)

    The installation wizard automatically checks if the server is a domain controller/ AD FS server or a dedicated server.

    - If it's a domain controller / AD FS server, the Defender for Identity sensor is installed.
    - If it's a dedicated server, the Defender for Identity standalone sensor is installed.

    For example, for a Defender for Identity sensor, the following screen is displayed to let you know that a Defender for Identity sensor is installed on your dedicated server:

    ![Screenshot of the Defender for Identity sensor installation.](../media/sensor-install-deployment-type.png)

1. Select **Next**.

    A warning is issued if the domain controller / AD FS server or dedicated server does not meet the minimum hardware requirements for the installation. The warning doesn't prevent you from selecting **Next**, and proceeding with the installation. It can still be the right option for the installation of Defender for Identity in a small lab test environment where less room for data storage is required. 

    For production environments, we highly recommend working with Defender for Identity's [capacity planning](capacity-planning.md) guide to make sure your domain controllers or dedicated servers meet the necessary requirements.

1. Under **Configure the sensor**, enter the installation path and the setup package access key. For example:

    ![Screenshot of the Defender for Identity sensor configuration screen.](../media/sensor-install-config.png)

    Enter the following details:

    - **Installation path**: The location where the Defender for Identity sensor is installed. By default the path is  `%programfiles%\Azure Advanced Threat Protection sensor`. Leave the default value.
    - **Access key**: Retrieved from the Microsoft 365 Defender portal in a [previous step](download-sensor.md).


1. Select **Install**. The following components are installed and configured during the installation of the Defender for Identity sensor:

    - **Defender for Identity sensor service** and **Defender for Identity sensor updater service**

    - **Microsoft Visual C++ 2013 Redistributable**

    - **KB 3047154** (for Windows Server 2012 R2 only)

        > [!IMPORTANT]
        >
        > - Don't install KB 3047154 on a virtualization host, which is the host that is running the virtualization. This may cause port mirroring to stop working properly. It's fine to run KB 3047154 on a virtual machine.
        > - If Wireshark is installed on the Defender for Identity sensor machine, you'll need to restart the Defender for Identity sensor after running Wireshark because they use the same drivers.

### Viewing sensor versions

Beginning with version 2.176, when installing the sensor from a new package, the sensor's version under **Add/Remove Programs** will appear with the full version number, such as **2.176.x.y**, instead of the static **2.0.0.0** that was previously shown.

The installed version continues to show even after automatic updates are run from the Defender for Identity cloud services. 

View the sensor's real version in the Microsoft 365 Defender [sensor settings page](https://security.microsoft.com/settings/identities?tabid=sensor), in the executable path or in the file version.

## Defender for Identity sensor silent installation

The Defender for Identity silent installation is configured to automatically restart the server at the end of the installation if necessary. 

Make sure to schedule a silent installation only during a maintenance window. Because of a Windows Installer bug, the *norestart* flag cannot be reliably used to make sure the server does not restart.

To track your deployment progress, monitor the Defender for Identity installer logs, which are located in `%AppData%\Local\Temp`.

### Silent installation via a deployment system

When silently deploying the Defender for Identity sensor via System Center Configuration Manager or other software deployment system, we recommend creating two deployment packages:

- Net Framework 4.7 or later, which may include rebooting the domain controller
- The Defender for Identity sensor

Make the Defender for Identity sensor package dependent on the deployment of the .Net Framework package deployment. If needed, get the [.Net Framework 4.7 offline deployment package](https://support.microsoft.com/topic/the-net-framework-4-7-offline-installer-for-windows-f32bcb33-5f94-57ce-6120-62c9526a91f2).

### Run a silent installation

Use the following commands to perform a fully silent install of the Defender for Identity sensor, using the access key copied in a [previous step](download-sensor.md). 

**cmd.exe syntax**:

```cmd
"Azure ATP sensor Setup.exe" /quiet NetFrameworkCommandLineArguments="/q" AccessKey="<Access Key>"
```

**Powershell syntax**:

```powershell
.\"Azure ATP sensor Setup.exe" /quiet NetFrameworkCommandLineArguments="/q" AccessKey="<Access Key>"
```

> [!NOTE]
> When using the Powershell syntax, omitting the `.\` preface results in an error that prevents silent installation.

**Installation options**:

|Name|Syntax|Mandatory for silent installation?|Description|
|-------------|----------|---------|---------|
|`Quiet`|`/quiet`|Yes|Runs the installer displaying no UI and no prompts.|
|`Help`|`/help`|No|Provides help and quick reference. Displays the correct use of the setup command including a list of all options and behaviors.|
|`NetFrameworkCommandLineArguments="/q"`|`NetFrameworkCommandLineArguments="/q"`|Yes|Specifies the parameters for the .Net Framework installation. Must be set to enforce the silent installation of .Net Framework.|

**Installation parameters**:

|Name|Syntax|Mandatory for silent installation?|Description|
|-------------|----------|---------|---------|
|`InstallationPath`|`InstallationPath=""`|No|Sets the path for the installation of Defender for Identity Sensor binaries. Default path: `%programfiles%\Azure Advanced Threat Protection Sensor` |
|`AccessKey`|`AccessKey="\*\*"`|Yes|Sets the access key that is used to register the Defender for Identity sensor with the Defender for Identity instance.|
|`AccessKeyFile`|`AccessKeyFile=""`|No|Sets the workspace Access Key from the provided text file path.|
|`DelayedUpdate`|`DelayedUpdate=true`|No|Sets the sensor's update mechanism to delay the update for 72 hours from the official release of each service update. For more information, see [Delayed sensor update](../sensor-settings.md#delayed-sensor-update).|
|`LogsPath`|`LogsPath=""`|No|Sets the path for the Defender for Identity Sensor logs. Default path: `%programfiles%\Azure Advanced Threat Protection Sensor`|

**Examples**:

Use the following commands to silently install the Defender for Identity sensor:

```cmd
"Azure ATP sensor Setup.exe" /quiet NetFrameworkCommandLineArguments="/q" AccessKey="<access key value>"
```

```cmd
"Azure ATP sensor Setup.exe" /quiet NetFrameworkCommandLineArguments="/q" AccessKeyFile="C:\Path\myAccessKeyFile.txt"
```

## Installing on AD FS servers

If you installed the sensor on AD FS servers, follow the steps in [Post-installation steps for AD FS servers](active-directory-federation-services.md#post-installation-steps-for-ad-fs-servers) to complete the setup.

These steps are required, or the sensor services will not start.

## Next step

> [!div class="step-by-step"]
> [Manage action accounts »](manage-action-accounts.md)
