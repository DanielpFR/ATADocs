---
title: Configure sensor settings  | Microsoft Defender for Identity
description: Learn how to configure Microsoft Defender for Identity sensor settings 
ms.date: 06/21/2023
ms.topic: how-to
---

# Configure Microsoft Defender for Identity sensor settings

In this article, you'll learn how to correctly configure Microsoft Defender for Identity sensor settings to start seeing data. You'll need to do additional configuration and integration to take advantage of Defender for Identity's full capabilities.

## View and configure sensor settings

After the Defender for Identity sensor is installed, do the following to view and configure Defender for Identity sensor settings:

1. In [Microsoft 365 Defender](https://security.microsoft.com), go to **Settings** > **Identities** **Sensors**. For example:

   [![Screenshot of the Sensors page.](../media/sensor-page.png)](../media/sensor-page.png#lightbox)

   The **Sensors** page displays all of your Defender for Identity sensors, listing the following details per sensor:

   - Sensor name
   - Sensor domain membership
   - Sensor version number
   - Whether updates should be [delayed](../sensor-settings.md#delayed-sensor-update).
   - Sensor service status
   - Sensor status
   - Sensor health status
   - The number of health issues
   - When the sensor was created. 

   For more information, see [Sensor details](../sensor-settings.md#sensor-details).

1. Select **Filters** to select the filters you want visible. For example:

   [![Screenshot of sensor filters.](../media/sensor-filters.png)](../media/sensor-filters.png#lightbox)

1. Use the dispalyed filters to determine which sensors to display. For example:

   :::image type="content" source="../media/filtered-sensor.png" alt-text="Screenshot of a filtered list of sensors." lightbox="../media/filtered-sensor.png":::

1. Select a sensor to show a details pane with more information about the sensor and its health status. For example:

   [![Screenshot of a sensor details pane.](../media/sensor-details.png)](../media/sensor-details.png#lightbox)

1. Select **Manage sensor** to show a pane where you can configure sensor details. For example:

   :::image type="content" source="../media/manage-sensor.png" alt-text="Screenshot of the Manage sensor option." lightbox="../media/manage-sensor.png":::

1. Configure the following sensor details:

   - **Description** (optional): Enter a description for the Defender for Identity sensor.

   - **Domain Controllers (FQDN)**: This is required for the Defender for Identity standalone and AD FS sensors, and cannot be modified for the Defender for Identity sensor.
   
      Enter the complete FQDN of your domain controller and select the plus sign to add it to the list. For example,  **DC1.domain1.test.local**.

      For any servers you define in the **Domain Controllers** list:

      - All domain controllers whose traffic is being monitored via port mirroring by the Defender for Identity standalone sensor must be listed in the **Domain Controllers** list. If a domain controller isn't listed in the **Domain Controllers** list, detection of suspicious activities might not function as expected.

      - At least one domain controller in the list should be a global catalog. This enables Defender for Identity to resolve computer and user objects in other domains in the forest.

   - **Capture Network adapters** (required):

          - For Defender for Identity sensors, all network adapters that are used for communication with other computers in your organization.
          - For Defender for Identity standalone sensor on a dedicated server, select the network adapters that are configured as the destination mirror port. These network adapters receive the mirrored domain controller traffic.

1. On the **Sensors** page, select **Export** to export a list of your sensors to a **.csv** file. For example:

   :::image type="content" source="../media/export-sensors.png" alt-text="Screenshot of exporting a list of sensors." lightbox="../media/export-sensors.png":::

## Validate installations


   >[!NOTE]
   > The version of Defender for Identity updates frequently, to check the latest version, in the Defender for Identity portal, go to **Configuration** and then **About**.


To validate that the Defender for Identity sensor has been successfully deployed:

1. Check that the **Azure Advanced Threat Protection sensor** service is running. After you save the Defender for Identity sensor settings, it might take a few seconds for the service to start.

1. If the service doesn't start, review the **Microsoft.Tri.sensor-Errors.log** file, located by default at `%programfiles%\Azure Advanced Threat Protection sensor\Version X\Logs`.

1. Verify Defender for Identity connectivity on any domain device using the following steps:

     1. Open a command prompt and enter `nslookup`

     1. Enter **server** and the FQDN or IP address of the domain controller where the Defender for Identity sensor is installed. For example: `server <contosodc.contoso>.azure`

     1. Enter `ls -d <contoso.azure>`

         - Make sure to replace contosodc.contoso.azure and contoso.azure with the FQDN of your Defender for Identity sensor and domain name respectively.
       1. Repeat the previous two steps for each sensor you wish to test.
       1. From the Defender for Identity console, open the entity profile for the computer you ran the connectivity test from.
   1. Sign in to the [Microsoft 365 Defender portal](https://security.microsoft.com/). In the top middle search box, type in the name of the user you used to perform the commands detailed above, select the name in the results to view the user's page and all its related activities and alerts.

   > [!NOTE]
   > If the domain controller you wish to test is your first deployed sensor, wait at least 15 minutes to allow the database backend to finish initial deployment of the necessary microservices before you attempt to verify the related logical activity for that domain controller.

To validate that the Defender for Identity sensor has been successfully deployed on an AD FS server, check the following:

1. Check that the service named **Azure Advanced Threat Protection sensor** is running. After you save the Defender for Identity sensor settings, it might take a few seconds for the service to start.
1. If the service doesn't start, review the "Microsoft.Tri.sensor-Errors.log" file located in the following default folder, "%programfiles%\Azure Advanced Threat Protection sensor\Version X\Logs".
   > [!NOTE]
   > The version of Defender for Identity updates frequently, to check the latest version, in the Defender for Identity portal, go to **Configuration** and then **About**.

1. Use AD FS to authenticate a user to any application.
1. Verify that the AD FS authentication was observed by Defender for Identity using the following steps:

    1. Sign in to the [Microsoft 365 Defender portal](https://security.microsoft.com/). From the navigation menu, select **Hunting** and then **Advanced Hunting**. In the **Query** pane, type in and run the following query:

        ```query
        IdentityLogonEvents | where Protocol contains 'Adfs'
        ```

    1. The results pane should include a list of events with a **LogonType** of **Logon with ADFS authentication**. You can select a specific row and see additional details in the **Inspect Record** left pane.

    :::image type="content" source="../media/adfs-logon-advanced-hunting.png" alt-text="See results of ADFS logon advanced hunting query." lightbox="../media/adfs-logon-advanced-hunting.png":::


>[!NOTE]
> The version of Defender for Identity updates frequently, to check the latest version, in the Defender for Identity portal, go to **Configuration** and then **About**.

## Related videos

- [Microsoft Defender for Identity settings](https://www.microsoft.com/videoplayer/embed/RWFVEX)

## Next steps

Now that you've configured the initial configuration steps, you can configure more settings. Go to any of the pages below for more information:

- [Updating your sensors](../sensor-settings.md#updating-your-sensors)
- [Set entity tags: sensitive, honeytoken, and Exchange server](../entity-tags.md)
- [Configure detection exclusions](../exclusions.md)
- [Configure notifications: health issues, alerts, and Syslog](../notifications.md)
