---
title: "Background"
layout: single 

collection: casestudy
permalink: /casestudy/background/
author_profile: true
layout: default
#nav_order: 0
---

## What is a Cookie?

At its simplest, a cookie is just a small text file stored on your computer by your browser. It was invented in 1994 by Lou Montulli at Netscape to solve a specific problem: **memory**.

Without cookies, the web is "stateless." This means a website has no idea that the person visiting Page A is the same person visiting Page B.

**Legitimate uses of cookies include:**
* Keeping you logged in as you navigate.
* Remembering what is in your shopping cart.
* Saving your language preferences.

**Key Definition:** A **First-Party Cookie** is created by the site you are currently visiting (e.g., `nytimes.com`). It is generally useful and safe.
{: .notice}

---

## The Pivot: From Memory to Surveillance

The ethical shift occurred when advertisers realized they could use cookies not just to remember a user *on one site*, but to track them *across all sites*.

This is done using **Third-Party Cookies**.

### Visualizing the Trap

The diagram below illustrates how a single visit to a news site leaks your data to an invisible third party (like Google or Facebook) without you realizing it.

```mermaid
sequenceDiagram
    participant User as 👤 You
    participant Site as 🖥️ News Website
    participant AdTech as 👁️ Ad/Tracking Network

    User->>Site: 1. Request Homepage (GET /)
    Site->>User: 2. Send HTML + "Invisible Tracker Code"
    
    Note over User,AdTech: Your browser sees the tracker code and automatically runs it.
    
    User->>AdTech: 3. Auto-Request Ad Image
    AdTech->>AdTech: 4. Check existing "ID" Cookie
    
    alt No Cookie Found
        AdTech->>User: 5. Set New Unique ID (User_12345)
    else Cookie Found
        AdTech->>AdTech: 6. Log: "User_12345 likes Politics"
    end

[↩ Back to main page](https://sevvara.github.io/ethics_fall2025/casestudy/)
