---
layout: post
title: "Custom Interface Java Bean Service to return \"Hello World\" using Oracle E-Business Suite Integrated SOA Gateway"
date: 2018-12-06 05:08:00 +0000
categories: ["Programming"]
---

We recently upgraded our Oracle E-Business Suite and along with it was the Integrated SOA Gateway. At work, we usually have requirements for systems integration in which we uses flat files. However, since we already have the Integrated SOA Gateway, we decided to give it a try.
<br />
<br />
The Integrated SOA Gateway supported a few custom interface types. In this post, I will be working on the Java Bean Service. To test this, I’m going to build a very simple custom interface that returns a string “Hello World”. Later on, I can probably try something more complex. But let's stick with this for now.
<br />
<br />
Before we proceed, let me give you a bit of information about our instance.
<br />
<br />
Oracle E-Business Suite Release 12.1.3
<br />
Java 1.6.0_21 
<br />
JDBC Driver 11.2.0.1.0 
<br />
Database Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production 
<br />
Operating System Linux 3.10.0-229.20.1.el7.x86_64 
<br />
<br />
Without further ado, here's my Java code.
<br />
<br />
HelloWorld.java
<br />
<pre><code class="java">package oracle.apps.per.sample.service;

/**
 * A sample class to demonstrate how Java API can use the ISG REST framework. This class provides
 * methods to return string Hello World
 * @rep:scope public
 * @rep:product PER
 * @rep:displayname Hello World
 * @rep:category BUSINESS_ENTITY HR_USER_HOOK
 * @rep:category IREP_CLASS_SUBTYPE JAVA_BEAN_SERVICES
 */
public class HelloWorld {

    public HelloWorld() {
    }
    
    /**
     * 
     * This method returns a list of direct reports of the requesting user.
     *
     * @return Hello World string
     * @rep:scope public
     * @rep:displayname Get Hello World
     * @rep:httpverb get
     */
    public String getHelloWorld() {
        return "Hello World";
    }
        
}
</code>
</pre>
Take note of the annotations on class level and method level as some of these are required. More information on annotations <a href="https://docs.oracle.com/cd/E18727_01/doc.121/e12065/T511473T545912.htm#sigdg_appb_servicebean" target="_blank">here</a>.
<br />
<br />
Next is we need to transfer this java file to the appropriate folder in the server. In my case, I will transfer this to $JAVA_TOP/oracle/apps/per/sample/service.
<br />
<br />
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgYQuKaRlwL2W29tyPIgx45scwYwsYDcCeRaxBdJ400OezxrK9Am2H3ys06KslAVfLA22J7nZUxfujRF2bH1dx4lBBffp8RXR_zdRhtiTE5wn4IxAKlPJIixRM0mm741hJnhh6ZZ0t4m-HD/s1600/2018-12-06+10_40_06-service+-+las%2540lvdverp.adb.org+-+WinSCP.png" imageanchor="1" style="margin-bottom: 1em; margin-right: 1em;"><img border="0" data-original-height="113" data-original-width="450" height="80" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgYQuKaRlwL2W29tyPIgx45scwYwsYDcCeRaxBdJ400OezxrK9Am2H3ys06KslAVfLA22J7nZUxfujRF2bH1dx4lBBffp8RXR_zdRhtiTE5wn4IxAKlPJIixRM0mm741hJnhh6ZZ0t4m-HD/s320/2018-12-06+10_40_06-service+-+las%2540lvdverp.adb.org+-+WinSCP.png" width="320" /></a>
<br />
<br />
Now we need to connect to the server via ssh. Set JAVA_HOME to $IAS_ORACLE_HOME/appsutil/jdk if not already set. Lastly, make sure you have the appropriate permissions to execute the following commands:
<br />
<br />
Compile the java file using the javac command.
<pre><code class="unix">javac $JAVA_TOP/oracle/apps/per/sample/service/HelloWorld.java</code></pre>
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiEmS8uT2FtkPoPKKsWqj2i5_C75Flo5jX19xmym0CgxCswuZSO2EBgPiyoLELLrV8JsnFwBJJDT-uGvGGsj_pPYCxWsdQabPmgyPkIh7_7n4ntfzNI8u1Vmt5kbDQYqR2CJLILm60Fza20/s1600/2018-12-10+11_32_06-Blogger_+Allen+Sandiego+-+Edit+post.png" imageanchor="1" style="margin-bottom: 1em; margin-right: 1em;"><img border="0" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiEmS8uT2FtkPoPKKsWqj2i5_C75Flo5jX19xmym0CgxCswuZSO2EBgPiyoLELLrV8JsnFwBJJDT-uGvGGsj_pPYCxWsdQabPmgyPkIh7_7n4ntfzNI8u1Vmt5kbDQYqR2CJLILm60Fza20/s320/2018-12-10+11_32_06-Blogger_+Allen+Sandiego+-+Edit+post.png" width="320" height="28" data-original-width="1034" data-original-height="92" /></a>
<br />
<br />
Generate the iLDT file using the irep_parser.pl script.
<pre><code class="shell">
[las@server ~]$ $IAS_ORACLE_HOME/perl/bin/perl $FND_TOP/bin/irep_parser.pl -g -v -username=sysadmin per:patch/115/java:HelloWorld.java:12.1=$JAVA_TOP/oracle/apps/per/sample/service/HelloWorld.java
</code></pre>
If successful, you should see something similar to the screen shot below.
<br />
<br />
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj1G6DwgvDAcLFkys5IO-tsJJh78nG4zA6iIseQX4xkT6aZLjvAAOUkUWQ8mtjZ16l0-KF3Ohoe9osUsFjbPD1zI4zjbpH0PBXNIy7Badi5RHQlul9djE6Ws5T6qwwTOpdNP67zol-_2ua6/s1600/2018-12-06+10_52_53-las%2540lvdverp_%257E.png" imageanchor="1" style="margin-bottom: 1em; margin-right: 1em;"><img border="0" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj1G6DwgvDAcLFkys5IO-tsJJh78nG4zA6iIseQX4xkT6aZLjvAAOUkUWQ8mtjZ16l0-KF3Ohoe9osUsFjbPD1zI4zjbpH0PBXNIy7Badi5RHQlul9djE6Ws5T6qwwTOpdNP67zol-_2ua6/s320/2018-12-06+10_52_53-las%2540lvdverp_%257E.png" width="320" height="117" data-original-width="1035" data-original-height="379" /></a>
<br />
You should also notice that an Integration Repository loader file (ildt) was created as shown above. This file will contain the metadata for the Java Bean Service object.
<br />
<br />
<b>IMPORTANT NOTE:</b> For succeeding changes, you need to increment the version when running the irep_parser.pl script. In the example above, I used version 12.1. If I need to make changes to the Java code, I will again need to generated the iLDT file but in doing so, I need to use version 12.2 or 12.1.1 for the changes to take effect.
<br />
<br />
Moving on, here's the content of the iLDT file for reference.
<br />
<br />
HelloWorld_java.ildt
<br />
<pre><code>
# $Header: HelloWorld_java.ildt 12.1 2018/12/04 13:54:23 sysadmin  $

# dbdrv: sql fnd patch/115/sql afdlfsub.sql none none none sqlplus \
# dbdrv:  &amp;phase=dat+10 checkfile:~PROD:~PATH:~FILE &amp;un_apps &amp;pw_apps per \
# dbdrv:  patch/115/java HelloWorld_java.ildt 12.1 FND FNDDLFLD NORMAL \
# dbdrv:  ARU2ILDT 50
LANGUAGE = "US"
LDRCONFIG = "wfirep.lct 120.7"

# Generated by the IRep Parser, 12.0.0

# -- Begin Entity Definitions --

DEFINE IREP_OBJECT
  KEY   OBJECT_NAME              VARCHAR2(430)
  KEY   DEST_TABLE               VARCHAR2(1)
  CTX   OWNER                    VARCHAR2(4000)
  BASE  API_NAME                 VARCHAR2(400)
  BASE  OBJ_TYPE                 VARCHAR2(30)
  BASE  SERVICEABLE              VARCHAR2(1)
  BASE  CLASS_RESOURCE_PATH      VARCHAR2(240)
  BASE  PRODUCT                  VARCHAR2(8)
  BASE  IMP_NAME                 VARCHAR2(400)
  BASE  COMPATABILITY            VARCHAR2(1)
  BASE  SCOPE                    VARCHAR2(30)
  BASE  LIFECYCLE                VARCHAR2(30)
  BASE  SOURCE_FILE_PRODUCT      VARCHAR2(8)
  BASE  SOURCE_FILE_PATH         VARCHAR2(100)
  BASE  SOURCE_FILE_NAME         VARCHAR2(36)
  BASE  SOURCE_FILE_VERSION      VARCHAR2(150)
  BASE  DESCRIPTION              VARCHAR2(32000)
  BASE  STANDARD                 VARCHAR2(30)
  BASE  STANDARD_VERSION         VARCHAR2(30)
  BASE  STANDARD_SPEC            VARCHAR2(240)
  TRANS DISPNAME                 VARCHAR2(240)
  TRANS SHORTDISC                VARCHAR2(2000)
  BASE  TIMESTAMP                VARCHAR2(11)
  BASE  OI_FLAG                  VARCHAR2(1)
  BASE  MAPCODE                  VARCHAR2(255)
  BASE  PARSER_VERSION           VARCHAR2(80)
  BASE  SDO_DEF_CLASS            VARCHAR2(400)
  BASE  SDO_CLASS_NAME           VARCHAR2(400)
  BASE  SDO_IS_FILTER            VARCHAR2(1)
  BASE  SDO_FILTER_REQUIRED      VARCHAR2(1)
  BASE  SDO_IS_EXPRESSION        VARCHAR2(1)
  BASE  SB_INTERFACE_CLASS       VARCHAR2(400)
  BASE  CRAWL_CRAWLABLE          VARCHAR2(1)
  BASE  CRAWL_VISIBILITY_LEVEL   VARCHAR2(8)
  BASE  CRAWL_SEARCH_PLUGIN      VARCHAR2(4000)
  BASE  CRAWL_UI_FUNCTION        VARCHAR2(240)
  BASE  CRAWL_CHANGE_EVENT_NAME  VARCHAR2(4000)
  BASE  CRAWL_CHANGE_NTF         VARCHAR2(1)
  BASE  CRAWL_DRIVING_TABLE      VARCHAR2(30)
  BASE  CRAWL_OBJ_ATTR_0         VARCHAR2(2000)
  BASE  CRAWL_OBJ_ATTR_1         VARCHAR2(2000)
  BASE  CRAWL_OBJ_ATTR_2         VARCHAR2(2000)
  BASE  CRAWL_OBJ_ATTR_3         VARCHAR2(2000)
  BASE  CRAWL_OBJ_ATTR_4         VARCHAR2(2000)
  BASE  CRAWL_OBJ_ATTR_5         VARCHAR2(2000)
  BASE  CRAWL_OBJ_ATTR_6         VARCHAR2(2000)
  BASE  CRAWL_OBJ_ATTR_7         VARCHAR2(2000)
  BASE  CRAWL_OBJ_ATTR_8         VARCHAR2(2000)
  BASE  CRAWL_OBJ_ATTR_9         VARCHAR2(2000)

  DEFINE PARENTS
    KEY   PARENT_NAME            VARCHAR2(430)
  END PARENTS

  DEFINE OBJECT_CATEGORY
    KEY   TYPE                   VARCHAR2(30)
    KEY   CODE                   VARCHAR2(30)
    BASE  SEQUENCE               VARCHAR2(50)
  END OBJECT_CATEGORY

  DEFINE OBJECT_FLEXFIELD
    KEY   APPL_SHORT_CODE        VARCHAR2(50)
    KEY   FLEX_TYPE              VARCHAR2(20)
    KEY   FLEX_CODE              VARCHAR2(400)
    BASE  CONTEXT                VARCHAR2(4000)
    BASE  SEGMENT                VARCHAR2(4000)
    BASE  COMPLEX_TYPE           VARCHAR2(4000)
  END OBJECT_FLEXFIELD

  DEFINE OBJ_CHILD_ANNOTATIONS
    KEY   CHILD_FLAG             VARCHAR2(1)
    KEY   VALUE                  VARCHAR2(500)
  END OBJ_CHILD_ANNOTATIONS

  DEFINE TYPE_MEMBERS
    KEY   SEQUENCE               VARCHAR2(50)
    KEY   INNERTYPE_SEQUENCE     VARCHAR2(50)
    BASE  MEMBER_NAME            VARCHAR2(240)
    BASE  TYPE                   VARCHAR2(430)
    BASE  PRECISION              VARCHAR2(50)
    BASE  SIZE                   VARCHAR2(50)
    BASE  SCALE                  VARCHAR2(50)
    BASE  NULL_ALLOWED           VARCHAR2(1)
    BASE  DESCRIPTION            VARCHAR2(4000)
    BASE  ATTR_SET               VARCHAR2(240)
    BASE  PRIMARY_KEY            VARCHAR2(1)
    BASE  TRANSLATABLE           VARCHAR2(1)
    BASE  COMPOSITE              VARCHAR2(1)
    BASE  DOMAIN_NAME            VARCHAR2(240)
    BASE  MEMBER_TYPE_NAME       VARCHAR2(240)
    BASE  SEARCH_CRITERIA_TYPE   VARCHAR2(30)
    BASE  ATTACHMENT             VARCHAR2(1)
    BASE  MIME_TYPE              VARCHAR2(120)
    BASE  DOMAIN_IMPLEMENTATION  VARCHAR2(400)
    BASE  IS_SORTABLE            VARCHAR2(1)
    BASE  CRAWL_IS_DATE_BASED    VARCHAR2(1)
    BASE  CRAWL_MEMBER_VIS_LVL   VARCHAR2(8)
    BASE  CRAWL_IS_DISPLAYED     VARCHAR2(1)
    BASE  CRAWL_UI_FPARAM_NAME   VARCHAR2(240)
    BASE  CRAWL_INDEXED          VARCHAR2(1)
    BASE  CRAWL_STORED           VARCHAR2(1)
    BASE  CRAWL_IS_SECURE        VARCHAR2(1)
    BASE  CRAWL_IS_TITLE         VARCHAR2(1)
    BASE  CRAWL_WEIGHT           VARCHAR2(50)
    BASE  CRAWL_MBR_ATTR_0       VARCHAR2(2000)
    BASE  CRAWL_MBR_ATTR_1       VARCHAR2(2000)
    BASE  CRAWL_MBR_ATTR_2       VARCHAR2(2000)
    BASE  CRAWL_MBR_ATTR_3       VARCHAR2(2000)
    BASE  CRAWL_MBR_ATTR_4       VARCHAR2(2000)
    BASE  CRAWL_MBR_ATTR_5       VARCHAR2(2000)
    BASE  CRAWL_MBR_ATTR_6       VARCHAR2(2000)
    BASE  CRAWL_MBR_ATTR_7       VARCHAR2(2000)
    BASE  CRAWL_MBR_ATTR_8       VARCHAR2(2000)
    BASE  CRAWL_MBR_ATTR_9       VARCHAR2(2000)
  END TYPE_MEMBERS

  DEFINE USES_TABLE
    KEY   TABLE_NAME             VARCHAR2(30)
    BASE  UT_SEQ                 VARCHAR2(50)
    BASE  UT_DIRECTION           VARCHAR2(1)
  END USES_TABLE

  DEFINE USES_MAP
    KEY   MAP_NAME               VARCHAR2(30)
    BASE  UM_SEQ                 VARCHAR2(50)
  END USES_MAP

  DEFINE CLASS_DATASOURCES
    KEY   DATASOURCE_NAME        VARCHAR2(120)
    BASE  DEF_CLASS              VARCHAR2(400)
    BASE  QUERYABLE              VARCHAR2(1)
    BASE  UPDATEABLE             VARCHAR2(1)
    BASE  INSERTABLE             VARCHAR2(1)
    BASE  MERGEABLE              VARCHAR2(1)
    BASE  DELETEABLE             VARCHAR2(1)
    BASE  PROCESS_QNAME          VARCHAR2(120)
    BASE  QUERY_QNAME            VARCHAR2(120)
  END CLASS_DATASOURCES

  DEFINE OBJ_KEY_SET
    KEY   KEY_SET_NAME           VARCHAR2(120)
    KEY   KEY_SET_SEQUENCE       VARCHAR2(50)
    BASE  KEY1_MBR_NAME          VARCHAR2(240)
    BASE  KEY2_MBR_NAME          VARCHAR2(240)
    BASE  KEY3_MBR_NAME          VARCHAR2(240)
    BASE  KEY4_MBR_NAME          VARCHAR2(240)
    BASE  KEY5_MBR_NAME          VARCHAR2(240)
    BASE  ALT1_MBR_NAME          VARCHAR2(240)
    BASE  ALT2_MBR_NAME          VARCHAR2(240)
    BASE  ALT3_MBR_NAME          VARCHAR2(240)
    BASE  ALT4_MBR_NAME          VARCHAR2(240)
    BASE  ALT5_MBR_NAME          VARCHAR2(240)
  END OBJ_KEY_SET

  DEFINE IREP_RECORD
    KEY   COMPLEX_TYPE_NAME      VARCHAR2(480)
    BASE  RECORD_NAME            VARCHAR2(80)
    BASE  SCOPE                  VARCHAR2(30)
    BASE  TYPE                   VARCHAR2(20)
    BASE  LIFECYCLE              VARCHAR2(30)
    BASE  DESCRIPTION            VARCHAR2(32000)
    TRANS USER_RECORD_NAME       VARCHAR2(80)
    TRANS SHORT_DESCRIPTION      VARCHAR2(240)

    DEFINE FIELDS
      KEY   SEQUENCE             VARCHAR2(50)
      KEY   INNERTYPE_SEQUENCE   VARCHAR2(50)
      BASE  NAME                 VARCHAR2(240)
      BASE  DIRECTION            VARCHAR2(1)
      BASE  OPTIONAL             VARCHAR2(1)
      BASE  TYPE                 VARCHAR2(430)
      BASE  PRECISION            VARCHAR2(50)
      BASE  SIZE                 VARCHAR2(50)
      BASE  SCALE                VARCHAR2(50)
      BASE  NULL_ALLOWED         VARCHAR2(1)
      BASE  DESCRIPTION          VARCHAR2(4000)
      BASE  DEFAULT_VALUE        VARCHAR2(255)
      BASE  DISPLAYED            VARCHAR2(1)
      BASE  ATTRIBUTE_SET        VARCHAR2(240)
      BASE  KEY_PARAM            VARCHAR2(1)
    END FIELDS

  END IREP_RECORD

  DEFINE IREP_METHOD
    KEY   FUNCTION_NAME          VARCHAR2(480)
    BASE  METHOD_NAME            VARCHAR2(80)
    KEY   OVERLOAD_SEQ           VARCHAR2(50)
    BASE  SCOPE                  VARCHAR2(30)
    BASE  INPUT_XSD_FILE         VARCHAR2(400)
    BASE  INPUT_ROOT_ELEMENT     VARCHAR2(400)
    BASE  OUTPUT_XSD_FILE        VARCHAR2(400)
    BASE  OUTPUT_ROOT_ELEMENT    VARCHAR2(400)
    BASE  STATIC                 VARCHAR2(1)
    BASE  EXCEPTIONLIST          VARCHAR2(4000)
    BASE  HTTP_VERB              VARCHAR2(50)
    BASE  METHOD_RESOURCE_PATH   VARCHAR2(240)
    BASE  LIFECYCLE              VARCHAR2(30)
    BASE  DESCRIPTION            VARCHAR2(32000)
    BASE  COMPATABILITY          VARCHAR2(1)
    BASE  SYNCHRO                VARCHAR2(1)
    BASE  DIRECTION              VARCHAR2(1)
    BASE  CTX_DEPENDENCE         VARCHAR2(8)
    TRANS USER_FN_NAME           VARCHAR2(80)
    TRANS SHORT_DESCRIPTION      VARCHAR2(240)
    BASE  PRIMARY_FLAG           VARCHAR2(1)
    BASE  INDIRECT_OP_FLAG       VARCHAR2(1)

    DEFINE METHOD_CATEGORY
      KEY   TYPE                 VARCHAR2(30)
      KEY   CODE                 VARCHAR2(30)
      BASE  SEQUENCE             VARCHAR2(50)
    END METHOD_CATEGORY

    DEFINE METHOD_FLEXFIELD
      KEY   APPL_SHORT_CODE      VARCHAR2(50)
      KEY   FLEX_TYPE            VARCHAR2(20)
      KEY   FLEX_CODE            VARCHAR2(400)
      BASE  CONTEXT              VARCHAR2(4000)
      BASE  SEGMENT              VARCHAR2(4000)
      BASE  COMPLEX_TYPE         VARCHAR2(4000)
    END METHOD_FLEXFIELD

    DEFINE METHOD_CHILD_ANNOTATIONS
      KEY   CHILD_FLAG           VARCHAR2(1)
      KEY   VALUE                VARCHAR2(500)
    END METHOD_CHILD_ANNOTATIONS

    DEFINE PARAMS
      KEY   SEQUENCE             VARCHAR2(50)
      KEY   INNERTYPE_SEQUENCE   VARCHAR2(50)
      BASE  NAME                 VARCHAR2(240)
      BASE  DIRECTION            VARCHAR2(1)
      BASE  OPTIONAL             VARCHAR2(1)
      BASE  TYPE                 VARCHAR2(430)
      BASE  PRECISION            VARCHAR2(50)
      BASE  SIZE                 VARCHAR2(50)
      BASE  SCALE                VARCHAR2(50)
      BASE  NULL_ALLOWED         VARCHAR2(1)
      BASE  DESCRIPTION          VARCHAR2(4000)
      BASE  DEFAULT_VALUE        VARCHAR2(255)
      BASE  DISPLAYED            VARCHAR2(1)
      BASE  ATTRIBUTE_SET        VARCHAR2(240)
      BASE  KEY_PARAM            VARCHAR2(1)
    END PARAMS

  END IREP_METHOD

END IREP_OBJECT

# -- End Entity Definitions --

BEGIN IREP_OBJECT "JAVA:oracle.apps.per.sample.service.HelloWorld" "C"
  OWNER = "sysadmin"
  API_NAME = "oracle.apps.per.sample.service.HelloWorld"
  OBJ_TYPE = "JAVA"
  PRODUCT = "PER"
  SCOPE = "PUBLIC"
  SOURCE_FILE_PRODUCT = "per"
  SOURCE_FILE_PATH = "patch/115/java"
  SOURCE_FILE_NAME = "HelloWorld.java"
  SOURCE_FILE_VERSION = "12.1"
  DESCRIPTION = "A sample class to demonstrate how Java API can use the ISG REST framework. This class provides\n\
 methods to return string Hello World"
  DISPNAME = "Hello World"
  SHORTDISC = "A sample class to demonstrate how Java API can use the ISG REST framework."
  TIMESTAMP = "2018/12/04"
  OI_FLAG = "N"
  PARSER_VERSION = "12.0.0"
  SDO_IS_FILTER = "N"
  SDO_FILTER_REQUIRED = "N"
  SDO_IS_EXPRESSION = "N"
  CRAWL_CRAWLABLE = "N"
  CRAWL_CHANGE_NTF = "N"

  BEGIN OBJECT_CATEGORY "BUSINESS_ENTITY" "HR_USER_HOOK"
  END OBJECT_CATEGORY

  BEGIN OBJECT_CATEGORY "IREP_CLASS_SUBTYPE" "JAVA_BEAN_SERVICES"
  END OBJECT_CATEGORY

  BEGIN IREP_METHOD "JAVA:oracle.apps.per.sample.service.HelloWorld:getHelloWorld" "1"
    METHOD_NAME = "getHelloWorld"
    SCOPE = "PUBLIC"
    HTTP_VERB = "GET"
    DESCRIPTION = "This method returns a list of direct reports of the requesting user."
    USER_FN_NAME = "Get Hello World"
    SHORT_DESCRIPTION = "This method returns a list of direct reports of the requesting user."
    PRIMARY_FLAG = "N"
    INDIRECT_OP_FLAG = "N"

    BEGIN METHOD_CATEGORY "BUSINESS_ENTITY" "HR_USER_HOOK"
    END METHOD_CATEGORY

    BEGIN METHOD_CATEGORY "IREP_CLASS_SUBTYPE" "JAVA_BEAN_SERVICES"
    END METHOD_CATEGORY

    BEGIN PARAMS "0" "0"
      NAME = "RETURN"
      DIRECTION = "O"
      OPTIONAL = "N"
      TYPE = "java.lang.String"
      NULL_ALLOWED = "N"
      DESCRIPTION = "Hello World string"
      DISPLAYED = "Y"
    END PARAMS

  END IREP_METHOD

END IREP_OBJECT
</code></pre>
<br />
Next, we’ll be uploading this file to the Integration Repository using the following command:
<br />
<pre><code>
FNDLOAD apps/&lt;apps_password&gt; 0 Y UPLOAD $FND_TOP/patch/115/import/wfirep.lct HelloWorld_java.ildt
</apps_password></code></pre>
If successful, you will see something like this:
<br />
<br />
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiJyLrzHutiRTnzE7-xIjDnoiL0krNpNW-300ezF_YUQCN5MQx7HfMTyEOLNcxay9OIDrbliHfWSE3E3jUUgxPp3eMvTPfsh2l0i7KdoH6PhfuEDsveMJ38kdr1_OXe16E24FPKtucYk67Y/s1600/2018-12-06+16_15_00-Java+Bean+Services.docx+-+Word.png" imageanchor="1" style="margin-bottom: 1em; margin-right: 1em;"><img border="0" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiJyLrzHutiRTnzE7-xIjDnoiL0krNpNW-300ezF_YUQCN5MQx7HfMTyEOLNcxay9OIDrbliHfWSE3E3jUUgxPp3eMvTPfsh2l0i7KdoH6PhfuEDsveMJ38kdr1_OXe16E24FPKtucYk67Y/s320/2018-12-06+16_15_00-Java+Bean+Services.docx+-+Word.png" width="320" height="41" data-original-width="1030" data-original-height="131" /></a>
<br />
<br />
Let’s check the log file to make sure there were no errors during the upload.
<br />
<br />
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjyXk55-PZ6C5JYO__qBBMm0S7E2yuqlxRBaedeZslDP3_w9QnnNTy7AT38SrfPV-E0M3jVcHODtDu-2Aw4QUo6gqRscwCkOAZCiTqIf3XTvyZxbl8mFCHfjiUma4PWkR_BRQB8FgYbgGLC/s1600/2018-12-06+16_17_14-Java+Bean+Services.docx+-+Word.png" imageanchor="1" style="margin-bottom: 1em; margin-right: 1em;"><img border="0" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjyXk55-PZ6C5JYO__qBBMm0S7E2yuqlxRBaedeZslDP3_w9QnnNTy7AT38SrfPV-E0M3jVcHODtDu-2Aw4QUo6gqRscwCkOAZCiTqIf3XTvyZxbl8mFCHfjiUma4PWkR_BRQB8FgYbgGLC/s320/2018-12-06+16_17_14-Java+Bean+Services.docx+-+Word.png" width="320" height="132" data-original-width="1034" data-original-height="426" /></a>
<br />
<br />
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
Click Search
<br />
<br />
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjswE8vz6ht9aYZB08D3Mgo6wVf4HwqaBGqIGP0nG4MJDYyPvdCDE115I6lGGf9m4k-3reYdLH5YZJAkC4V6-GIvY1Bw-F4_o_iZI9eu0cY3pLLNRRz-Vz81iPwNg9GQYNErIbGpUB8m1Dy/s1600/2018-12-06+16_25_39-Integration+Repository+-+Internet+Explorer.png" imageanchor="1" style="margin-bottom: 1em; margin-right: 1em;"><img border="0" data-original-height="243" data-original-width="1498" height="52" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjswE8vz6ht9aYZB08D3Mgo6wVf4HwqaBGqIGP0nG4MJDYyPvdCDE115I6lGGf9m4k-3reYdLH5YZJAkC4V6-GIvY1Bw-F4_o_iZI9eu0cY3pLLNRRz-Vz81iPwNg9GQYNErIbGpUB8m1Dy/s320/2018-12-06+16_25_39-Integration+Repository+-+Internet+Explorer.png" width="320" /></a>
<br />
<br />
Search for “Hello World” in the Interface Name and click Go
<br />
<br />
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhcxmBy5aIIaSn2-lbQ0kTmX8xSTHmZR2ltRqaChC90Y0DMBMnQPU9Vv48D9K7kZMYawc-3hwWsnd4AkiWZ61DcKNCzjlbYNqA-KkvcuZ6rAZQDCSTr7d8S7Y11Sy-cSRJcBkBHi7D0_Jer/s1600/2018-12-06+16_26_29-Search+Interfaces+-+Internet+Explorer.png" imageanchor="1" style="margin-bottom: 1em; margin-right: 1em;"><img border="0" data-original-height="397" data-original-width="459" height="277" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhcxmBy5aIIaSn2-lbQ0kTmX8xSTHmZR2ltRqaChC90Y0DMBMnQPU9Vv48D9K7kZMYawc-3hwWsnd4AkiWZ61DcKNCzjlbYNqA-KkvcuZ6rAZQDCSTr7d8S7Y11Sy-cSRJcBkBHi7D0_Jer/s320/2018-12-06+16_26_29-Search+Interfaces+-+Internet+Explorer.png" width="320" /></a>
<br />
<br />
Click the Name of the Interface
<br />
<br />
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjn0JB3N8ReSAOvzUUXkxnzsFGSoiDpCRdQyhkwScRynUFkmfy2E0hYkVfdjC387srmtKCikuFcVTY9fwNtn2oEdqT8UmtFAucEsPkeEf5IpZQsQkBBShUDut1zh01jqOARmxxuiCQ_UbHd/s1600/2018-12-06+16_27_29-Search+Interfaces+-+Internet+Explorer.png" imageanchor="1" style="margin-bottom: 1em; margin-right: 1em;"><img border="0" data-original-height="196" data-original-width="1492" height="42" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjn0JB3N8ReSAOvzUUXkxnzsFGSoiDpCRdQyhkwScRynUFkmfy2E0hYkVfdjC387srmtKCikuFcVTY9fwNtn2oEdqT8UmtFAucEsPkeEf5IpZQsQkBBShUDut1zh01jqOARmxxuiCQ_UbHd/s320/2018-12-06+16_27_29-Search+Interfaces+-+Internet+Explorer.png" width="320" /></a>
<br />
<br />
You should now see the details of the Interface object.
<br />
<br />
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi7ic3d6RJNxULsDIk4HCsiWuL8E1ISWktdjBATGYtU-gHPxZcAKbuQXfim0ZcgHA92k2g1YlPODs7-4B5PONxLQ_wSXJ6nWXHsebOwWLJ4fMh_W8D1sDmu7cBekuNCHyws9D78GCeqBcL6/s1600/2018-12-06+16_28_21-Java+Details+_+Hello+World+-+Internet+Explorer.png" imageanchor="1" style="margin-bottom: 1em; margin-right: 1em;"><img border="0" data-original-height="676" data-original-width="1104" height="196" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi7ic3d6RJNxULsDIk4HCsiWuL8E1ISWktdjBATGYtU-gHPxZcAKbuQXfim0ZcgHA92k2g1YlPODs7-4B5PONxLQ_wSXJ6nWXHsebOwWLJ4fMh_W8D1sDmu7cBekuNCHyws9D78GCeqBcL6/s320/2018-12-06+16_28_21-Java+Details+_+Hello+World+-+Internet+Explorer.png" width="320" /></a>
<br />
<br />
To enable the REST Web Service, click on the REST Web Service tab. Enter an alias for this. Then click Deploy. The alias will be used as part of the URL later on when invoking the service. Once deployed, you can get more information on the service by clicking on the WADL.
<br />
<br />
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjO6f2RNBuRtZrhcTbWCF9G9dTNGGdGutnz1_7ennUEkc0cdP-UAgcnIzNbu4cdj6FfPXlXz_OXRPPAghu0_zbTu0ZepnEDX5APXD5fLh-g87HiaJI7QlTZNjKlRyw3wiGV5nD2KjNpqEO0/s1600/2018-12-06+16_44_57-Java+Details+_+Hello+World+-+Internet+Explorer.png" imageanchor="1" style="margin-bottom: 1em; margin-right: 1em;"><img border="0" data-original-height="573" data-original-width="1048" height="175" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjO6f2RNBuRtZrhcTbWCF9G9dTNGGdGutnz1_7ennUEkc0cdP-UAgcnIzNbu4cdj6FfPXlXz_OXRPPAghu0_zbTu0ZepnEDX5APXD5fLh-g87HiaJI7QlTZNjKlRyw3wiGV5nD2KjNpqEO0/s320/2018-12-06+16_44_57-Java+Details+_+Hello+World+-+Internet+Explorer.png" width="320" /></a>
<br />
<br />
Let’s move on to the Grants tab and grant the service to some users. Check the Get Hello World and click Create Grant.
<br />
<br />
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgLrpSanuQ1qRco5UP0gcPNBVNLPPcjq2tBlK4ADpInObm71tQRejaqfCl1K1hnr8Ik8sqz3dh1EUJ4yj9UFzTfLItkV1ZRoDjZC3mVhs3NV4Jk5ONtM5sExKA0GC6Vqegi-jMN2ldB1j22/s1600/2018-12-06+16_47_23-Java+Details+_+Hello+World+-+Internet+Explorer.png" imageanchor="1" style="margin-bottom: 1em; margin-right: 1em;"><img border="0" data-original-height="303" data-original-width="463" height="209" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgLrpSanuQ1qRco5UP0gcPNBVNLPPcjq2tBlK4ADpInObm71tQRejaqfCl1K1hnr8Ik8sqz3dh1EUJ4yj9UFzTfLItkV1ZRoDjZC3mVhs3NV4Jk5ONtM5sExKA0GC6Vqegi-jMN2ldB1j22/s320/2018-12-06+16_47_23-Java+Details+_+Hello+World+-+Internet+Explorer.png" width="320" /></a>
<br />
<br />
Enter the necessary information then click Create Grant. In my case, I chose Special User for Grantee Type and SYSADMIN for Grantee Name.
<br />
<br />
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjobU2sm8PMbBmHTe-BROJJwSbSKZJyEq2ZLqzHWB9cMH_YfFNjghjitxI7uIlwk7VzxnsrwPzln59sClmm_GPT0aSUvTPlLHqMucVf5Zyw0dFWPX-b0TFJuqGs3ZELfdRDRVlLOayQxvZS/s1600/2018-12-06+16_49_01-Create+Grants+-+Internet+Explorer.png" imageanchor="1" style="margin-bottom: 1em; margin-right: 1em;"><img border="0" data-original-height="270" data-original-width="366" height="236" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjobU2sm8PMbBmHTe-BROJJwSbSKZJyEq2ZLqzHWB9cMH_YfFNjghjitxI7uIlwk7VzxnsrwPzln59sClmm_GPT0aSUvTPlLHqMucVf5Zyw0dFWPX-b0TFJuqGs3ZELfdRDRVlLOayQxvZS/s320/2018-12-06+16_49_01-Create+Grants+-+Internet+Explorer.png" width="320" /></a>
<br />
<br />
At this point, we need to restart the following:
<br />
<br />
opmn
<br />
oafm
<br />
<br />
You can refer to this <a href="https://onlineappsdba.com/index.php/2007/06/10/start-up-shutdown-scripts-in-oracle-apps-r12/" target="_blank">link</a> for details on how to start and stop Oracle scripts.
<br />
<br />
To invoke the web service, I'm going to use Postman as my REST client. Below are the details for the request:
<br />
<ul>
<li>Method: GET
</li>
<li>Url: http://<host>:<port>/webservices/rest/hw/getHelloWorld/?ctx_responsibility=SYSTEM_ADMINISTRATOR&amp;ctx_respapplication=SYSADMIN&amp;ctx_securitygroup=STANDARD&amp;ctx_nlslanguage=AMERICAN
<br />
where:
<ul>
<li>ctx_responsibility. FND Responsibility Key</li>
<li>ctx_respapplication. FND Application Short Name</li>
<li>ctx_securitygroup. Security Group</li>
<li>ctx_nlslanguage. NLS Language</li>
</ul>
More information on these <a href="https://docs.oracle.com/cd/E18727_01/doc.121/e12065/T511473T516919.htm#isgdg_restheader" target="_blank">here</a>.
</port></host></li>
<li>Authorization: Basic &lt;encoded username:password&gt;</li>
<li>Content-Type: application/json</li>
</ul>
<br />
See sample response below:
<br />
<pre><code class="json">{
    "OutputParameters": {
        "ControlBean": [
            {
                "fields": "",
                "filter": "",
                "limit": "",
                "offset": "",
                "sort": ""
            }
        ],
        "value": [
            "Hello World"
        ]
    }
}
</code></pre>
<br />
See screen shot below for sample
<br />
<br />
<a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi3nE6lB50SiwcuOT4DK4w2meHDAhzWW-kaMEpvTjaRa-uv0hVuSMai7tiCx3l1sg-p1iA2cpMGX3WykWOzgBclS1-Hf8iiQv4clE-gh8MssuTzAj7knScwmTipi56r_b-HwsLiWKmFKOJD/s1600/2018-12-06+17_25_17-Postman.png" imageanchor="1" style="margin-bottom: 1em; margin-right: 1em;"><img border="0" data-original-height="545" data-original-width="1099" height="159" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi3nE6lB50SiwcuOT4DK4w2meHDAhzWW-kaMEpvTjaRa-uv0hVuSMai7tiCx3l1sg-p1iA2cpMGX3WykWOzgBclS1-Hf8iiQv4clE-gh8MssuTzAj7knScwmTipi56r_b-HwsLiWKmFKOJD/s320/2018-12-06+17_25_17-Postman.png" width="320" /></a>
<br />
<br />
And that's it. Enjoy.
<br />
<br />
References:
<br />
<br />
<ul>
<li><a href="https://docs.oracle.com/cd/E18727_01/doc.121/e12169/T511175T543269.htm" target="_blank">Administering Custom Integration Interfaces and Services</a></li>
<li><a href="https://docs.oracle.com/cd/E26401_01/doc.122/e20925/T511175T620514.htm" target="_blank">Oracle E-Business Suite Integrated SOA Gateway Error Messages</a></li>
</ul>
