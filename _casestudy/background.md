---
title: "Background: How Cookie Tracking Works"
author_profile: true
layout: default
---

# Background: How Cookie Tracking Works (Draft)

This page will explain the technological foundation of cookie tracking, including:

## What Cookies Are
Cookies are small text files that a website stores on a user’s browser. Technically, they serve many legitimate purposes: maintaining login sessions, keeping track of cart items, and customizing site settings. But cookies also became the backbone of the online advertising economy, enabling companies to track users across different sites, build behavioral profiles, and infer interests or demographics.
As Mellet & Beauvisage describe in their work on digital market infrastructures, cookies are part of a larger ecosystem designed to extract, exchange, and monetize user data without requiring explicit user awareness (Mellet & Beauvisage, 2022).

 [Link](https://www.taylorfrancis.com/chapters/edit/10.4324/9781003130154-2/cookie-monsters-anatomy-digital-market-infrastructure-kevin-mellet-thomas-beauvisage)

Cookies are often invisible in the moment-to-moment experience of browsing, but they quietly power a vast tracking infrastructure. Once placed in a browser, they can persist for months or years unless manually deleted or blocked.

## First-Party vs. Third-Party Cookies
First-party cookies come directly from the website a user is visiting. They support functionality and personalization.
Third-party cookies, however, are placed by external entities such as advertisers, tracking networks, or data brokers.
Third-party cookies enable cross-site tracking: an advertising network like Google or Meta can follow a user across the web, stitching together browsing behavior into a detailed behavioral profile. Research shows that these profiles can infer highly sensitive attributes, political leaning, sexuality, mental health indicators, even when such data is never explicitly provided (Öğüt et al., 2024; Narayanan & Shmatikov, 2009).
 [Link1] (https://www.usenix.org/conference/usenixsecurity24/presentation/ogut)
 [Link2] (https://arxiv.org/abs/1305.2306)
Third-party cookies are the main subject of privacy regulation because they are both powerful and largely invisible to users.

## Why Cookie Banners Exist
The explosion of third-party tracking collided with modern privacy laws. Regulations like the EU General Data Protection Regulation (GDPR) and the California Consumer Privacy Act (CCPA) require websites to get meaningful, informed consent before collecting and sharing personal data.
The GDPR specifically mandates:
- clear explanation of what data is being collected
- the purpose of collection
- the ability to refuse non-essential cookies
- consent that is freely given and not coerced

Cookie banners emerged as the universal way to comply with these requirements.
However, research shows that most banners do not follow the spirit of the law. They present users with confusing choices, misleading interfaces, or design tricks that nudge toward acceptance (Degeling et al., 2019).
 [Link] (https://dl.acm.org/doi/abs/10.1145/3173574.3174108)

## Dark Patterns in Cookie Design
Dark patterns are manipulative interface designs that push users toward outcomes beneficial to companies, not users. In cookie banners, dark patterns typically appear as:

Bright, bold “Accept All” buttons vs. tiny “Manage Preferences” links
- Multiple confusing steps to decline non-essential cookies
- Pre-checked boxes (illegal under GDPR but still widespread)
- Language ambiguity (“we value your privacy…” followed by pressure to accept tracking)

Empirical studies show that these manipulations significantly increase acceptance rates. Bösch et al. (2016) show that interface friction can affect privacy decisions even among users who intend to opt out.

 [Link] (https://dl.acm.org/doi/abs/10.1145/2872427.2882991)

The dominance of dark patterns suggests that the “consent” users give is not genuine autonomous choice but rather a behavioral response engineered through UI manipulation.

More content coming soon.

[↩ Back to main page](https://sevvara.github.io/ethics_fall2025/casestudy/)
