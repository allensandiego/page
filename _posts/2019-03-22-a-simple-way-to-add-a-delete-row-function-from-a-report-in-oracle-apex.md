---
layout: post
title: "A simple way to add a delete row function from a report in Oracle APEX"
date: 2019-03-22 04:51:00 +0000
categories: ["Programming"]
---

I had a requirement to add a delete button in an Interactive Report of Oracle APEX. I did some research online and found a few articles discussing on how to do this. But they are using java scripts and dynamic actions.
<br/>
<br/>
However, I wanted to see if there is a simpler way to do this. After giving it some more thought, I figured out something simple. Let me show you.
<br/>
<br/>
Assuming we had an Interactive Report as shown in the Figure below.
<br/>
<br/>
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEizmKtqf3gY6KYMYPGyBcs8Cea5dmpNErimXri-Y-4hXERvG-ty-oXGG7t6AG-AruJAFMWre8Rg1_55RjEVqTMrN7o9vFf-WVuD7GbV5dAs0Ppo9IBX7UrZZ-ilFMFW-Yje3xTuNpCHwL0X/s1600/2019-03-22+12_49_11-Currencies.png" imageanchor="1"><img border="0" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEizmKtqf3gY6KYMYPGyBcs8Cea5dmpNErimXri-Y-4hXERvG-ty-oXGG7t6AG-AruJAFMWre8Rg1_55RjEVqTMrN7o9vFf-WVuD7GbV5dAs0Ppo9IBX7UrZZ-ilFMFW-Yje3xTuNpCHwL0X/s320/2019-03-22+12_49_11-Currencies.png" width="320" height="60" data-original-width="1271" data-original-height="239" /></a>
<br/>
<br/>
Now we add a column link. We can do this by adding an additional item in the query or using the Interactive Report Attributes. I'll use the first approach in this case. See screen shots below.
<br/>
<br/>
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiHm2GYC4hKaIuL2u8QX6aGWkUtN2wKgFL6mtSvElBF2HKK2TmQQvLrcE9M_DxAh-q2tOhSAozztUytYQ-8stuOTc1Gw2X1aBKV5Q6xb7XD-hdX9u188hYsPGJX-EgvJ8AeOvPfZtH2G37a/s1600/2019-03-22+12_54_01-Page+Designer.png" imageanchor="1"><img border="0" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiHm2GYC4hKaIuL2u8QX6aGWkUtN2wKgFL6mtSvElBF2HKK2TmQQvLrcE9M_DxAh-q2tOhSAozztUytYQ-8stuOTc1Gw2X1aBKV5Q6xb7XD-hdX9u188hYsPGJX-EgvJ8AeOvPfZtH2G37a/s320/2019-03-22+12_54_01-Page+Designer.png" width="320" height="112" data-original-width="1356" data-original-height="475" /></a>
<br/>
<br/>
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi2LL9iozLE7_bYT4Li7xhgjrr9BtwwSIF1aq8x3oz1OkRSyLWcPPk-bzvNch0huR-zeTlRDt8RFntp7cB6m2QCxwHYV0SLrP1trN7tkL3cubVmJ_8Wq2h43-E93k0gd_br0Sokl99A_j-M/s1600/2019-03-22+12_59_08-Page+Designer.png" imageanchor="1"><img border="0" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi2LL9iozLE7_bYT4Li7xhgjrr9BtwwSIF1aq8x3oz1OkRSyLWcPPk-bzvNch0huR-zeTlRDt8RFntp7cB6m2QCxwHYV0SLrP1trN7tkL3cubVmJ_8Wq2h43-E93k0gd_br0Sokl99A_j-M/s320/2019-03-22+12_59_08-Page+Designer.png" width="320" height="135" data-original-width="1357" data-original-height="573" /></a>
<br/>
<br/>
At this point, we have an additional column in the report (see screen shot below) but it doesn't do anything yet.
<br/>
<br/>
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjjwY6tnGJLXuROR0UGdSxJ1BGJkZ_RApSyprgvW0499f72_d9tdLSIEOXPE9x2LpnmyMSlEhS2rKLhlQZ2lV0RvgkdYTxkEUEknf6Bi8ygZvChZdBafFkS0o4qC41HGqiVq5zkUf_nOJ_r/s1600/2019-03-22+13_05_07-My+Table.png" imageanchor="1"><img border="0" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjjwY6tnGJLXuROR0UGdSxJ1BGJkZ_RApSyprgvW0499f72_d9tdLSIEOXPE9x2LpnmyMSlEhS2rKLhlQZ2lV0RvgkdYTxkEUEknf6Bi8ygZvChZdBafFkS0o4qC41HGqiVq5zkUf_nOJ_r/s320/2019-03-22+13_05_07-My+Table.png" width="320" height="64" data-original-width="1290" data-original-height="259" /></a>
<br/>
<br/>
<h3>Now on to the solution.</h3>
<br/>
Create a blank page and set it as a Modal Dialog.
<br/>
<br/>
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEim2SrHcN56OEKl29MXNBW5F6CHVWFFP1PQXz6PrXP9d1DEwclDzRa8ZqqvWQ2BrelMrRw4pY0Z6UH5GSRzytFc4cvmuq_CfoKiijl080BmC6nBRzEZaQ4D3gDUs4H0fdLpXpmWPcUDDsfG/s1600/2019-03-22+13_26_35-Page+Designer.png" imageanchor="1"><img border="0" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEim2SrHcN56OEKl29MXNBW5F6CHVWFFP1PQXz6PrXP9d1DEwclDzRa8ZqqvWQ2BrelMrRw4pY0Z6UH5GSRzytFc4cvmuq_CfoKiijl080BmC6nBRzEZaQ4D3gDUs4H0fdLpXpmWPcUDDsfG/s320/2019-03-22+13_26_35-Page+Designer.png" width="320" height="239" data-original-width="793" data-original-height="592" /></a>
<br/>
<br/>
Create a Region and set the Type as Static Content under Identification. Set the region's Text field under Source to "Would you like to perform this delete action?". Set the region's Template under Appearance to Blank with Attributes (No Grid).
<br/>
<br/>
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjGCped63NLVPpoDMAIw0O21tbNPY3WJOmtvrMEOM6cxH8OjbkpKrObXbbrwYYoyQDkwjRIqikF5kZScvZ6adX16czNTKplsdz3T4uX3_Idyg1-5YWUHMbbNvu8kyocTFBzt2XOQcAs3d0k/s1600/2019-03-22+13_28_22-Page+Designer.png" imageanchor="1"><img border="0" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjGCped63NLVPpoDMAIw0O21tbNPY3WJOmtvrMEOM6cxH8OjbkpKrObXbbrwYYoyQDkwjRIqikF5kZScvZ6adX16czNTKplsdz3T4uX3_Idyg1-5YWUHMbbNvu8kyocTFBzt2XOQcAs3d0k/s320/2019-03-22+13_28_22-Page+Designer.png" width="320" height="115" data-original-width="1356" data-original-height="486" /></a>
<br/>
<br/>
Add a page item to the region and name it to PX_ID (where X is the page number). Set it to Hidden.
<br/>
<br/>
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEis2U3QlWlC1z0HkJtlMJmnW16gS9kqWnh-1m2NapeeaV9wXNzC0vRBEPOWZEObww6dC2Z4Ri6pN3yDMn-MpdfLadS1saH-G6pNnIPf56r2mwWOp0z53S60uJdBlKiaNz2c6ZA2Rzn3d5lG/s1600/2019-03-22+13_32_22-Page+Designer.png" imageanchor="1"><img border="0" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEis2U3QlWlC1z0HkJtlMJmnW16gS9kqWnh-1m2NapeeaV9wXNzC0vRBEPOWZEObww6dC2Z4Ri6pN3yDMn-MpdfLadS1saH-G6pNnIPf56r2mwWOp0z53S60uJdBlKiaNz2c6ZA2Rzn3d5lG/s320/2019-03-22+13_32_22-Page+Designer.png" width="320" height="75" data-original-width="1356" data-original-height="319" /></a>
<br/>
<br/>
Add a Cancel button in the region and create a Cancel Dialog Dynamic Action to the Cancel button.
<br/>
<br/>
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgk4njpTax6TeMO8b-tUN1bQ03VJ6Xzo3V6dTxSXWUKCpLGs1D5m2bV4JZxd3SzJMjGLLakKm82vWd51LaGfaJ2fb5z0x0zE5CPPv3sBHl7_1BTfAguTKpDVfysKS2EymYHPp5aeLb26lhK/s1600/2019-03-22+13_33_46-Page+Designer.png" imageanchor="1"><img border="0" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgk4njpTax6TeMO8b-tUN1bQ03VJ6Xzo3V6dTxSXWUKCpLGs1D5m2bV4JZxd3SzJMjGLLakKm82vWd51LaGfaJ2fb5z0x0zE5CPPv3sBHl7_1BTfAguTKpDVfysKS2EymYHPp5aeLb26lhK/s320/2019-03-22+13_33_46-Page+Designer.png" width="320" height="102" data-original-width="1356" data-original-height="434" /></a>
<br/>
<br/>
Add an OK button in the region.
<br/>
<br/>
Create a process with Type of PL/SQL Code. Select the button OK on When Button Pressed under Server-side Condition. Then in the PL/SQL Code under Source, enter the appropriate code to delete the record. In my case, I need to execute the following:
<pre>DELETE FROM my_table WHERE id = :PX_ID;</pre>
<br/>
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhHyPyBhEC3aciSij7wfrGovQYZI4DmZJmI9dxdR3l9foVId-qPlFT6N1CFv3H95LmCgzOL4mIGZSkGbH02G_Jh7ly8O1YZ7BOL8xTCPChFESHbVFhk2IpDDzzue62LGvLCnNRUhHyyVkD_/s1600/2019-03-22+13_35_57-Page+Designer.png" imageanchor="1"><img border="0" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhHyPyBhEC3aciSij7wfrGovQYZI4DmZJmI9dxdR3l9foVId-qPlFT6N1CFv3H95LmCgzOL4mIGZSkGbH02G_Jh7ly8O1YZ7BOL8xTCPChFESHbVFhk2IpDDzzue62LGvLCnNRUhHyyVkD_/s320/2019-03-22+13_35_57-Page+Designer.png" width="320" height="126" data-original-width="1349" data-original-height="530" /></a>
<br/>
<br/>
Create a process with Type of Close Dialog. Select the button OK on When Button Pressed under Server-side Condition.
<br/>
<br/>
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg-qVILB-AM88eTLHdDbNIz2MViZZVQ-FNq2VcSFmalXajw7G_ECXQnoqn7kozsMJvpyMTahx1QiY5QMAKr7YcNPnBlYCbtCz8apUa5g0fPeYYRXjUwSBJsi39ZI7ZXgcT5h8yrTGyHxf30/s1600/2019-03-22+13_36_56-Page+Designer.png" imageanchor="1"><img border="0" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg-qVILB-AM88eTLHdDbNIz2MViZZVQ-FNq2VcSFmalXajw7G_ECXQnoqn7kozsMJvpyMTahx1QiY5QMAKr7YcNPnBlYCbtCz8apUa5g0fPeYYRXjUwSBJsi39ZI7ZXgcT5h8yrTGyHxf30/s320/2019-03-22+13_36_56-Page+Designer.png" width="320" height="113" data-original-width="1350" data-original-height="475" /></a>
<br/>
<br/>
Let's go back to the report page and edit the following properties for the DELETE_LINK column.
<br/>
<br/>
Under Link, set Target to Page in this application.
<br/>
<br/>
Under Page, select the modal page we just created.
<br/>
<br/>
Under Set Items, select the PX_ID (where X is the page number of modal page) for Name and PY_ID (where Y is the page number of report page).
<br/>
<br/>
Under Clear Cache, enter the page number of the modal page then click OK.
<br/>
<br/>
<br/>
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjkOQJLv9FLYB3-RdpJB4FjXOuT8NyzL6ESA1q1_IRZOFaq5wlPsAK8nVuvKU_p5aD9QqoZHi2pYn9HMmqLixq1gv6eALapwOyuRAojdIH33tLCUUAU-1zNoF_dKxu-quN85A-uwOVocwSG/s1600/2019-03-22+13_38_08-Page+Designer.png" imageanchor="1"><img border="0" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjkOQJLv9FLYB3-RdpJB4FjXOuT8NyzL6ESA1q1_IRZOFaq5wlPsAK8nVuvKU_p5aD9QqoZHi2pYn9HMmqLixq1gv6eALapwOyuRAojdIH33tLCUUAU-1zNoF_dKxu-quN85A-uwOVocwSG/s320/2019-03-22+13_38_08-Page+Designer.png" width="320" height="307" data-original-width="523" data-original-height="502" /></a>
<br/>
<br/>
In the Link Text under Link, let's change this to a trash icon. Enter the following:
<br/>
<pre><code>&lt;span aria-hidden="true" class="fa fa-trash"&gt;&lt;/span&gt;</code></pre>
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhGtNsUHThjS7RCTNmr9c1br8HPjJOtDZ9TqwabyBdFBiB58G5gokKSLT134L1YYwygWYYfCa2tKAhyVLoZ7dvLd60v1d14x81O7SuFF7VhmabX5Fh2b5OMO8E-cL4iUZDjUP9p3vXsG92q/s1600/2019-03-22+13_51_32-Page+Designer.png" imageanchor="1"><img border="0" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhGtNsUHThjS7RCTNmr9c1br8HPjJOtDZ9TqwabyBdFBiB58G5gokKSLT134L1YYwygWYYfCa2tKAhyVLoZ7dvLd60v1d14x81O7SuFF7VhmabX5Fh2b5OMO8E-cL4iUZDjUP9p3vXsG92q/s320/2019-03-22+13_51_32-Page+Designer.png" width="320" height="71" data-original-width="1346" data-original-height="299" /></a>
<br/>
<br/>
Now all that's left to do is to run it.
<br/>
<br/>
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhjQsM3Odb0i80aG8OijZdDhdGspm0oRPOojYW2TukvtIEjWPpyYmNy1dOdNRTPFPfyew5oWiscsxbKwGZO5Ym3tjMqHBzcypJUz8PiCzJgzfEd2bGHerkHnDuWvvl_C4amj6cZNZdF4ReH/s1600/2019-03-22+13_44_40-My+Table.png" imageanchor="1"><img border="0" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhjQsM3Odb0i80aG8OijZdDhdGspm0oRPOojYW2TukvtIEjWPpyYmNy1dOdNRTPFPfyew5oWiscsxbKwGZO5Ym3tjMqHBzcypJUz8PiCzJgzfEd2bGHerkHnDuWvvl_C4amj6cZNZdF4ReH/s320/2019-03-22+13_44_40-My+Table.png" width="320" height="102" data-original-width="1305" data-original-height="418" /></a>
<br/>
<br/>
That's it. Enjoy!
