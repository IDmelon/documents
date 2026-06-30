---
title: "Shared iPad"
description: "Set up IDmelon Authenticator on a shared iPad, from a blank device to Microsoft sign-in."
lead: "Set up a shared iPad your whole team can sign in to."
date: 2026-06-30T00:00:00-07:00
lastmod: 2026-06-30T00:00:00-07:00
draft: false
images: []
type: docs
weight: 62050
toc: true
---

A shared iPad lets a team pass one device around and have each person sign in as
themselves. Someone walks up, taps their badge, and your apps — Teams, Outlook, and
anything you put on the home screen — open as them. When they finish, the iPad signs out
and is ready for the next person.

These pages take you from a blank, enrolled iPad to that experience. You don't need to
know what MSAL, Entra, or a property list is before you start. Each page tells you
exactly what to paste and what to change.

## What you'll set up

1. **The shared device** — install IDmelon Authenticator and turn the iPad into a shared
   device. People sign in by tapping a badge over the IDmelon Hub by default; face on the
   camera is an option.
2. **Microsoft sign-in** — the default. Connect it so Teams, Outlook, and other Microsoft
   apps open without a second login. Skip it if your team doesn't use Microsoft apps.
3. **A check** — confirm a real sign-in works before you hand out devices.

## What you need

- iPads on **iPadOS 17 or later**, already enrolled in Microsoft Intune (supervised
  recommended).
- **IDmelon Authenticator** in your Intune app catalog.
- A **Shared Mobile API key** from the [IDmelon Admin Panel](https://panel.idmelon.com).
  This one key is what turns the app into a shared device.
- For Microsoft sign-in: access to **Microsoft Entra** and **Microsoft Authenticator** in
  your catalog.

If you don't hold all of these logins yourself, each page notes who to ask.

## Where you'll work

- **IDmelon Admin Panel** — create the API key.
- **Microsoft Intune** — install the app and push its settings.
- **Microsoft Entra** — for the Microsoft sign-in steps.

## Follow these pages in order

1. **[Set up a shared iPad](set_up_a_shared_ipad)** — get one iPad working end to end,
   including Microsoft sign-in.
2. **[User Experience](what_users_see)** — the end-user experience, so you can spot a
   healthy device at a glance.

The other pages under Shared iPad — settings, login methods, customization, and fixes —
are there when a step points you to them.
