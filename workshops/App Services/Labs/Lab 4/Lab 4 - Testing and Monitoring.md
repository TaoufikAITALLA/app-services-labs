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


































# Exercise 1 - Create a Load Test

For this exercise, follow the tutorial at:

https://docs.microsoft.com/en-us/azure/load-testing/tutorial-identify-bottlenecks-azure-portal#configure-and-create-the-load-test

Follow the tutorial with the exception of these items:

1. Clone the code sample that is provided, but **do not** run the deployment script. You will be using the App Service that was created in the previous labs, however you will to download/clone the code in order to get the provided JMeter script.
2. When you enter the parameters in ** Create a load test** Step 4, enter in the IP address of the App Gateway that you created in the previous lab.

