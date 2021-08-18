---
title: eNodeB and SAS Setup
parent: Infrastructure
---

# eNodeB and SAS Setup

## Introduction
Despite CBRS being a relatively open frequency band, the processes for spectrum access are still somewhat opaque and require significant capital investment and/or ISP-level resources to set up. To clarify this process, here’s a step by step walkthrough tutorial of the setup of a Baicells eNodeB (eNB) base station running in the Citizen’s Broadband Radio Service (CBRS) spectrum band (or band 48).
 
Before following this tutorial, you should have completed the setup of a LTE Evolved Packet Core (EPC) to control your eNB, for which the setup of an open source version based on open5gs is outlined in this [tutorial](https://hackmd.io/brHS3l1-T_uTUaDEAHMOxw?view). 

## I. Get set up with a Spectrum Access System (SAS)

### A. Why get set up with a SAS?
Current FCC regulations require all CBRS equipment (called a CBSD) to be registered on a Spectrum Access System (SAS) that coordinates all spectrum assignments and ensures that no transmissions interfere with each other. This will likely require a commercial agreement with a SAS provider such as Google, Federated Wireless, etc. **This tutorial uses the Google SAS.**

### B. CPI License
At least one member of your team will require “Certified Professional Installer” (CPI) training and license in order to hold legal responsibility for and sign off on device installations. Most SAS providers will offer training at about $500 for both an online training course and the certification exam. If you aren’t able to get someone on your team certified, be sure to collaborate with a CPI! Feel free to contact us at the Local Connectivity Lab if you need support for your community project in this regard, and we can figure out what is feasible.

The following are some links and helpful notes about this process:
* https://wifidevan.wordpress.com/cbrs-certified-professional-installer-cpi-study-notes/
* https://alliancecorporation.ca/webinars/webinars-webinars/cbrs-for-beginners-part-2-by-commscope/
* https://cbrs.wirelessinnovation.org/acronyms

### C. SAS Pricing Agreements

For Google, the price options provided us in summer 2020 were:

* Fixed Wireless
    *  SAS services are billed per link/household so you pay for each CPE (Customer Premises Equipment) CBSD registered with SAS. 
    *  CBSDs that operate as base stations are free of charge. Price Per Customer Link $2.25/month.
*  Mobility/Private LTE (price is based on CBSD categoris)
    *  Category A CBSD 
        *  max transmit capability: 30 dBm/10 MHz = 20 dBm/MHz or “1 Watt”
        *  mounted under 6m Height Above Average Terrain measured 3-16 km away from site
        *  $2.67/month
    *  Category B CBSD 
        *  max transmit capability: Maximum EIRP of 47 dBm/10 MHz = 37 dBm/MHz or “50 Watt"
        *  $13.33/month.

### D. SAS Registration

CBSDs must register their transmit capabilities with the SAS using either the “one-step” or “multi-step” process. 

The one-step process requires you to input all installation parameters and sign them with the CPI certificate all on the base station itself, or via a cloud domain proxy such as used by Baicells. **Not all base stations support this and the interfaces for doing so might vary widely, so “multi-step” is typically recommended.**

## II. Register device in SAS portal
This tutorial will be walking through steps following the specifics of the Google SAS portal interface, but the steps should be generalizable to other SAS portals.

### A. Once you have an account on an SAS service, register your devices on their portal or dashboard.
The Google SAS portal can be found at: https://wirelessconnectivity.google.com/sas/

### B. Our Setup

Our test setup in the lab includes: 
- 1W Baicells Nova 233 base station in the CBRS band mounted on the 6th floor balcony of our UW computer science building.
- Alpha Wireless 18 dBi-gain panel antenna with a beamwidth of 65 degrees (model AW3014-T4), mounted straight ahead and not tilted down. 


### C. Example Configuration
An example configuration for this setup is shown below.

![Google SAS Configuration Screen](https://i.imgur.com/9G0bymT.png)

*The configuration screen is a right-hand sidebar next to the map view, hence the unwieldy aspect ratio.*

Explanation of parameters:
1. CBSD Category (A or B):
    * Defined by rules in Section I.C above
2. User ID
    * Specified by the SAS provider when you register
3. FCC ID and Serial Number:
    * Both the radio and antenna model must be pre-authorized for use with CBRS by the FCC.
    * The FCC ID is used to identify this approved device type.
    * The serial number specifies the exact device identity.
    * Both can usually be found on the outside of the device (circled in image below).
![](https://i.imgur.com/YDAABLk.jpg)
 
4. Beamforming Gain, Beamwidth
    * Based on antenna specs in II.B
5. EIRP
    * [Effective Isotropic Radiated Power](https://www.everythingrf.com/rf-calculators/eirp-effective-isotropic-radiated-power) of your system including both the base station radio and antenna. 
    * For a Cat B CBSD, this must be 46 dBm/10 MHz=36 dBm/MHz or lower. 
    * Calculate this value by adding the max transmit power (actually power density per MHz) of the base station, in our case 28 dBm, to the antenna beamforming gain, in our case 18 dBi; 28+18=36 dBm/MHz. 
    * For the units requested by the Google interface, add 10 to this value to specify power per 10 MHz instead of per MHz. 
6. Height
    * Specified in terms of height Above Ground Level (AGL) which you can measure using a rangefinder/ measuring tape/ building plan, or in height Above Mean Sea Level (AMSL). 
    * **Not** in terms of HAAT as in the Cat A/B definition.
    * Must be accurate to within 3 m.
7. Azimuth
    * Refers to the compass heading/ direction that the antenna is pointing (set this to 0 for an omnidirectional antenna). 
    * This [FCC tool](https://www.fcc.gov/media/radio/distance-and-azimuths) is extremely helpful for calculating the azimuth based on the antenna’s gps location and that of a structure you are pointing it at.
    * You can get these GPS coordinates via Google Maps or Google Earth.
    ![](https://i.imgur.com/SgxORTx.png)

8. Air Interface
    * E_UTRA is the LTE radio standard used by our Baicells box.
    * The only “supported spec” currently available for Baicells is FFS (according to a forum post, linked here).
9. Location: 
    * In the Google interface, set the site location in GPS coordinates under the tab labeled with the map pin icon. *(not shown)*
10. Parameters under "CBSD Info"
    * Call Sign
        *  As far as I can tell, this can be any reasonable alphanumeric string as long as it is unique and matches the value of the “call sign” parameter as sent over by the eNB or domain proxy.
        *  You will set this in the SAS interface as well as either the eNB or Baicells Cloud Core (they all need to match).
    * Others
        * These should match the settings with the same name on the eNB’s local management portal, shown on the “Basic Info” page in section IV.A below.
    
### D. CPI Signature
When the parameters are all filled out, click the big red “Ready for CPI” button at the bottom of the panel (not shown here). On the CPI’s version of the interface, it will provide a place to “sign” the configuration with their CPI certificate, which they will upload to the interface. **This must happen before the device can get a spectrum grant.** 

### E. Status Tab
After the CPI signs the eNB configuration, under the “Status” tab (visible in the config panel), you should see “Not yet Registered” (or a similar message) because the eNB has not checked in to the Google SAS yet with its matching parameters to complete the multi-step process. If something has otherwise gone wrong, you’ll see an error message here.

### F. Other helpful links
* [Google CBSD registration and deregistration](https://support.google.com/sas/answer/9539493?hl=en&ref_topic=9455755)
* [Elevation finder tool with map](https://www.freemaptools.com/elevation-finder.htm)

## III. Steps in Baicells Cloud interface

### A. Make a Baicells OMC account. 
Due to Baicells’ use of a “domain proxy” for their SAS requests, you will need to make a new user account in the Baicells Operators Management Console (OMC): https://cloudcore.baicells.com:4443/ 

This is distinct from their paid “Cloud Core” service which we will not be using in this tutorial, although the management portal is the same.

### B. Take note of the CloudKey
Once you have made an account, note the 6-letter “CloudKey” in the upper right corner of the screen (circled in red). 

![](https://i.imgur.com/thYx40F.png)


This will need to be inputted into the local eNB management portal for the eNB to check into the Cloud OMC. 

*On your version of this portal, if you’re doing this for the first time, you shouldn’t see any eNBs already present.*

### C. Set your SAS service provider. 
Navigate to Advance→SAS in the left hand menu, and then click the gear icon on the upper right corner, which has the hover text “Settings.” 

## IV. Steps in Baicells management interface

### A. Local Management Portal
The Baicells eNodeB (eNB) is best managed through the browser-based management portal; the current command line interface is accessible but extremely limited. 

The default IP address of the management portal (and that of most Baicells equipment I’ve seen) is 192.168.150.1, and the default login credentials are admin/admin. *I would recommend changing the admin login credentials to be more secure.*

Connect your computer to the eNB via Ethernet, and navigate to this IP address in your browser (using http://192.168.150.1, not https).

Baicells Initial Login Screen:

![](https://i.imgur.com/4RlDG7R.png)

BTS Info→“Basic Info” Page visible upon login:

![](https://i.imgur.com/AdSEjMC.png)

### B. Upgrade firmware
Upgrade the firmware to the latest firmware version that supports SAS functionality, or verify that it is already up to date. 

You can check the official [firmware page](https://na.baicells.com/Service/Firmware) under the correct eNB model. The Nova 233 CBRS small cell we’re using is model mBS1105. The latest firmware version after which SAS is officially supported is BaiBS_RTS_3.6.6.IMG (as of Feb 2021), for which the direct download is available [here](https://img.baicells.com//Upload/20200912/FILE/6aaf03fa-7768-40eb-8e1b-ee79f5cb443a.IMG). 

![](https://i.imgur.com/OPL0HYs.png)

***Do not skip this step, otherwise none of the following steps will work right.***

### C. Get everything connected

Once the firmware is upgraded, you will want to get the eNB connected to your local [LTE core network (EPC)](https://hackmd.io/brHS3l1-T_uTUaDEAHMOxw?view) as well as to the Internet so it can contact the necessary SAS infrastructure.

#### 1. Configure Internet Access (WAN)
Navigate to the Network→WAN/LAN/VLAN tab on the left hand menu. 

We will set the WAN interface IP address to 192.168.151.1, since the Baicells console requires (for whatever reason) a different subnet for the WAN as opposed to the LAN. 

Then we will connect the eNB to an Ethernet port on the EPC that has the IP address 192.168.151.2 (as set up in our previous tutorial), which will act as the eNB’s Internet gateway.

**Don’t forget to hit “Save” after each change you make in this interface.**
![](https://i.imgur.com/knqF26j.png)

#### 2. Check Internet access

At this point, if the EPC is configured correctly to pass eNB traffic to the Internet, the eNB should be able to ping an arbitrary IP address. 

To test this, navigate to the Network→Diagnostics tab on the left hand menu and select “Ping” under the “Method of Diagnostics” dropdown menu. Set the “Target IP Domain” to be a highly reachable IP address on the Internet such as 1.1.1.1, which is the CloudFlare DNS server. Press “Implement.” 

If the result is “Fail!” as in the screenshot, there is likely something wrong with your eNB’s Internet connection through the EPC; you should fix this issue before continuing. 
![](https://i.imgur.com/tOgPFxa.png)

#### 3. Reboot as needed
If a message appears that the eNB needs a reboot after the new settings are saved, navigate to the Reboot tab in the left hand menu and perform the reboot (Warm Reset is fine).

#### 4. Attach to Baicells OMC
To configure the eNB to talk to the OMC as discussed in the prior section, navigate to the BTS Setting→Management Server tab in the management console and enter the CloudKey. 

Within a few minutes, the eNB should appear in your Baicells Cloud OMC console, and the “Basic Info” page should show that the OMC is “Connected.”
![](https://i.imgur.com/we7ySwo.png)
![](https://i.imgur.com/Ic7u0II.png)

#### 5. Disable IPsec
For our purposes we will not be using IPsec between our EPC and eNB; the default IPSec configured is used for the Baicells Cloud EPC which we are not using. 

Navigate to the Network→“MME&IPSec Binding” menu tab and set “IPSec Status” to “Disable.” You may also delete the IPSec tunnels as shown below. 
![](https://i.imgur.com/1XO8wj5.png)

#### 6. Disable GPS Sync when testing indoors.
Navigate to the “BTS Setting”→“Sync Setting” menu and disable both “Forced Sync” and “GPS Sync Switch,” in case you need to work with the base station in a location where you don’t have a strong GPS signal. 

Some base stations will not start up normally or attach to the EPC unless they get a GPS signal, and we should avoid this behavior.
![](https://i.imgur.com/RPq0kzq.png)

#### 7. Change the MME settings
Change the MME settings. Since we are using our local EPC, we will need to change the MME settings to reflect our MME’s IP address, on which it is listening for eNBs to attach, as well as other configurations. Navigate to the BTS Info→Quick Setting tab on the left hand menu.

![](https://i.imgur.com/owYmUoJ.png)

* Disable RF
    * You should set the “RF Status” setting to “Disable” before you change the MME IP, because attaching to the MME will normally cause the eNB’s radio to turn on.
    * Since we have not enabled the eNB to ask for spectrum coordinated by the SAS yet, turning on the radio may cause unwanted interference on someone else’s network.
* PLMN setting
    * Remove the existing “PLMN ID” (by clicking the trash can symbol) and set it to the value that you have configured in your EPC.
    * In our networks, we use “91054” as our PLMN, so add this as a “Primary” and “NotReserved” PLMN by entering the number in the text box and clicking the “+” button.
* MME IP address
    * Remove the existing MME IP associated with the old PLMN.
    * Add the new MME IP address, in our case 192.168.150.2, by entering it in the text box and clicking “+”. This MME IP should be associated with the newly added PLMN by default.
    * Save the changes and reboot the eNB (Warm Reset); after the reboot has finished (within a few minutes), the eNB should attach to the MME.
    * If you navigate to BTS Info→Basic Info, you should see the MME Status change from “Not Connected” to “Connected.” If you are looking at the MME logs on the EPC, you will also see the record that an eNB has attached.

#### 8. Enable SAS
**SAS should only be enabled after successfully attaching the eNB to the MME.** Unfortunately, when SAS is enabled, the eNB will not attach to the MME unless it has a currently valid authorization to transmit on a certain frequency. However, until it is attached to an MME, the Baicells Cloud OMC will not provide it this authorization. 

So we need to have SAS disabled first with the RF also disabled, attach the eNB to the MME, and then enable SAS. 

Choose “Multi-step” under “SAS Registration Type,” as specified in Section I.E. Also choose “B” under “category,” and write in the other parameters to match the ones with the same name in the Google SAS configuration.

![](https://i.imgur.com/d5KzckP.png)


After you click “Save,” SAS should be enabled immediately. You should see the SAS enabled status change in the Baicells Cloud OMC. If all goes smoothly, your device should get an authorization to transmit within a few minutes and the radio should turn on!

#### 9. Check Baicells CLOUD OMC to debug issues
You can check the status of the SAS authorization process in the Cloud OMC. Here you can find logs (upper right corner of SAS screen, shown in the screenshot below) with any error messages that may have occurred in the process.
![](https://i.imgur.com/rQntnQ9.png)

* Errors can be caused by invalid or non-matching parameter values, lack of CPI signature, lack of spectrum availability, etc. 
* In more difficult cases, after device registration the SAS may not respond to spectrum inquiries without sending any clear error messages. I have encountered this scenario when requesting spectrum around midnight, which may have been caused by brief database unavailability during the daily “SAS Sync” or IAP. My recommendation is to avoid requesting a new spectrum grant after 11 pm PST.
* If you change anything about the equipment used on site or the location/orientation of the equipment, you need to change the SAS registration, have it re-signed by the CPI, and use the Baicells OMC to re-request a new spectrum authorization- this process is described in the following section.

## V. How to change location, antenna properties, etc. after deployment
As an example, this section will show how you would change the equipment’s location upon moving from test site to deployment site.

1. Get the new GPS location either manually using Google Maps/Earth, or automatically using Baicells OMC’s GPS reading for the eNB if available.
2. Google SAS steps
    * In the upper right hand corner of the Google SAS configuration for the deployed equipment (long narrow right side panel for a particular site), press the unlock button (shaped like a padlock) to make the configuration editable.
    <center>
    
    ![](https://i.imgur.com/B6DB1or.png)
    </center>
    
    * To edit the site location, click on the map pin icon in the upper left corner of this same right hand configuration panel to enter the location panel. Enter the new GPS coordinates in the box. After your changes, lock the site configuration again. (If the red “Ready for CPI” button appears again at the bottom of the main configuration panel, go ahead and click it to prompt the CPI to sign.)
    
    <center>
    
    ![](https://i.imgur.com/mFD3cWc.png)
    </center>
    
    * You may have to wait a few minutes or hours for the changes to sync to the CPI’s SAS database view. If after a while the CPI still cannot see the location change, ask them to enter the new GPS coordinates in their own interface and re-sign the configuration.
3. Baicells Cloud OMC steps
    * On the Baicells OMC, navigate to the Advance→SAS screen where you can see the list of CBRS devices and their SAS status. Click on the 3 dots ( ⠇) symbol before the serial number for a particular device and click on “Procedure” to enter the SAS procedure screen.
    ![](https://i.imgur.com/6zXTVah.png)

    * On the Procedure screen, you can see the most recent SAS logs, relinquish and re-request active spectrum authorizations, or de-register and re-register devices. First click on the “Authorized” icon and click on the “Relinquishment req” button to relinquish the current spectrum authorization. Then the latter two icons will become greyed out, but the device will remain registered. 
    ![](https://i.imgur.com/bjbGzyY.png)

    * We will need to fully de-register and re-register the device with the new parameters. Click the “Registered” icon and then the “De-register” button when it appears to de-register the device. 
    ![](https://i.imgur.com/aW9qDZF.png)

    * Once the device is in the “Unregistered” state, click the “Unregistered” icon and then click the “Register req” button when it appears. If all goes well, the device should re-register, and also request and receive a new grant (completing the full procedure) within a few moments.
