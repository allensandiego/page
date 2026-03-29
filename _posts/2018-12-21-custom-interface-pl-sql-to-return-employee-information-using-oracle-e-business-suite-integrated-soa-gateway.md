---
layout: post
title: "Custom Interface PL/SQL to return Employee Information using Oracle E-Business Suite Integrated SOA Gateway"
date: 2018-12-21 04:40:00 +0000
categories: ["Programming"]
---

In my previous posts, I wrote about how to create custom interface types using Java as part of the Oracle E-Business Suite Integrated SOA Gateway. In this post, I will be building the service using the same requirement I had in the Application Module Service. Only this time, I will be using PL/SQL.
<br />
<br />
First thing we'll do is to create our package scripts. One for the specification and one for the body.
<br />
<br />
Here's the package specification. Filename will be xxhr_soa_pkg.pls
<pre><code class="pgsql hljs">create or replace package APPS.xxhr_soa_pkg as.
/* $Header: $ */
/*#
* This is the a sample custom HR SOA Web Service
* @rep:scope public
* @rep:product per
* @rep:category BUSINESS_ENTITY HR_USER_HOOK
* @rep:displayname HR SOA Web Service
*/

  type person_rec is record ( person_id             number
                            , effective_start_date  date
                            , effective_end_date    date
                            , employee_number       varchar2(30)
                            , marital_status        varchar2(30)
                            , date_of_birth         date );
                            
  type person_tbl is table of person_rec;

/*#
 * This API returns a list of person information.
 * @param p_employee_number Employee Number of the Person
 * @return This API will return person_tbl type.
 * @rep:displayname Get Employees
 * @rep:scope public
 * @rep:lifecycle active
*/
  function get_employees ( p_employee_number in varchar2 ) return person_tbl;
end;
/
</code></pre>
Here's the package body and the filename will be xxhr_soa_pkg.plb.
<pre><code class="pgsql hljs">create or replace package body APPS.xxhr_soa_pkg as

  function get_employees ( p_employee_number in varchar2 ) return person_tbl
  is
    l_employees person_tbl;
  begin
    select person_id
         , effective_start_date
         , effective_end_date
         , employee_number
         , marital_status
         , date_of_birth
      bulk
   collect
      into l_employees
      from per_people_x
     where hr_person_type_usage_info.get_user_person_type(sysdate,person_id) like 'Employee%'
       and employee_number = nvl(p_employee_number,employee_number);
       
    return l_employees;
  end;
end;
/
</code></pre>
Note that only the package specification requires the appropriate annotations. More information on PL/SQL Annotations <a href="https://docs.oracle.com/cd/E18727_01/doc.121/e12065/T511473T545912.htm#sigdg_appb_plsql" target="_blank">here</a>.
<br />
<br />
Of course, we also need to compile the package scripts into the target database. After that, we need to move these files (the specification script at the least) so that we can generate the iLDT file similar to what we did in the previous posts. See command below.
<br />
<pre><code>[las@server ~/soa-plsql]$ $IAS_ORACLE_HOME/perl/bin/perl $FND_TOP/bin/irep_parser.pl -g -v -username=sysadmin per:patch/115/sql:xxhr_soa_pkg.pls:12.0=xxhr_soa_pkg.pls</code></pre>
If everything went well, the expected output should be somewhat similar to this.
<br />
<pre><code>[las@server ~/soa-plsql]$ $IAS_ORACLE_HOME/perl/bin/perl $FND_TOP/bin/irep_parser.pl -g -v -username=sysadmin per:patch/115/sql:xxhr_soa_pkg.pls:12.0=xxhr_soa_pkg.pls
# Interface Repository Annotation Processor, 12.0.0

#
# Generating annotation output.
# Processing file 'xxhr_soa_pkg.pls'.
# Using YAPP-based parser.
#  Found a package-level annotation for 'APPS.XXHR_SOA_PKG'.
#  Found a detail-level annotation...
# Found a function named 'GET_EMPLOYEES'.
# Done all files.
[las@server ~/soa-plsql]$
</code></pre>
Next, we need to upload the iLDT file to our Integrated SOA repository. We can do this by executing the fndload command with the appropriate lct file as shown below.
<pre><code>[las@server ~/soa-plsql]$ FNDLOAD apps/&lt;apps_password&gt; 0 Y UPLOAD $FND_TOP/patch/115/import/wfirep.lct</code></pre>
Again if successful, expected output should be somewhat similar to this.
<pre><code>[las@server ~/soa-plsql]$ FNDLOAD apps/&lt;apps_password&gt; 0 Y UPLOAD $FND_TOP/patch/115/import/wfirep.lct xxhr_soa_pkg_pls.ildt
Log filename : L6603415.log


Report filename : O6603415.out
[las@server~/soa-plsql]$
</code></pre>
Now let’s check the service in the Repository.
<br />
<br />
Login to your Oracle E-Business Suite instance as SYSADMIN.
<br />
<br />
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhE1Wlt2Naon2Swv-7LLkyRbvVklmFG9AMmiR1eTNe9VMeKTTB9BINXRSzfxuSqt_lg17zSw7ncntIDrQKKvHD_kdkZMj6hFJ7IAdmFj73WF9K2yT8gidNwa68TySTbX-zjIe-KPZHKhH0i/s1600/2018-12-06+16_20_20-Login+-+Internet+Explorer.png" imageanchor="1" style="margin-bottom: 1em; margin-right: 1em;"><img border="0" data-original-height="221" data-original-width="625" height="113" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhE1Wlt2Naon2Swv-7LLkyRbvVklmFG9AMmiR1eTNe9VMeKTTB9BINXRSzfxuSqt_lg17zSw7ncntIDrQKKvHD_kdkZMj6hFJ7IAdmFj73WF9K2yT8gidNwa68TySTbX-zjIe-KPZHKhH0i/s320/2018-12-06+16_20_20-Login+-+Internet+Explorer.png" width="320" /></a>
<br />
<br />
Switch to Integrated SOA Gateway responsibility.
<br />
<br />
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgyR2NgZU0JBJ4D0Pjs8CzLdu9ZeZ3w_EBsMSxeE9iQWPdCvA31tYa2gL0uoND_H6wSbsbH5QPfnYiqcrgV2gHLogwR9DWoa22CNdAkDpqvSIvyTzgmcMOmjk8GlxzK1dMgoTBpDUa-acWv/s1600/2018-12-06+16_21_51-Oracle+Applications+Home+Page+-+Internet+Explorer.png" imageanchor="1" style="margin-bottom: 1em; margin-right: 1em;"><img border="0" data-original-height="431" data-original-width="380" height="320" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgyR2NgZU0JBJ4D0Pjs8CzLdu9ZeZ3w_EBsMSxeE9iQWPdCvA31tYa2gL0uoND_H6wSbsbH5QPfnYiqcrgV2gHLogwR9DWoa22CNdAkDpqvSIvyTzgmcMOmjk8GlxzK1dMgoTBpDUa-acWv/s320/2018-12-06+16_21_51-Oracle+Applications+Home+Page+-+Internet+Explorer.png" width="282" /></a>
<br />
<br />
Select Integration Repository
<br />
<br />
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg2vvgp7ThistDg3pNSXdLTwZjBlETd6TndcNUPUEDcEByG3KfJfhOPMhXYxPMmrcBKBfxouv5eBouNtyOogIC5hk4imrkoCxN5uu_QxBIJo0ECbwS0PupoyKT4UFLMCJ-CDoRbGWUm9E9m/s1600/2018-12-06+16_24_58-Oracle+Applications+Home+Page+-+Internet+Explorer.png" imageanchor="1" style="margin-bottom: 1em; margin-right: 1em;"><img border="0" data-original-height="135" data-original-width="270" height="160" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg2vvgp7ThistDg3pNSXdLTwZjBlETd6TndcNUPUEDcEByG3KfJfhOPMhXYxPMmrcBKBfxouv5eBouNtyOogIC5hk4imrkoCxN5uu_QxBIJo0ECbwS0PupoyKT4UFLMCJ-CDoRbGWUm9E9m/s320/2018-12-06+16_24_58-Oracle+Applications+Home+Page+-+Internet+Explorer.png" width="320" /></a>
<br />
<br />
Click Search and then search for your service. In this case, we'll search for HR SOA Web Service as Internal Name.
<br />
<br />
<div class="separator" style="clear: both; text-align: left;"><a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgP81udEpYtq-0aOwOzdULOhXeBO96queDjo9VautrRoh4rZ2Kq9jz_ZvedXRXGTEOZfu0qfyQHV5y51DyhnO_NCXF03u8Ovvtf-XWaikf25dzPZVuQ_cnk750eNol_JQh2UC_1ADgL6fig/s1600/2018-12-21+11_09_21-PLSQL+Interface+_+HR+SOA+Web+Service+-+Internet+Explorer.png" imageanchor="1" style="margin-bottom: 1em; margin-right: 1em;"><img border="0" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgP81udEpYtq-0aOwOzdULOhXeBO96queDjo9VautrRoh4rZ2Kq9jz_ZvedXRXGTEOZfu0qfyQHV5y51DyhnO_NCXF03u8Ovvtf-XWaikf25dzPZVuQ_cnk750eNol_JQh2UC_1ADgL6fig/s320/2018-12-21+11_09_21-PLSQL+Interface+_+HR+SOA+Web+Service+-+Internet+Explorer.png" width="320" height="147" data-original-width="1475" data-original-height="679" alt="HR SOA Web Service image"/></a></div>
<br />
Notice here that for PL/SQL Interface Type, you have both SOAP and REST web service available. In this post, I will be focusing on the REST web service. Before we go into that, let's click on the <b>Get_Employees</b> item below to see more information.
<br />
<br />
<div class="separator" style="clear: both; text-align: left;"><a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjOf7Vbd-E6-HIJa2eLWqWSFse9tl7fFH-8HrzZwr0lxewdH4gSqJccJAN7oHkLSYqVpBf8v7mxRGamqsTdFyZQl08IbDYsH5oER0wj9Nn1gkMi-RxtimsRntyNiAJYxDQE5SUR_F-uBY0W/s1600/2018-12-21+11_10_08-PLSQL+Method+Details+_+Get+Employees+-+Internet+Explorer.png" imageanchor="1" style="margin-bottom: 1em; margin-right: 1em;"><img border="0" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjOf7Vbd-E6-HIJa2eLWqWSFse9tl7fFH-8HrzZwr0lxewdH4gSqJccJAN7oHkLSYqVpBf8v7mxRGamqsTdFyZQl08IbDYsH5oER0wj9Nn1gkMi-RxtimsRntyNiAJYxDQE5SUR_F-uBY0W/s320/2018-12-21+11_10_08-PLSQL+Method+Details+_+Get+Employees+-+Internet+Explorer.png" width="320" height="140" data-original-width="1474" data-original-height="647" /></a></div>
<br />
Moving on, click on the REST Web Service tab. In here, we will need to set an alias for this service which will be used as part of the URL when invoking it. I will enter <b>hr</b> in this case. Then click Deploy.
<br />
<br />
<div class="separator" style="clear: both; text-align: left;"><a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg-ZZ6iW8oDVtYA26kany9H_Ul_nioYJ_04kosTxbgKRvy1vvtiy23hD0qkFjREu7GW5VJh8UU2gfbDykNaoezx6hFZgp6bqiFhdSpzF9IHrHF_nzMhymxztczz7PWqgOeyNhVdw3M31x6V/s1600/2018-12-21+11_45_45-PLSQL+Interface+_+HR+SOA+Web+Service+-+Internet+Explorer.png" imageanchor="1" style="margin-bottom: 1em; margin-right: 1em;"><img border="0" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg-ZZ6iW8oDVtYA26kany9H_Ul_nioYJ_04kosTxbgKRvy1vvtiy23hD0qkFjREu7GW5VJh8UU2gfbDykNaoezx6hFZgp6bqiFhdSpzF9IHrHF_nzMhymxztczz7PWqgOeyNhVdw3M31x6V/s320/2018-12-21+11_45_45-PLSQL+Interface+_+HR+SOA+Web+Service+-+Internet+Explorer.png" width="320" height="167" data-original-width="1476" data-original-height="769" alt="Sample screen shot of REST Service Tab" /></a></div>
<br />
Click on the View WADL link to get more information on how to invoke the service. This will show you an XML description of the service.
<pre><code class="xml hljs">&lt;?xml version = &apos;1.0&apos; encoding = &apos;UTF-8&apos;?&gt;
&lt;application name=&quot;XXHR_SOA_PKG&quot; targetNamespace=&quot;http://xmlns.oracle.com/apps/per/soaprovider/plsql/rest/hr/&quot; xmlns:tns=&quot;http://xmlns.oracle.com/apps/per/soaprovider/plsql/rest/hr/&quot; xmlns=&quot;http://wadl.dev.java.net/2009/02&quot; xmlns:xsd=&quot;http://www.w3.org/2001/XMLSchema&quot; xmlns:tns1=&quot;http://xmlns.oracle.com/apps/per/rest/hr/get_employees/&quot;&gt;
   &lt;grammars&gt;
      &lt;include href=&quot;http://host:port/webservices/rest/hr/?XSD=GET_EMPLOYEES_SYNCH_TYPEDEF.xsd&quot; xmlns=&quot;http://www.w3.org/2001/XMLSchema&quot;/&gt;
   &lt;/grammars&gt;
   &lt;resources base=&quot;http://host:port/webservices/rest/hr/&quot;&gt;
      &lt;resource path=&quot;get_employees/&quot;&gt;
         &lt;method id=&quot;GET_EMPLOYEES&quot; name=&quot;POST&quot;&gt;
            &lt;request&gt;
               &lt;representation mediaType=&quot;application/xml&quot; type=&quot;GET_EMPLOYEES_Input&quot;/&gt;
               &lt;representation mediaType=&quot;application/json&quot; type=&quot;GET_EMPLOYEES_Input&quot;/&gt;
            &lt;/request&gt;
            &lt;response&gt;
               &lt;representation mediaType=&quot;application/xml&quot; type=&quot;GET_EMPLOYEES_Output&quot;/&gt;
               &lt;representation mediaType=&quot;application/json&quot; type=&quot;GET_EMPLOYEES_Output&quot;/&gt;
            &lt;/response&gt;
         &lt;/method&gt;
      &lt;/resource&gt;
   &lt;/resources&gt;
&lt;/application&gt;</code></pre>
Get the URL for the <b>xsd</b> and open it on a different browser tab/window for additional information. In this case, the URL is
<pre><code>http://host:port/webservices/rest/hr/?XSD=GET_EMPLOYEES_SYNCH_TYPEDEF.xsd</code></pre>
<pre><code class="xml hljs">&lt;?xml version = &apos;1.0&apos; encoding = &apos;UTF-8&apos;?&gt;
&lt;schema targetNamespace=&quot;http://xmlns.oracle.com/apps/per/rest/hr/get_employees/&quot; xmlns=&quot;http://www.w3.org/2001/XMLSchema&quot; xmlns:db=&quot;http://xmlns.oracle.com/apps/per/rest/hr/get_employees/&quot; elementFormDefault=&quot;qualified&quot; xmlns:head=&quot;http://xmlns.oracle.com/apps/fnd/rest/header&quot; xmlns:isgf=&quot;http://xmlns.oracle.com/isg/servicefault&quot;&gt;
   &lt;import namespace=&quot;http://xmlns.oracle.com/isg/servicefault&quot; schemaLocation=&quot;http://host:port/webservices/rest/hr/?XSD=ISGServiceFault.xsd&quot;/&gt;
   &lt;import namespace=&quot;http://xmlns.oracle.com/apps/fnd/rest/header&quot; schemaLocation=&quot;http://host:port/webservices/rest/hr/?XSD=RESTHeader.xsd&quot;/&gt;
   &lt;element name=&quot;InputParameters&quot;&gt;
      &lt;complexType&gt;
         &lt;sequence&gt;
            &lt;element name=&quot;P_EMPLOYEE_NUMBER&quot; type=&quot;string&quot; db:index=&quot;1&quot; db:type=&quot;VARCHAR2&quot; minOccurs=&quot;0&quot; nillable=&quot;true&quot;/&gt;
         &lt;/sequence&gt;
      &lt;/complexType&gt;
   &lt;/element&gt;
   &lt;element name=&quot;OutputParameters&quot;&gt;
      &lt;complexType&gt;
         &lt;sequence&gt;
            &lt;element name=&quot;XXHR_SOA_PKG-24GET_EMPLOYEES&quot; type=&quot;db:APPS.XXHR_SOA_PKG_PERX3501705X1X1&quot; db:index=&quot;0&quot; db:type=&quot;Array&quot; minOccurs=&quot;0&quot; nillable=&quot;true&quot;/&gt;
         &lt;/sequence&gt;
      &lt;/complexType&gt;
   &lt;/element&gt;
   &lt;complexType name=&quot;APPS.XXHR_SOA_PKG_PERX3501705X1X2&quot;&gt;
      &lt;sequence&gt;
         &lt;element name=&quot;PERSON_ID&quot; type=&quot;decimal&quot; db:type=&quot;NUMBER&quot; minOccurs=&quot;0&quot; nillable=&quot;true&quot;/&gt;
         &lt;element name=&quot;EFFECTIVE_START_DATE&quot; type=&quot;dateTime&quot; db:type=&quot;DATE&quot; minOccurs=&quot;0&quot; nillable=&quot;true&quot;/&gt;
         &lt;element name=&quot;EFFECTIVE_END_DATE&quot; type=&quot;dateTime&quot; db:type=&quot;DATE&quot; minOccurs=&quot;0&quot; nillable=&quot;true&quot;/&gt;
         &lt;element name=&quot;EMPLOYEE_NUMBER&quot; db:type=&quot;VARCHAR2&quot; minOccurs=&quot;0&quot; nillable=&quot;true&quot;&gt;
            &lt;simpleType&gt;
               &lt;restriction base=&quot;string&quot;&gt;
                  &lt;maxLength value=&quot;30&quot;/&gt;
               &lt;/restriction&gt;
            &lt;/simpleType&gt;
         &lt;/element&gt;
         &lt;element name=&quot;MARITAL_STATUS&quot; db:type=&quot;VARCHAR2&quot; minOccurs=&quot;0&quot; nillable=&quot;true&quot;&gt;
            &lt;simpleType&gt;
               &lt;restriction base=&quot;string&quot;&gt;
                  &lt;maxLength value=&quot;30&quot;/&gt;
               &lt;/restriction&gt;
            &lt;/simpleType&gt;
         &lt;/element&gt;
         &lt;element name=&quot;DATE_OF_BIRTH&quot; type=&quot;dateTime&quot; db:type=&quot;DATE&quot; minOccurs=&quot;0&quot; nillable=&quot;true&quot;/&gt;
      &lt;/sequence&gt;
   &lt;/complexType&gt;
   &lt;complexType name=&quot;APPS.XXHR_SOA_PKG_PERX3501705X1X1&quot;&gt;
      &lt;sequence&gt;
         &lt;element name=&quot;XXHR_SOA_PKG-24GET_EMPLOYEES_ITEM&quot; type=&quot;db:APPS.XXHR_SOA_PKG_PERX3501705X1X2&quot; db:type=&quot;Struct&quot; minOccurs=&quot;0&quot; maxOccurs=&quot;unbounded&quot; nillable=&quot;true&quot;/&gt;
      &lt;/sequence&gt;
   &lt;/complexType&gt;
&lt;element name=&quot;GET_EMPLOYEES_Input&quot;&gt;
      &lt;complexType&gt;
         &lt;sequence&gt;
            &lt;element ref=&quot;head:RESTHeader&quot;/&gt;
            &lt;element ref=&quot;db:InputParameters&quot;/&gt;
         &lt;/sequence&gt;
      &lt;/complexType&gt;
   &lt;/element&gt;
   &lt;element name=&quot;GET_EMPLOYEES_Output&quot;&gt;
      &lt;complexType&gt;
         &lt;choice&gt;
            &lt;element ref=&quot;db:OutputParameters&quot;/&gt;
            &lt;element ref=&quot;isgf:ISGServiceFault&quot;/&gt;
         &lt;/choice&gt;
      &lt;/complexType&gt;
   &lt;/element&gt;
&lt;/schema&gt;</code></pre>
<br />
Let's not forget to setup the <b>grants</b>. In my case, I granted it to SYSADMIN user.
<div class="separator" style="clear: both; text-align: left;"><a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhPRnLXGBg5wI6_6YG3Cs-LMFSphx82-mcYPpttNpYQVn7DUSCK0g03zylxqvIzNMa5NXDdzerSYup_2nEOEh0EvxfBJzkuS9T5S6iy8EmgckglHKeJowbNuZccap-j0wQzikqKKCG8bdir/s1600/2018-12-21+12_03_08-PLSQL+Interface+_+HR+SOA+Web+Service+-+Internet+Explorer.png" imageanchor="1" style="margin-bottom: 1em; margin-right: 1em;"><img border="0" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhPRnLXGBg5wI6_6YG3Cs-LMFSphx82-mcYPpttNpYQVn7DUSCK0g03zylxqvIzNMa5NXDdzerSYup_2nEOEh0EvxfBJzkuS9T5S6iy8EmgckglHKeJowbNuZccap-j0wQzikqKKCG8bdir/s320/2018-12-21+12_03_08-PLSQL+Interface+_+HR+SOA+Web+Service+-+Internet+Explorer.png" width="320" height="124" data-original-width="1473" data-original-height="570" alt="Sample screen shot grants setup" /></a></div>
<br />
Then of course we need to bounce the following services
<br />
<br />
opmn
<br />
oafm
<br />
<br />
Forgot to mention that PL/SQL Interface Type is always invoke using POST method. And since this is the case, we need to build the request body. We will also use Basic Authentication as our authorization method.
<br />
<br />
So to put it all together, the URL will be:
<pre><code>http://host:port/webservices/rest/hr/get_employees/</code></pre>
The request body will be:
<pre><code>{
  "GET_EMPLOYEES_Input": {
    "@xmlns": "http://xmlns.oracle.com/apps/per/rest/hr/get_employees/",
    "RESTHeader": {
      "@xmlns": "http://xmlns.oracle.com/apps/fnd/rest/header",
      "Responsibility": "SYSTEM_ADMINISTRATOR",
      "RespApplication": "SYSADMIN",
      "SecurityGroup": "STANDARD",
      "NLSLanguage": "AMERICAN"
    },
    "InputParameters": { "P_EMPLOYEE_NUMBER": "123456" }
  }
}</code></pre>
Here's a sample screen shot invoking the service using Postman and getting a response.
<br />
<br />
<div class="separator" style="clear: both; text-align: left;"><a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhkEUg-KA0YvreqrfT5q-5cGefkjCxvSHIVmG_Gr5hTTLZcLFKcrhfAn5SctO-CZVomr8RkpU8Vbx_0iAIHkGRudYCZ-xWUXa_-qAX94_sEjej7nVSiMaB4w-4o9ASBk82BQVzeMWPtKv-o/s1600/2018-12-21+12_27_00-Postman.png" imageanchor="1" style="margin-bottom: 1em; margin-right: 1em;"><img border="0" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhkEUg-KA0YvreqrfT5q-5cGefkjCxvSHIVmG_Gr5hTTLZcLFKcrhfAn5SctO-CZVomr8RkpU8Vbx_0iAIHkGRudYCZ-xWUXa_-qAX94_sEjej7nVSiMaB4w-4o9ASBk82BQVzeMWPtKv-o/s320/2018-12-21+12_27_00-Postman.png" width="320" height="153" data-original-width="1600" data-original-height="766" alt="Sample screen shot using Postman" /></a></div>
<br />
Some pointers that I noticed. Input Parameters seems to be optional. So if you wanted to pass a null value for <b>P_EMPLOYEE_NUMBER</b>, you can do either of the following:
<pre><code>"InputParameters": { "P_EMPLOYEE_NUMBER": "" }</code></pre>
or just simply not include it in the body.
<pre><code>"InputParameters": { }</code></pre>
And that's it. Enjoy!
