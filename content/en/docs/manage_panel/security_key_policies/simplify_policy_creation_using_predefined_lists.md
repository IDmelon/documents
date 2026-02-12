---
title: "Simplify Policy Creation using Pre-defined Lists"
description: "Create security key policy lists to re-use between user and user groups."
lead: ""
date: 2023-09-11T15:18:54+03:30
lastmod: 2023-09-11T15:18:54+03:30
draft: false
images: []
menu:
  docs:
    parent: "conditional_access_for_security_keys"
weight: 54000
toc: true
---

You can define your conditions as a list of items and later use them when creating **Security key Policies**. Here are the steps to define a named list:

## Defining a Named List:

1. **Access Named Lists Section:** Start by navigating to the **Security Policy** section in the admin panel. Look for the **Named Lists** section under the **Security Policy**.
2. In the **Named Lists** section, locate and click on the **New List** button. This action initiates the process of creating a new named list of conditions.
3. **Provide List Details:** In the list creation dialog, you need to provide the following details:
   - **Name:** Enter a descriptive name for the list.
   - **Description:** Optionally, provide a brief description for the list.
   - **List Type:** Choose the type of list you want to create. You have options like IP, Device, RP, Location, and Time.
4. **Add Conditions to the List:**
   - **For IP List:**
     - Click on the **+** icon on the top right corner of the page.
     - Enter the **IP** address or range and click on **Add IP**.
   - **For Device List:**
     - Click on the **Devices** dropdown menu and select a new device.
   - **For RP List:**
     - Click on the **+** icon on the top right corner of the page.
     - Enter the domain **(RP address)** and click on **Add RP**.
   - **For Location List:**
     - Click on the **+** icon on the top right corner of the page.
     - Follow the wizard to locate your desired location on the map.
     - Confirm the location by clicking **Confirm Location**, then click **Confirm** to add it to the list.
   - **For Time List:**
     - Click on the **+** icon on the top right corner of the page.
     - Configure the time period by choosing the **Time Zone**, **Days of the Week**, and specific **Time Period**.
     - Click on **Add Time** to add the new time period to your list.
5. **Save List:** After adding conditions to the list, make sure to save your changes by clicking the **Confirm** button. This action finalizes the creation of the named list.

By following these steps, you’ll be able to define a named list containing specific items that can be used as conditions when creating Security Key Policies. These lists help tailor policies based on factors like IP addresses, devices, relying parties, locations, and time periods, adding a layer of customization and security to your access control.
