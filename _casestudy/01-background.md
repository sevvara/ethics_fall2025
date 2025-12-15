---
title: "Background"
layout: single 

collection: casestudy
permalink: /casestudy/background/
author_profile: true
---
Cookies were originally designed to help websites remember users, not to track them across the internet. This section explains how a simple technical tool evolved into the foundation of modern data tracking.

## Background: How Cookie Tracking Works

Cookies were originally introduced in 1994 by Netscape engineer Lou Montulli to solve a simple technical problem: the early web had no memory. Each webpage loaded independently, and browsers had no way to know whether the person viewing one page was the same person who had just visited another.

Cookies provided a solution. By storing a small text file in the user’s browser, websites could remember basic information, such as login status or shopping cart contents, across multiple pages. At this stage, cookies were a tool for functionality and convenience, not surveillance.

Understanding this original purpose is essential to understanding how cookies later became ethically contentious.

## What Is a Cookie?

A cookie is a small text file saved by your browser at the request of a website. It allows the site to recognize your device and remember limited information between visits or page loads.

Common, legitimate uses of cookies include:
- Keeping users logged in across pages  
- Remembering items in a shopping cart  
- Saving language or accessibility preferences  

### Key Definition

**First-Party Cookie**  
A cookie created and accessed by the site you are intentionally visiting (for example, `nytimes.com`). These cookies generally support core site functionality and are widely considered low risk.

At this stage, cookies functioned as local memory, not as tracking tools.

## The Shift: From Memory Tool to Tracking Infrastructure

The ethical shift began when advertisers realized that cookies could do more than store preferences on a single site. They could be used to recognize the same browser across *many* sites.

This led to the rise of **third-party cookies**, which are created not by the site a user chooses to visit, but by external services embedded within that site. These commonly include:
- advertising networks  
- analytics scripts  
- social media buttons  
- invisible tracking pixels  

When a webpage loads, these third-party elements load as well. As a result, the same external company can observe a user’s behavior across dozens, or hundreds, of unrelated websites.

Over time, this enables the construction of detailed behavioral profiles based on browsing habits, interests, location signals, and device information.

This system now forms the backbone of the modern ad-tech ecosystem.

## Visualizing the Tracking Flow

From the user’s perspective, visiting a website feels simple. They see text, images, and headlines. Behind the scenes, however, the browser may be making multiple invisible requests to external domains—often before the user scrolls or interacts with the page.

The diagram below illustrates a simplified version of how a single page visit can transmit data to an advertising or tracking network.

```mermaid
sequenceDiagram
    participant User as 👤 You
    participant Site as 🖥️ News Website
    participant AdTech as 👁️ Ad / Tracking Network

    User->>Site: 1. Request homepage
    Site->>User: 2. Send HTML with embedded tracker

    Note over User,AdTech: The browser automatically loads third-party scripts.

    User->>AdTech: 3. Auto-request ad image or script
    AdTech->>AdTech: 4. Check for existing tracking ID

    alt No cookie found
        AdTech->>User: 5. Set new unique ID
    else Cookie found
        AdTech->>AdTech: 6. Update behavioral profile
    end
