# Activate Azure with App Service Environment

## Student Lab Manual

*Version 1.0*

*February 2022*



------

**Conditions and Terms of Use**

Microsoft Confidential 

 

This training package is proprietary and confidential, and is intended only for uses described in the training materials. Content and software is provided to you under a Non-Disclosure Agreement and cannot be distributed. Copying or disclosing all or any portion of the content and/or software included in such packages is strictly prohibited.

The contents of this package are for informational and training purposes only and are provided "as is" without warranty of any kind, whether express or implied, including but not limited to the implied warranties of merchantability, fitness for a particular purpose, and non-infringement.

Training package content, including URLs and other Internet Web site references, is subject to change without notice. Because Microsoft must respond to changing market conditions, the content should not be interpreted to be a commitment on the part of Microsoft, and Microsoft cannot guarantee the accuracy of any information presented after the date of publication. Unless otherwise noted, the companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place, or event is intended or should be inferred. 

 

© 2022 Microsoft Corporation. All rights reserved.



------





















**Copyright and Trademarks**

© 2019 Microsoft Corporation. All rights reserved.

 

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation. 

For more information, see Use of Microsoft Copyrighted Content at
 http://www.microsoft.com/en-us/legal/intellectualproperty/Permissions/default.aspx

 

Azure, Microsoft, MSDN, Microsoft Dynamics logo, Visual Studio, Visual Studio Design, 2019, Windows Server, and Windows are either registered trademarks or trademarks of Microsoft Corporation in the United States and/or other countries. Other Microsoft products mentioned herein may be either registered trademarks or trademarks of Microsoft Corporation in the United States and/or other countries. All other trademarks are property of their respective owners.



------







































## Contents

[TOC]



































# Exercise 1 - Create an Application Gateway in the ASEv3

1. On the Azure portal menu or from the **Home** page, select **Create a resource**. 

2. In the **Search services and marketplace** search bar, search for **Application Gateway**

3. Select **Create** -> **Application Gateway**

4. From the **Basics** tab:

   1. Select the **Subscription** and **Resource Group** that was used in Module 1.
   2. For the **Application Gateway name**, enter an appropriate name. 
   3. For **Region**, select the appropriate region.
   4. For the **Tier** dropdown, select **Standard V2**
   5. For **Enable autoscaling**, choice **No**
   6. For **Instance count**, enter **1**
   7. For **Firewall status**, select **Enable**
   8. For **Firewall mode**, select **Prevention**
   9. For **Availability zone**, select **None**
   10. For **HTTP2**, select **Disabled**
   11. For **Virtual Network**, select the virtual network created in Module 1 - Lab 1
   12. For **Subnet**, select the application gateway subnet created in Module 1 - Lab 1 (Example: appgw-subnet).
   13. Click on **Next: Frontends**

5. From the **Frontends** tab:

   1. For **Frontend IP address type**, select **Public**
   2. For **Public IP address**, click on **Add new**
   3. In the **Add a public IP** popup:
      1. For **Name**, give the public IP a unique name (Example: ase-appgw-pip)
      2. Click on **OK**
   4. Click on **Next: Backends**

6. From the **Backends** tab:

   1. Select **Add a backend pool**.

   2. In the **Add a backend pool** window that opens, enter the following values to create an empty backend pool:

      1. For **Name**, enter an appropriate name for the name of the backend pool. (Example: *BackendPool1*)

      2. For **Add backend pool without targets**, select **Yes** to create a backend pool with no targets. You'll add backend targets after creating the application gateway and app service.
      3. Click **Add**
      4. Click on **Next: Configuration**

   3. From the **Configuration** tab

      1. Click **Add a routing rule** in the **Routing rules** column.

      2. In the **Add a routing rule** window that opens:

         1. For the **Rule name**, enter an appropriate rule name. (Example:  *RoutingRule1*)

         2. For the **Priority**, enter 1

         3. On the **Listener** tab within the **Add a routing rule** window:

            1. For **Listener name**: Enter an appropriate listener name. (Example:  *Listener*)

            2. For **Frontend IP Protocol**, select **Public** and **HTTP**

            3. For **Listener type**, select **Basic**
   
            4. For **Error page url**, select **No**
   
               ![image-20220223082406069](.\images\image-20220223082406069.png)
   
         4. On the **Backend Targets** tab within the **Add a routing rule** window:
   
            1. For **Target type**, select **Backend pool**
   
            2. For **Backend target**, select the backend pool that was created in the previous steps. (Example: *BackendPool1*)
   
            3. For **Backend settings**, click **Add new**
   
            4. In the **Add a Backend setting** popup window:
   
               1. For **Backend settings name**, enter an appropriate name. (Example: *BackendSettings1*)
   
               2. For **Backend protocol**, select **HTTP**
   
               3. For **Backend port**, enter **80**
   
               4. For **Cookie-based affinity**, select **Disable**
   
               5. For **Connection draining**, select **Disable**
               
               6. For **Request time-out (seconds)**, enter **20**
               
               7. For **Override backend path**, leave blank
               
               8. For **Override with new host name**, select **No**
               
               9. Click **Add**
               
            5. In the **Add a routing rule**, click **Add**
   
         5. In the **Add a backend pool** window, select **Add** to save the backend pool configuration and return to the **Backends** tab.

      3. Click on **Next: Tags** 
   
   4. From the **Tags** tab:
   
      1. Click on **Next: Review + Create >**
   
   5. From the **Review + Create** tab:
   
      1. Click on **Create**
   
   
    
   



# Exercise 2 - Create a Virtual Machine (Jumpbox) to access resources 

1. On the Azure portal menu or from the **Home** page, select **Create a resource**. 

2. In the **Search services and marketplace** search bar, search for **Virtual machine** or select **Virtual Machine** from the **Popular Azure services** menu.

3. Click on **Create**

4. Enter these values in the **Basics** tab for the following virtual machine settings:

   1. For **Subscription** and **Resource group**, select the resource group from previous exercises

   2. For **Virtual machine name**, enter a name for the name of the virtual machine.

   3. For **Region**, select the same region where you created the application gateway.

   4. For **Availability options**, select **No infrastructure redundancy required**

   5. For **Security type**, select **Standard**

   6. For **Image**, click on **See all images**

      1. From the **Select an image** window, in the **Search the Marketplace** search box, type in **Visual Studio**

      2. Select an appropriate VM image that includes Visual Studio (Example: *Visual Studio 2022 Community (latest release) on Windows 11 Enterprise N (x64) - Gen 1*)

         ![image-20220223090628907](.\images\image-20220223090628907.png)

   7. For **Azure Spot instance**, leave check box unchecked

   8. For **Size**, select **Basic_A1** or another appropriate size.

   9. For **Username**, type a name for the administrator user name.

   10. For **Password**, type a password.

   11. For **Public inbound ports**, select **None**.

   12. Click on **Next: Disks**

6. Accept the **Disks** tab defaults and then select **Next: Networking**.

6. On the **Networking** tab:

   1. For **Virtual network**, select the virtual network created in Lab1.
   2. For **Subnet**, select the **default** subnet
   3. For **Public IP** select, none
   4. Accept the other defaults and then click **Next: Management**.

7. On the **Management** tab, set **Boot diagnostics** to **Disable**. Accept the other defaults and then click **Review + create**.

8. On the **Review + create** tab, click **Create**.





# Exercise 3 - Create an App Service for a Web App in the ASEv3

​	NOTE: This exercise must be done after the App Service Environment has been created and fully deployed.

1. On the Azure portal menu or from the **Home** page, select **Create a resource**. 

2. In the **Search services and marketplace** search bar, search for **Web App** or select **Web App** from the **Popular Azure services** menu.

3. Search for **Web App**

   ![image-20220223095041618](.\images\image-20220223095041618.png)

   4. Select **Create** -> **Web App**

   5. From the **Basics** tab of the **Create Web App** Wizard:

      1. Select the **Subscription** and **Resource Group** used in Lab 1
      2. For **Name**, enter a name. (Example: *webapp1*).  NOTE: Because we will be adding this to our ASE in a later step, the name doesn't need to be globally unique, so you can ignore the *"The app name webapp1 is not available"* error if you get it.
      3. For **Publish**, select **Code**
      4. For **Runtime stack**, select **.NET 6 (LTS)**
      5. For Operating System, select **Windows**
      6. For **Region**, scroll to the top of the selection box and under **App Service Environment v3** select the ASEv3 created in Lab 1
      7. For **Windows Plan** you can keep the defaults shown.
      8. Click on **Next: Deployment**
   
   6. From the **Deployment** tab:
   
      1. For **Continuous deployment**, select **Disable**
   
   7. For the **Monitoring** tab:
   
      1.  For **Enable Application Insights**, select **Yes**
      2.  For **Application Insights**, click on **Create new**
      3.  In the **Create new Application Insights** window:
          1. Type in a **Name**.  (NOTE: We will use this Application Insights for other aspects of the ASE)
          2. For **Location** select the appropriate Location.
          3. Click on **OK**
          4. Click on **Create + Review**
      4.  From the **Review + create** tab, check that your configuration is correct, and select **Create**.
      
      NOTE:  The App Service deployed into an ASE can take up to 30 minutes to create

