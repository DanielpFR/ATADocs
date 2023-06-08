---
title: Remove local admins on identity assets | Microsoft Defender for Identity
description: Learn about Microsoft Defender for Identity's `Remove local admins on identity assets` security assessment in Microsoft Secure Score.
ms.date: 06/08/2023
ms.topic: how-to
---

# Security assessment: Remove local admins on identity assets


## Why are local admins on identity assets a risk?

Accounts with indirect control over an identity system, such as AD FS, Active Directory, and so on, have the rights to escalate their privileges within the environment, which can lead to obtaining Domain Admin access or equivalent. 

Every local admin on a Tier-0 system is an indirect Domain Admin from an attacker's point of view.

## How do I use this security assessment to improve my organizational security posture?

1. Review the recommended action at <https://security.microsoft.com/securescore?viewid=actions> for **Remove local admins on identity assets**.

    For example:

    :::image type="content" source="media/secure-score/local-admins.png" alt-text="Screenshot of the Remove local admins on identity assets security assessment.":::

1. Review this list of exposed entities to discover which of your accounts have local admin rights on your identity assets.

1. Take appropriate action on those entities by removing their privileged access rights.

To achieve a full score, you must remediate all exposed entities.

> [!NOTE]
> This assessment is updated in near real time.
> The reports show the affected entities from the last 30 days. After that time, entities no longer affected will be removed from the exposed entities list.

## Next steps

- [Learn more about Microsoft Secure Score](/microsoft-365/security/defender/microsoft-secure-score)
- [Check out the Defender for Identity forum!](<https://aka.ms/MDIcommunity>)
