
    #!div class="important" style="border: 2pt solid; text-align: center" 
    '''''DEPRECATED, NO LONGER USED''''' 
# Nutch packaging for Fedora
 

## Overview

The goal of this page is to organize the work needed to get Nutch packaged and into Fedora. 

## Version

Using nightly builds from: http://hudson.zones.apache.org/hudson/job/Nutch-trunk/

 The reason for using a nightly build and not the release "0.9" is that I ran into problems when trying to index "/help/index.pxt".  Nutch was crashing, I looked on the mailing list and saw other people had similar problems which were resolved when they upgraded to the nightly builds.
## Estimate

 * We are estimating 2 weeks to get nutch and all 3rd party packages into RPMs we can use to begin the submission process into Fedora.

 * Bug 477196 -  Repackage nutch so it's built from source  https://bugzilla.redhat.com/show_bug.cgi?id=477196
## Nutch Dependencies

Looking at: nutch-2008-11-12_04-01-21


|  JAR File  |  RPM Name  |  Spacewalk Version  |  Fedora-9/10 Version  |  Packager/Owner  |  Comments  |  Status |
| --- | --- | --- | --- | --- | --- | --- |
|  commons-cli-2.0-SNAPSHOT.jar  |  jakarta-commons-cli  |    |  1.0  |  jmrodi  |  Might need a version upgrade? Orphaned in Fedora |    |  |
|  commons-logging-1.0.4.jar  |  jakarta-commons-logging  |    |  1.0.4  |    |    |  DONE  |
|  commons-logging-api-1.0.4.jar  |  jakarta-commons-logging  |    |  1.0.4  |    |    |  DONE  |
|  commons-codec-1.3.jar  |  jakarta-commons-codec  |    |  1.3  |    |    |  DONE  |
|  commons-el.jar  |  jakarta-commons-el  |    |  1.0  |    |  under jetty-ext   |  DONE  |
|  commons-httpclient-3.0.1.jar  |  jakarta-commons-httpclient  |    |  3.1.0  |    |    |    |  |
|  commons-lang-2.1.jar  |  jakarta-commons-lang  |    |  2.3  |    |    |    |  |
|  log4j-1.2.15.jar  |  log4j  |    |  1.2.14  |    |    |  IN_FEDORA  |
|  pmd-3.6.jar  |  pmd  |    |  3.6  |    |    |  DONE  |
|  jaxen-1.1-beta-7.jar  |  jaxen  |    |  1.1  |    |  under pmd-ext   |  DONE  |
|  tika-0.1-incubating.jar  |    |    |    |    |  http://www.apache.org/dyn/closer.cgi/incubator/tika  |  NEEDS_RPM  |
|  jakarta-oro-2.0.8.jar  |  jakarta-oro  |    |  2.0.8  |    |    |  IN_FEDORA  |
|  jasper-compiler.jar  |  tomcat5-jasper  |    |  5.5.27  |   |  under jetty-ext  |   |  |
|  jasper-runtime.jar   |  tomcat5-jasper  |    |  5.5.27  |   |  under jetty-ext  |   |  |
|  jsp-api.jar  |  tomcat5-jsp-2.0-api  |    |  5.5.27  |   |  as tomcat5-jsp-2.0-api in Fedora  |  IN_FEDORA  |
|  servlet-api.jar  |  tomcat5-servlet-2.4-api  |    |  5.5.27  |    |  as tomcat6-servlet-2.5-api in Fedora  |  IN_FEDORA  |
|  ant.jar  |  ant  |    |  1.7.0  |   |  under jetty-ext  |  IN_FEDORA  |
|  lucene-core-2.3.0.jar   |  lucene  |    |  2.3.0  |    |    |  DONE  |
|  lucene-misc-2.3.0.jar  |  lucene  |    |  2.3.0  |    |    |  DONE  |
|  xerces-2_6_2-apis.jar  |  UNSURE  |    |    |    |  This might be included with xerces-j2, not sure  |    |  |
|  xerces-2_6_2.jar  |  xerces-j2  |    |  2.7.1  |    |    |    |  |
|  hadoop-0.18.1-core.jar  |    |    |    |    |    |    |  |
|  jets3t-0.6.0.jar  |    |    |    |    |  https://jets3t.dev.java.net/  |  NEEDS_RPM  |
|  junit-3.8.1.jar   |  junit  |    |  3.8.2  |    |    |  IN_FEDORA   |
|  taglibs-i18n.jar  |  unsure, might be related to jakarta-taglibs-i18n  |    |  not in fedora  |    |    |  NEEDS_RPM  |
|  icu4j-3_6.jar   |  icu4j  |    |  3.6.1  |    |    |  DONE  |
|  jetty-5.1.4.jar  |  jetty  |    |  5.1.14  |    |    |  DONE  |
## Hadoop Dependencies
 

Looking at: hadoop-0.19.0

|  JAR File  |  RPM Name  |  Spacewalk Version  |  Fedora Version  |  Packager  |  Comments  |  Status |
| --- | --- | --- | --- | --- | --- | --- |
|  commons-cli-2.0-SNAPSHOT.jar  |  jakarta-commons-cli  |    |  1.0  |  jmrodi  |  Might need a version upgrade?  |    |  |
|  commons-codec-1.3.jar  |  jakarta-commons-codec  |    |  1.3  |    |    |  ORPHANED_IN_FEDORA  |
|  commons-httpclient-3.0.1.jar  |  jakarta-commons-httpclient  |    |  3.1.0  |    |    |  ORPHANED_IN_FEDORA  |
|  commons-logging-1.0.4.jar  |  jakarta-commons-logging  |    |  1.0.4  |    |    |  ORPHANED_IN_FEDORA  |
|  commons-logging-api-1.0.4.jar  |  jakarta-commons-logging  |    |  1.0.4  |    |    |  ORPHANED_IN_FEDORA  |
|  commons-net-1.4.1.jar  |   jakarta-commons-net  |    |  1.4.1  |    |    |  ORPHANED_IN_FEDORA  |
|  hsqldb-1.8.0.10.jar  |  hsqldb   |    |  1.8.0.10 (F10)  |    |    |  IN_FEDORA  |
|  junit-3.8.1.jar   |  junit  |    |  3.8.2  |    |    |  IN_FEDORA  |
|  jetty-5.1.4.jar  |  jetty  |    |  5.1.14  |    |    |  IN_FEDORA  |
|  log4j-1.2.15.jar  |  log4j  |    |  1.2.14  |    |    |  IN_FEDORA  |
|  oro-2.0.8.jar  |  jakarta-oro  |    |  2.0.8  |    |    |  IN_FEDORA  |
|  servlet-api.jar  |  tomcat5-servlet-2.4-api  |    |  5.5.27  |    |    |  IN_FEDORA  |
| xmlenc-0.52.jar   |  xmlenc  |    |  0.52  |  fabiand   |  http://xmlenc.sourceforge.net/  |  IN_FEDORA  |
|  jets3t-0.6.1.jar  |    |   |    |  jmatthews  |  https://jets3t.dev.java.net/  |  FedoraPackageReview - [bz 484281](https://bugzilla.redhat.com/show_bug.cgi?id=484281) |
|  kfs-0.2.0.jar  |    |    |    |    |  http://kosmosfs.wiki.sourceforge.net/  |  BUILD FAILED, I think it needs log4cpp  |
|  slf4j-api-1.4.3.jar  |    |    |    |  fabiand  |  Build uses maven, http://www.slf4j.org/dist/slf4j-1.5.5.tar.gz  |  NEEDS_RPM, IN_PROGRESS - fabiand  |
|  slf4j-log4j12-1.4.3.jar  |    |    |    |  fabiand  |  Build uses maven, http://www.slf4j.org/dist/slf4j-1.5.5.tar.gz  |  NEEDS_RPM, IN_PROGRESS - fabiand  |

And looking at hadoop 0.20 they use ivy, http://ant.apache.org/ivy/, a new buildtool, maybe similar to maven ...







