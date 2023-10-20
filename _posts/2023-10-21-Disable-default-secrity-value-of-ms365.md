---
layout: mypost
title: 禁用 Microsoft 365 的安全默认值
categories: [Microsoft 365]
---

Microsoft 365 的安全默认值原来在 Azure Active Directory 中管理，现在 Azure Active Directory 改名为 Microsoft Entra ID，并且过一段时间会强制启用安全默认值。
对于使用受信任 IP 地址登录的用户来说，启用安全默认值后每次 SSO 登录都要 MFA，过于繁琐。需要关闭安全默认值，根据策略和不同用户属性启用安全默认值。

[!image](MFA_disable.png)

[!image](SecurityDefault_disable.png)
