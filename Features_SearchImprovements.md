# Overview

System and Errata search will be integrated with the lucene based external search service, plus new search functionality for satellite documents will be added.

# Requirements



1. Errata
 1. Errata search will have feature parity with RHN Satellite 5.1 errata search
 1. Errata search will also have the ability to search using these additional types: CVE, Date Range,  Topic, Description, Solution
2. System
 1. System search will have feature parity with RHN Satellite 5.1 system search
 1. System search will also have the ability to search using the additional criteria of: Number of CPUs
 1. In addition, System search will allow matches to be restricted to a particular system group. 
3. Docs
 1. Search will be able to index the help documentation to allow the user to search the following docs:
  1. Reference Guide
  1. Satellite Guide
  1. Proxy Guide
  1. Client Configuration
  1. Channel Management
  1. Proxy Release Notes
  1. Satellite Release Notes
  1. API             (* Wasn't Implemented *)
# Specifications

## Systems


#### Filter Matches On


  * Invert match results

  * Search within system set manager
  * Search within a specific system group
  * Filter based on org
#### Searchable Data

1. Activity

 * Days Since Last Check-in
 * Days Since First Registered
2. Details
 * Name/Description <- Default criteria for system search
 * ID
 * Custom Info
 * Snapshot Tag
 * Running Kernel (*Implemented late in dev cycle, after spec was formed*)
3. Packages
 * Installed Packages
 * Needed Packages
4. DMI Info
 * System
 * BIOS
 * Asset Tag
5. Location
 * Address
 * Building
 * Room
 * Rack
6. Hardware Devices
 * Description
 * Driver
 * Device ID
 * Vendor ID
7. Network Info
 * Hostname
 * IP Address
 * netmask  (*Not Implemented*)
 * broadcast address  (*Not Implemented*)
 * mac address (*Not Implemented*)
 * network module (driver)  (*Not Implemented*)
8. Hardware
 * CPU Model
 * CPU MHz Less Than
 * CPU Mhz Greater Than
 * Number of CPUs ****** - was not part of Satellite 5.1
 * RAM Less Than
 * RAM Greater Than
#### Approach

* Changes to Search Service [ estimate: 8 days ]

 * Flesh out model at: com.redhat.satellite.search.db.models.Server 
 * Update db query file to populate a Server model: src/config/com/redhat/satellite/search/db/server.xml 
 * Flesh out DocumentBuilder at: com.redhat.satellite.search.index.builder.ServerDocumentBuilder
 * Flesh out Indexing task at: com.redhat.satellite.search.scheduler.tasks.IndexSystemsTask[[BR]]Update com.redhat.satellite.search.scheduler.ScheduleManager to run IndexSystemsTask
* Changes to WebUI [ estimate: 2 days]
 * Need to update systems/Search  
  * Update jsp to account for searching for matches within particular "system groups"[[BR]]/WEB-INF/pages/systems/systemsearch.jsp
  * Update to use external Search Service instead of SystemManager.systemSearch(): [[BR]]com.redhat.rhn.frontend.action.systems.SystemSearchSetupAction
 * Update <searchbar> to use new SearchService
## Errata

#### Filter Matches On


 * Severity and/or Type

#### Searchable Data

 * Advisory (RHBA/RHSA/RHEA ID)  <Implemented>

 * Synopsis <Implemented>
 * Package Name <Implemented>
 * CVE 
 * Issue Date Range
 * Topic/Description/Solution
#### Approach

 * Changes to Search Service [3 days ](estimate_)

  * Add CVE info to com.redhat.satellite.search.db.models.Errata
  * Add support for CVE to com.redhat.satellite.search.scheduler.tasks.IndexErrataTask
  * Add fetching CVE data from DB to errata.xml under src/config/com/redhat/satellite/search/db
 * Changes to WebUI [2 days](estimate_)
  * Edit com.redhat.rhn.frontend.action.errata.ErrataSearchAction, add support for searching: CVE, topic, description, solution
  * Update /WEB-INF/pages/errata/erratasearch.jsp for: CVE, topic, description, solution
  * To handle "Date Range" search, should this use ErrataManager and not bother with calling to Search Service?[[BR]]Looks like Errata Manager will need to be modified to support a look up of Errata by date range.
  * Add restriction of matches based on optional criteria of Severity/Type to ErrataSearchAction & erratasearch.jsp
  * Add Errata search link to !YourRHN Tasks section, "Search for:  Packages | Systems | Errata"
  * Ensure <searchbar> is using new Errata Search Service
## Docs

#### Filter Matches On


 * Guides/Manuals

 * API
#### Searchable Data

 * Reference Guide

 * Satellite Guide
 * Proxy Guide
 * Client Configuration
 * Channel Management
 * Proxy Release Notes
 * Satellite Release Notes
 * API
#### Problem Summary

 * Need a means of indexing these documents.  

  Current search service implementation indexes data from a database.  These documents exist as files on the satellite which are served through tomcat.
#### Resolution

Add spider capability for search service to crawl a specific URL and index all content.


Propose to use "nutch" as our spider.  This is the "spider" associated with Lucene.  
Nutch will crawl our help docs, generate a list of content/links, and index the data.  
Our Search Service will handle the rest of the work by reusing the lucene indexes generated by nutch. 

Will need to crawl:
 * Guides/Manuals[[BR]]Start URL: !https://<spacewalk-address>/help/index.pxt[[BR]]Restrict URL to descendants of:  !https://<spacewalk-address>/help
 * API Docs[[BR]]Start URL: !https://<spacewalk-address>/rhn/apidoc/index.jsp[[BR]]Restrict URL to descendants of: !https://<spacewalk-address>/rhn/apidoc
#### Approach

 * Changes to Search Service [15 days](estimate_)

  * Leverage nutch to create "indexes" representing the "Guide/Manual/API Docs".
   * Create a "generate/fetch/update" task using Nutch library.  Task will reside in com.redhat.satellite.search.scheduler.tasks 
   * Documents will be updated infrequently
    * Avoid wasting cycles, re-fetching/indexing docs when we know they haven't changed.
    * We want to support an ability to upgrade the docs on the server, then have that trigger a mechanism to kick off a refresh of the associated search index.
   * If fetch/index of docs takes a considerable amount of time, support the ability to alert a user that the docs index is being formed and not yet ready.  


 * Localization Issues
  * Nutch will use an Analyzer per language to Tokenize for indexing.  SearchService needs to pay attention to this and use the same Analyzer when forming queries for search.
   * Background:  Each language has certain ignore words (a, an, the, etc), Analyzers written per language understand this and will drop those words from being indexed.  We need to ensure the same rules are followed for searching, as were followed for indexing. 
 * Fields planned for docs search,
  * Search on Content and/or Title with the result returning a <url> to access the page
   * content/title does not need to be STORED, but should be TOKENIZED[[BR]]As a lucene Field this means: Field.Store.NO, Field.Index.TOKENIZED
   * url  needs to be STORED, but should not be TOKENIZED[[BR]]As a lucene Field this means: Field.Store.YES, Field.Index.UN_TOKENIZED 
 * Query/Search will be handled by existing Search Service implementation
  * Add a means for selecting the Analyzer and QueryParser based on index name.
   * New methods in IndexManager: getQueryParser(), getAnalyzer(), 
   * Process returned search results based on index name
  * Changes to WebUI
   * Integrate with upper right hand corner search bar
   * Add Doc Search to YourRHN, "Search for:  Packages | Systems | Errata | Users | Docs"
   * Add Doc Search to Help page 
# Development Status

## System Search


### Tasks


||  *Task* || *Target Release* ||  *Estimate (days)* || *Developer* || *Status* || *Notes* ||

||  -----   ||  ||  ||  ||  ||  ||||
||  --XMLRPC Search Server specific tasks-- ||   ||  ||  ||  ||  ||||
|| Create a generic index class || 0.3 || 3 || jmatthews || DONE || || 
|| Index/Search: hwdevice || 0.3 || 1 || jmatthews || DONE ||  ||||
|| Index/Search: systems  || 0.3 || 2 || jmatthews || DONE ||  ||||
|| Index/Search: snapshot tags  || 0.3 || .5 || jmatthews || DONE ||  ||  ||||
|| Index/Search: custom info  || 0.3 || .5 || jmatthews || DONE ||  ||||
|| Handle modified objects from DB to updated lucene index || 0.3 || 1 || jmatthews || DONE ||  ||||
|| Add a reconcile task that deletes items from the lucene index which have been removed from the DB || 0.3 || 1 || jmatthews || DONE  ||  ||||
|| Add "matching fields" return data which will parse a Lucene Explanation and determine which field contributed the most to the score || 0.3 || .5 || jmatthews || DONE || Added a simple version which parses the query and returns the first term, would be nice to look into using the Explanation in 0.4 or later, to get a more accurate feel for what caused the match.  

|  ----  |    |    |    |    |    |  |
| --- | --- | --- | --- | --- | --- | --- |
|  --WebUI specific tasks--  |    |    |    |    |    |  |
|  WebUI passing system search calls to XMLRPC search server  |  0.3  |  .5  |  jmatthews  |  DONE  |    |  |
|  Update systemsearch.jsp to newer JSP list-tag  |  0.3  |  1  |  jmatthews  |  DONE  |    |  |
|  Merge SystemSearchAction and SystemSearchSetupAction to single class, model after PackageSearchAction  |  0.3  |  .5  |  jmatthews/jsherrill |  DONE  |   |  |
|  Handle System Set Manager "whereToSearch" option  |  0.3  |  .5  |  jmatthews  |  DONE  |    |  |
|  Handle "invert matches" option  |  0.3  |  .5  |  jmatthews  |  DONE  |    |  |
|  Handle system search by packages  |  0.3  |  .5  |  jmatthews  |  DONE  |   |  |
### Functionality Check - full run through webui to xmlrpc server



|   *Functionality*  |  *Target Release*  |  *Status*  |  *Notes*  |
| --- | --- | --- | --- | --- |
|   -----   |    |    |    |   |  |
|  invert match results  |  0.3  |  VERIFIED  |   |  |
|  search within system set manager  |  0.3  |  VERIFIED  |   |  |
|  search within a specif system group  |  0.4  |  Punt  |  New functionality  |
|  filter based on org  |  0.4  |  Punt  |  New functionality  |
|   --- Searchable Data ---  |    |    |    |
|  Activity: Days Since Last Check-in  |  0.3  |  VERIFIED  |    |
|  Activity: Days Since First Registered  |  0.3  |  VERIFIED   |    |  |
|  Details: Name  |  0.3  |  VERIFIED  |   |  |
|  Details: Description  |  0.3  |  VERIFIED  |   |  |
|  Details: ID  |  0.3  |  VERIFIED  |   |  |
|  Details: Custom Info  |  0.3  |  VERIFIED  |   |  |
|  Details: Snapshot Tag  |  0.3  |  VERIFIED  |   |  |
|  Packages: Installed Packages  |  0.3  |  VERIFIED  |  Would like to see if we can speed this up  |
|  Packages: Needed Packages  |  0.3  |  VERIFIED  |  Would like to see if we can speed this up  |
|  DMI Info: System  |  0.3  |  VERIFIED  |    |  |
|  DMI Info: BIOS  |  0.3  |  VERIFIED  |    |  |
|  DMI Info: Asset Tag  |  0.3  | VERIFIED  |    |  |
|  Location: Address  |  0.3  |  VERIFIED  |    |  |
|  Location: Building  |  0.3  |  VERIFIED  |    |  |
|  Location: Room  |  0.3  |  VERIFIED  |    |
|  Location: Rack  |  0.3  |  VERIFIED  |    |  |
|  Hardware Devices: Description  |  0.3  |  VERIFIED  |    |  |
|  Hardware Devices: Driver  |  0.3  |  VERIFIED  |    |  |
|  Hardware Devices: Device ID  |  0.3  |  VERIFIED  |   |  |
|  Hardware Devices: Vendor ID  |  0.3  |  VERIFIED  |    |
|  Netork Info: Hostname  |  0.3  |  VERIFIED  |   |  |
|  Network Info: IP Address  |  0.3  |  VERIFIED  |   |  |
|  Network Info: Netmask  |  0.4  |  Punt  |  New functionality, Most likely punt to 0.4  |
|  Network Info: Broadcast Address  |  0.4  |  Punt  |  New functionality, Most likely punt to 0.4  |
|  Network Info: MAC Address  |  0.4  |  Punt  |  New functionality, Most likely punt to 0.4  |
|  Network Info: Network Module (driver)  |  0.4  |  Punt  |  New functionality, Most likely punt to 0.4  |
|  Hardware: CPU Model  |  0.3  |  VERIFIED  |   |  |
|  Hardware: CPU MHz less than  |  0.3  |  VERIFIED  |  Updated how range queries are sent from webui.  |
|  Hardware: CPU MHz greater than  |  0.3  |  VERIFIED  |   |  |
|  Hardware: Number of CPUs  |  0.3  |  VERIFIED  |  New functionality  |
|  Hardware: RAM Less than  |  0.3  |  VERIFIED  |   |  |
|  Hardware: RAM Greater than  |  0.3  |  VERIFIED  |   |  |
### Issues to Fix



|   *Issue*  |  *Status*  |  *Notes*  |
| --- | --- | --- |
|   -----   |    |    |    |  |
|  Searches which result in no matches causes a problem with ElaborationDecorator in the new list tag  |  DONE   |  Quickfix to ElaborationDecorator  |
|  Hwdevice indexing is indexing 2x to 3x more data than we have  |  DONE  |   |  |
|  Indexing for all data will become corrupted if task is stopped before it fully completes  |  DONE  |  We weren't indexing data in order, so updating of last id indexed was not usable |
|  Not handling pagination correctly  |  DONE  |  Using ListSubmitable fixed this...  |
|  Need to add location info (building, address, rack, room) to SystemSearchResult dto  |  DONE  |  Result is returned correctly from search server, needs to be added to DTO on webui side for better display  |
|  Need to add DMI info (asset, bios, system) to SystemSearchResult dto  |  DONE  |   |  |
|  Display hwdevice info on webui from a system search result  |  DONE  |  Displaying match from hwdevices is different since each system has a Set of hwdevices.   |
|  Handle 'special chars' in searchQuery, ":" "(" ")" "-"  |   |   |  |
|  Range Queries have issues, looks like they aren't handling Number to String conversions correctly  |  DONE  |  Using Lucene's NumberTools so lexicographically sorting is possible  |
|  Sort columns with new list-tag isn't working, i.e. click column but results are not sorted.  |  DONE  |  sort only works on data fields which do _not_ require elaboration  |
|  Tighten up the matching threshold to reduce poor matches  |  DONE  |  Added a configuration option "system_score_threshold" set to 0.30   |
|  Add CSV Tag to systemsearch.jsp  |   |   |  |
|  Create a Java version of the reindex script which can be delivered, possibly tie into /etc/init.d/rhn-search so we can give a "reindex" option to it and have it kick off  |  DONE  |   |  |
|  Explore how to hook into when a new system is register/modified/deleted and cause SearchServer to reindex on that event  |  FUTURE |  This won't be for 0.3, it's more something to consider  |
# Outstanding Issues as of 2/5/09

 * System Search

  * Missing Filter Results on
   * Filter by specific system group
   * Filter based on org 
  * Missing search on network params:
   * netmask
   * broadcast address
   * mac address
   * network module (driver) 
 * Doc Search
  * API Docs are not indexed/searchable