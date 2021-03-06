
    #!div class="important" style="border: 2pt solid; text-align: center"
    '''''DEPRECATED, NO LONGER USED'''''
# Background



Support for users with read-only access has been a common request of Satellite customers.

While most are agreed our ideal implementation to solve this problem would be (a) giving users access to multiple orgs and then (b) implementing a "read-only" role which restricted the existing UI, this approach was ruled out due to what could be extremely large development and QA impact.

 * Multi-Org Access
  * Probably the most manageable of the two pieces but still involves some significant changes to underlying data model. 
  * Many queries will need to be updated if we introduce the concept of a user logged in and working with an org other then his "home" org as per today.
  * Roles may need to be reworked to support associations with a particular org.
 * Read-Only UI
  * Entails a complete re-examination of virtually all actions and menu's in the existing satellite UI to now expose pages if the user has the correct role, but still hide the submit buttons and prevent actual changes.
  * Large amount of effort to systematically find and consider *all* acl's.
  * Code update is part of the battle, still a huge impact on development and QA to make sure every change they made took effect properly.
  * API would also need some attention.
  * Customers would likely still want fine grained control over what things their read-only users can see, which is a level of fine grained permissions we're not yet prepared for.
  * Completing Java migration is a likely requirement for implementing this, otherwise we'll need to do so in two languages...

However, after several customer discussions we found that this requirement more often than not boils down to a desire for a support user who can log in and perform some simple searches on systems, users, and organizations. As such we decided to pursue this path by implementing a new tab in the satellite UI exposing a simple search interface to users who have the appropriate role.
# Requirements



 1. Define a new "Support User" role which exposes a new top level "Support" tab, visible when they login.
 1. Support tab will link to a simple search interface.
  * For initial release this interface will allow only for searching of *systems* by:
   * Hostname
   * System ID
   * Custom Field
   * Package Installed
  * Search results will link to existing SDC pages but with all submit buttons disabled. Must be careful here to not expose actions that shouldn't be.
  * Other entities to be searched will be added in future releases. (see Customer Search Requests below)
 1. Support users can search across multiple organizations.
  1. By default when created or given the support role, users will see all orgs their home org trusts. (trusts are a feature planned for the second phase of multi-org support)
  1. Create a new UI screen to allow limiting to a selected *subset* of the trusted orgs.
  1. Removal of an org trust must result in removal of the org from any configured subsets. (cascading)
 1. Support role can be given out by org admins. (to users in their org)
 1. Existing functionality available to user with no roles should be audited to ensure we're comfortable with it being available to a user with the Support role.
  * Currently a user without roles can just register systems, and view channels / errata / scheduled actions in their home org.
 1. Create support user API handler offering the same functionality as is available in the UI.
  * Lay the groundwork for community involvement in expanding future support user searches.
  * Must ensure support users cannot utilize existing API calls.
    * when defining apis, let's try to reuse existing calls. i.e. I don't see the need for supportuser.assignSupportUserRole(user) as we already have a user.addRole method. - JesusRodriguez
 1. Plan for configurable support user functionality. Support user will not be the same thing to all users, some deployments will wish to tweak what the support user role actually can do.
# Specification



 1. Add "Support User" role and "Support" top level UI tab. (*estimate: 1 day*)
 1. Add search interface: (*estimate: 3 day*)
  1. *Mockups needed* but initially envisioning two dropdowns and a textbox. Dropdown 1 lists objects to search for, in this case "Systems". Dropdown 2 lists criteria supported for that object type (and thus must be updated on changes to Dropdown 1) such as hostname, ID, etc.
 1. Add small framework for adding new "search for" types and associated "search by" criteria. (*estimate: 3 days*)
 1. Add system search query. (*estimate: 1 day*)
  1. Must search across all trusted organizations. Will be dependent on [Multi-Org Phase 2](Features_MultiOrg2).
 1. Display search results using existing system list JSP code. (system_listdisplay.jspf) (*estimate: 1 day*)
  * Link search results to existing SDC pages.
 1. Modify SDC pages to remove all action buttons and links for users who do not have appropriate permissions. (*estimate: 15 days*)
  * Must also modify struts actions to reject for users who do not have appropriate permissions.
  * Will depend on upcoming fine grained permissions specification, could be a lot of work here isolating each action that can be performed and adding an appropriate permission to the system.
 1. Audit functionality available to a user with just the support role. (*estimate: 2 days*)
 1. Implement new API handler for support user searches. (*estimate: 5 days*)
  * Ensure existing API calls cannot be used by users with *just* the support role.
# Notes

## Customer Search Requests




While unable to get most of these in for a first release, we'll focus mainly on systems and bring as many of these other objects in as possible in coming releases.

 1. Allow searching for:
  1. Systems
   1. Search By:
    1. hostname 
    1. system id  
    1. custom info field 
    1. running kernel (or package installed)  
   1. View:
    1. running kernel
    1. last check in   
    1. last booted
    1. available updates and errata
    1. applied errata
    1. subscribed channels
    1. full hardware profile   
    1. scheduled action status
    1. action history
    1. system owner / contact info
  1. Errata
   1. Search By:
    1. CVE ID     (Supported in Search-Server)
    1. RHBA/RHSA/RHEA ID   (Supported in Search-Server)
    1. Package Name   (Supported in Search-Server)
    1. Issue Date   (Supported in Search-Server)
    1. Severity (Bug | Enhancement | Security (Critical, Moderate, Important)   
   1. View:
    1. Normal errata details
    1. Systems still affected
    1. Systems with errata applied. (including date)
  1. Software Channels
   1. Search By:
    1. Browsing was requested, not sure if this affects our plans.
   1. View:
    1. List of packages.
    1. List of subscribed systems.
    1. Errata issued (with issued/cloned date)
  1. Organizations
   1. Search By:
    1. Org ID
    1. Org name
    1. Browsing requested.
   1. View
    1. System/software entitlement usage.
  1. Entitlements
   1. Search By:
    1. Browsing requested.
   1. View
    1. Allocation to organizations.
  1. User
   1. Search By:
    1. email address
    1. real name
    1. user id
    1. login name
   1. View:
    1. user details
  1. Activation Keys (look up for user?)


||Type of Search ||Search Criteria || Supported in Search-Server || Comments ||
||Systems || hostname || yes || ||||
||Systems || system id || yes || ||||
||Systems || custom info || yes || ||||
||Systems || running kernel || yes || ||||
||Systems || package installed || yes || ||||
||Systems || last checkin || yes || ||||
||Errata || CVE ID  || yes || ||||
||Errata || RHBA/RHSA/RHEA ID || yes ||
||Errata || Package Name || yes || ||||
||Errata || Issue Date || yes || ||||
||Errata || Severity (Bug | Enhancement | Security (Critical, Moderate, Important) || NO || ||||
||Software Channels || * || NO ||
||ORG || * || NO ||
||Entitlements || * || NO ||
||User || * || NO ||