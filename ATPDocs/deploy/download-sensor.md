---
title: Download the sensor | Microsoft Defender for Identity
description: Learn how to download the Microsoft Defender for Identity sensors for your domain controllers.
ms.date: 06/13/2023
ms.topic: how-to
---

# Download the Microsoft Defender for Identity sensor

This article describes how to download the Microsoft Defender for Identity sensor for your domain controllers.

## Add and download a sensor

1. In [Microsoft 365 Defender](https://security.microsoft.com), go to **Settings** and then **Identities**. For example:

    ![Screenshot of the Settings page.](../media/settings-identities.png)

1. Select the **Sensors** tab, which displays all of your Defender for Identity sensors. For example:

    [![Screenshot of the Sensors tab.](../media//sensor-page.png)](../media/sensor-page.png#lightbox)

1. Select **Add sensor**. Then, in the **Add a new sensor** pane, select **Download installer** and save the installation package locally. The downloaded zip file includes the following files:

    - The Defender for Identity sensor installer

    - The configuration setting file with the required information to connect to the Defender for Identity cloud service

    Make sure that no pop-up blocker is blocking the download. <!-- in the installer is also NPCAP OEM version 1.0 Correct. But we could remove it as it is bundled within the setup package. Do you know if we still have customers using it separately?-->

1. In the **Add a new sensor** pane, copy the **Access key** value and save it to a secured location. This access key is a one-time password for use when deploying the sensor, after which communication is performed using certificates for authentication and TLS encryption.

    > [!TIP]
    > Use the **Regenerate key** button if you ever need to regenerate the new access key. It won't affect any previously deployed sensors, because it's only used for initial registration of the sensor.

1. Copy the downloaded installation package to the dedicated server or domain controller where you're installing the Defender for Identity sensor.

## Next step

> [!div class="step-by-step"]
> [Configure proxy settings »](configure-proxy.md)
