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



































# Exercise 1 - Deploy a Web App to the App Service

1. In the Azure Portal, navigate to the Virtual Machine that you created in Lab 2.
2. From the Virtual Machine portal window, click on **Bastion** from the **Operations** menu
3. From the **Bastion** window, click on **Deploy Bastion**
4. Enter the **Username** and **Password** that you used when you created the VM in Lab 2.
5. Click on **Connect**
6. Click on **Allow** to enable the pasting of clipboard items
7. Open a web browser, and navigate to the URL of the App Service created in Lab 2. (Example:  *https://webapp1.my-ase.appserviceenvironment.net/*).  You should see the default App Service site appear.
    ![image-20220223121452100](.\images\image-20220223121452100.png)
8. Confirm you can not access that same site from the Internet by opening a browser on your computer and navigating to the same URL.
9. On the VM, open Visual Studio
10. Create a web app:
    1. Click on **Create a new project**
    2. In the **Search for templates** search box, enter **ASP.NET Core Web App**
    3. Select **ASP.NET Core Web App**
    4. Click **Next** in the lower bottom right corner
    5. For **Project name**, enter an appropriate name
    6. Click **Next** in the lower bottom right corner
    7. For **Framework**, select **.NET 6.0 (Long-term support)**
    8. For **Authentication type**, select **None**
    9. Leave all other items as default.
    10. Click **Create** in the lower bottom right corner
    11. After the application has created, click on **Debug** -> **Start Debugging**
    12. Confirm the web app starts up with no errors
11. Publish web app:
     1. Right click on the Solution name in the Solution Explorer
    
        ![image-20220224134307755](.\images\image-20220224134307755.png)
    
        ![image-20220224134248500](.\images\image-20220224134248500.png)
    
     2. Select **Publish**
    
     3. In the **Publish** popup window, select **Azure**, then click **Next**
    
     4. Select **Azure App Service (Windows)**, then click **Next**
    
     5. If necessary, log into your Azure account
    
     6. Select the **App service** that you created in Lab 2
    
     7. Click **Finish**
    
     8. Click the **Publish** button to publish to Azure


12. After the app has been published, in the VM, verify you can see new website in a browser.



# Exercise 2 - Configure the App Gateway to point to App Service

1. Navigate to the Web App created in Lab 2.
2. Under the **Settings** menu on the left, click on **TLS/SSL settings**
3. Under **Protocol Settings**, set **HTTPS Only** to **Off** 
4. Navigate to the Application Gateway created in Lab 2.
5. Under the **Settings** menu on the left, click on **Backend pools**.
6. Click on the Backend pool that was created in Lab 2.
7. Under **Target type**, select **App Services** 
8. Under **Target** select the App service created in Lab 2. (Example: *webapp1*)
9. Click **Save**.
10. Under the **Settings** menu on the left, click on **Backend settings**.
11. For **Override with new host name**, select **Yes**
12. For **Host name override**, select **Override with specific domain name** and enter the domain name, without the *https://*.  (Example: *webapp1.my-ase.appserviceenvironment.net*)
13. Click **Save**
14. Under the **Settings** menu on the left, click on **Health probes**.
15. Click **+ Add**.
16. For **Name**, enter an appropriate name.
17. For **Protocol**, select **HTTP.**
18. For **Host**, enter "127.0.0.1"
19. For **Pick host name from backend settings**, select **Yes**.
20. For **Pick port from backend settings**, select **Yes**.
21. For **Path**, enter **/**.
22. For **Backend settings**, select the previously created **Backend Settings**
23. Leave all other settings as default.
24. Click on **Test**
25. After the test runs, confirm there is a green circle with a checkmark under **Status**
26. Click **Save**
27. Open a browser window on your desktop and navigate to the Public IP address of the Application Gateway.
28. In the Application Gateway menu, click on **Overview**.
29. Copy the IP address next to **Frontend public IP address**.
30. Using the IP address from the previous step, verify you can see the website from the App Service in your computer's browser (not the VM).

