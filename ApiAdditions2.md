
    #!div class="important" style="border: 2pt solid; text-align: center" 
    '''''DEPRECATED, NO LONGER USED''''' 
# OLD API Page



This page is simply a record of apis added during the spacewalk 0.1 - 0.6 time frame.


||  *Task* || *Target Release* ||  *Estimate (hours)* || *Developer* || *Status* || *Notes* ||
||  ----   ||  ||  ||  ||  ||  ||||
||  *Namespace: package.provider* ||  ||  ||  ||  ||  ||||
|| package.provider.list(string sessionKey)  || 0.4 || 3  || jsherrill ||  done || * List the package providers available
|| package.provider.listKeys(string sessionKey, String provider)  || 0.4 || 3  || jsherrill ||  done || * List the keys for a particular provider
|| package.provider.associateKeyKey(string sessionKey, string providerName, String key, String type)  || 0.4 || 3  || jsherrill || done  || *Add a new package provider and/or key


|   *Namespace: activationkey*  |    |    |    |    |    |  |
| --- | --- | --- | --- | --- | --- |
|   activationkey.addPackages(string sessionKey, string activationKey, array[ struct[ string name, string arch]])  |  0.4  |  3   |  bbuckingham  |  done   |  * Add a set of packages to an activation key.[[BR]]* May optionally include an arch. [[BR]]* This api will deprecate the existing addPackageNames  |
|   activationkey.removePackages(string sessionKey, string activationKey, array[ struct[ string name, string arch]])  |  0.4  |  3   |  bbuckingham  |  done   |  * Remove a set of packages from an activation key.[[BR]]* This api will deprecate the existing removePackageNames  |
|   activationkey.enableConfigDeployment(string sessionKey, string activationKey)  |  0.3  |  3   |  coec  |  done   |    |  |
|   activationkey.disableConfigDeployment(string sessionKey, string activationKey)  |  0.3  |  2  |  coec  |  done   |    |  |
|   activationkey.checkConfigDeployment(string sessionKey, string activationKey)  |  0.3  |  2  |  coec  |  done   |    |  |
|   activationkey.listActivatedSystems(string sessionKey, string activationKey)  |  0.5  |  3  |  bbuckingham  |  done   |  List the systems that have been activated by the given activation key.  |
|   *Namespace: channel.access*  |    |    |    |    |    |  |
|  channel.access.enableUserRestrictions(string sessionKey, string channelLabel)  |  0.4  |  4  |  bbuckingham  |  done  | (Multi Org Phase2 support)[[BR]]* Enable user/subscriber restrictions for the channel.  When user restrictions are enabled, only selected users may subscribe to the channel.[[BR]]* Returns 1 on success, exception thrown otherwise. |
|  channel.access.disableUserRestrictions(string sessionKey, string channelLabel)  |  0.4  |  4  |  bbuckingham  |  done  | (Multi Org Phase2 support)[[BR]]* Disable user/subscriber restrictions for the channel.  When user restrictions are disabled, all users within the organization may subscribe to the channel.[[BR]]* Returns 1 on success, exception thrown otherwise. |
|  channel.access.setOrgSharing(string sessionKey, string channelLabel, string access)  |  0.4  |  4  |  bbuckingham  |  done  | (Multi Org Phase2 support)[[BR]]* Set the org sharing access for the channel. Access may be set to public, private or protected. [[BR]]* Returns 1 on success, exception thrown otherwise. |
|  channel.access.getOrgSharing(string sessionKey, string channelLabel)  |  0.4  |  4  |  bbuckingham  |  done  | (Multi Org Phase2 support)[[BR]]* Retrieve the current org sharing access for the channel.[[BR]]* Returns a string (public, private or protected). |
|   ----    |    |    |    |    |    |  |
|   *Namespace: channel*  |    |    |    |    |    |  |
|  channel.listAllChannels(string sessionKey)  |  0.4  |  2  |  bbuckingham  |  done  | (Multi Org Phase2 support) |
|  channel.listRedHatChannels(string sessionKey)  |  0.4  |  2  |  bbuckingham  |  done  | (Multi Org Phase2 support) |
|  channel.listPopularChannels(string sessionKey, int popularityCount)  |  0.4  |  2  |  bbuckingham  |  done  | (Multi Org Phase2 support) |
|  channel.listMyChannels(string sessionKey)  |  0.4  |  2  |  bbuckingham  |  done  | (Multi Org Phase2 support) |
|  channel.listSharedChannels(string sessionKey)  |  0.4  |  2  |  bbuckingham  |  done  | (Multi Org Phase2 support) |
|  channel.listRetiredChannels(string sessionKey)  |  0.4  |  2  |  bbuckingham  |  done  | (Multi Org Phase2 support) |
|   ----    |    |    |    |    |    |  |
|   *Namespace: channel.org*  |    |    |    |    |    |  |
|  channel.org.list(string sessionKey, string channelLabel)  |  0.4  |  4  |  bbuckingham  |  done  | (Multi Org Phase2 support)[[BR]]* List the orgs that are trusted by the current organization (orgId) and info on which are trusted to access this channel (channelLabel).[[BR]]* Returns array[ struct[ int trustOrgId, boolean trustEnabled ] ].[[BR]]* The user executing this method must be a Sat/Org/Channel Admin. |


|  channel.org.enableAccess(string sessionKey, string channelLabel, int trustOrgId)  |  0.4  |  4  |  bbuckingham  |  done  | (Multi Org Phase2 support)[[BR]]* Enable trust of an org (trustOrgId) to grant access to view and consume the content of the channel specified (channelLabel).[[BR]]* The org being trusted must be one that from the organization's trust list (e.g. listTrusts).[[BR]]* Returns 1 on success, exception thrown otherwise.[[BR]]* The user executing this method must be an Sat/Org/Channel Admin. |
| --- | --- | --- | --- | --- | --- |
|  channel.org.disableAccess(string sessionKey, string channelLabel, int trustOrgId)  |  0.4  |  4  |  bbuckingham  |  done  | (Multi Org Phase2 support)[[BR]]* Disable trust of an org (trustOrgId) revoking access to view and consume the content of the channel specified (channelLabel).[[BR]]* Returns 1 on success, exception thrown otherwise.[[BR]]* The user executing this method must be an Sat/Org/Channel Admin. |
|   ----    |    |    |    |    |    |  |
|   *Namespace: channel.software*  |    |    |    |    |    |  |
|  channel.software.listErrata(string sessionKey, String channeLLabel, dateTime.iso8601 startDate, dateTime.iso8601 endDate)  |  0.5  |  1  |  jsherrill  |  done  |  deprecated string-date version  |
|  channel.software.listErrata(string sessionKey, String channeLLabel, dateTime.iso8601 startDate)  |  0.5  |  1  |   jsherrill  |  done  |  deprecated string-date version  |
|  channel.software.listErrata(string sessionKey, String channeLLabel, dateTime.iso8601 endDate)  |  0.5  |  1  |  jsherrill  |  done  |  deprecated string-date version  |
|  channel.software.listAllPackages(string sessionKey, string channelLabel, dateTime.iso8601 startDate)  |  0.5  |  3  |  bbuckingham  |  done  |  * List all packages based on the start date given.[[BR]]* This API will deprecate the existing listAllPackages API that has startDate defined as a string.  |
|  channel.software.listAllPackages(string sessionKey, string channelLabel, dateTime.iso8601 startDate, dateTime.iso8601 endDate)  |  0.5  |  3  |  bbuckingham  |  done  |  * List all packages based on the start/end date given.[[BR]]* This API will deprecate the existing listAllPackages API that has startDate and endDate defined as a string.  |
|  channel.software.listErrataByType(string sessionKey, string channelLabel, string advisoryType)  |  0.3  |  4  |  bbuckingham  |  done  |    |  |
|  channel.software.setContactDetails(string sessionKey, string channelLabel, string name, string email, string phoneNumber, string supportPolicy)  |  0.4  |  4  |  bbuckingham  |  done  | (Multi Org Phase2 support[[BR]]* Set the contact/support information for a given channel.[[BR]]* Returns 1 on success, exception thrown otherwise. |
|   byte[] package.getPackage(string sessionKey, int packageId)  |  0.3  |  8  |  jsherrill  |  Done  |    |  |
|   string package.getPackageUrl(string sessionKey, int packageId)  |  0.2  |  5  |  jsherrill  |  Done  |  Obtain the package download URL.  |
|   channel.software.mergeErrata(string sessionKey, string fromChannel, string toChannel)  |  0.3  |  3  |  bbuckingham  |  done  |    |  |
|   channel.software.mergeErrata(string sessionKey, string fromChannel, string toChannel, string startDate, string endDate)  |  0.3  |  6  |  bbuckingham  |  done  |    |  |
|   channel.software.uploadPackage(package)  |  0.6  |  12  |    |    |  This API provides the same basic function as rhnpush; however, there is a desire from the community to include it.  |
|   channel.software.regeneratedNeededCache(string sessionKey)  |  0.5  |  1  |  jsherrill  |  done  |  regenerates errata and package cache  |
|   channel.software.regeneratedNeededCache(string sessionKey,  String channelLabel)  |  0.5  |  1  |  jsherrill  |  done  |  regenerates errata and package cache  |
|   ----    |    |    |    |    |    |  |
|   *Namespace: configchannel*  |    |    |    |    |    |  |
|   configchannel.channelExists(sessionKey, string channelLabel)  |  0.3  |   |  coec  |  done  |  returns 1 if the channel exists or 0 if it does not  |
|   configchannel.scheduleFileComparisons(string sessionKey, string channelLabel, string path, array[serverId](int))  |  0.3  |  4  |  bbuckingham  |  done  |  * Schedule a file comparison on the list of systems provided.[[BR]]* Returns the id of the scheduled action (int actionId).  |
|   ----    |    |    |    |    |    |  |
|   *Namespace: errata*  |    |    |    |    |    |  |
|   errata.delete(string sessionKey, string advisoryName)  |  0.6  |  5  |  bbuckingham  |  done  |  Delete of custom errata.   |
|   errata.findByCve(string sessionKey, string cveName)  |  0.3  |  3  |  bbuckingham  |  done  |  Retrieve errata based on CVE (e.g. CVE-2008-1233)   |
|   errata.setDetails([[BR]]string sessionKey, [[BR]]struct(errata details)[[[BR]]string synopsis, [[BR]]string advisoryName, [[BR]]int advisoryRelease, [[BR]]string advisoryType, [[BR]]string product, [[BR]]string topic, [[BR]]string description, [[BR]]string solution, [[BR]]   string references, [[BR]]string notes, [[BR]]array[struct(bug)[id, string summary](int)], [[BR]]array[keyword](string))  |  0.4  |  3  |  bbuckingham  |  done  |  * Set errata details.  All arguments are optional and will only be modified if included.[[BR]] * Returns 1 on success, exception thrown otherwise.  |
|   errata.addPackages(string sessionKey, string advisoryName, array[packageId](int))  |  0.4  |  2  |  bbuckingham  |  done  |  * Adds a set of packages to an errata.  Invoking this API will not impact packages already associated with the errata.[[BR]] * Returns the number of packages added on success, exception thrown otherwise.   |
|   errata.removePackages(string sessionKey, string advisoryName, array[packageId](int))  |  0.4  |  2  |  bbuckingham  |  done  |  * Remove a set of packages from an errata.[[BR]] * Returns the number of packages removed on success, exception thrown otherwise.   |
|   ----    |    |    |    |    |    |  |
|   *Namespace: kickstart*  |    |    |    |    |    |  |
|   kickstart.listAllIpRanges(string sessionKey)  |  0.2  |  3  |  jsherrill   |  done  |   List all ip ranges for all kickstarts  |
|   kickstart.findKickstartForIp(string sessionKey)  |  0.2  |  3  |  jsherrill      |  done  |    |
|   ----    |    |    |    |    |    |  |
|   *Namespace: kickstart.profile*  |    |    |    |    |    |  |
|   kickstart.deleteProfile(string sessionKey, string kslabel)  |  0.3  |  2  |  jsherrill  |  done  |    |  |
|   kickstart.profile.compareActivationKeys(string sessionKey, string kslabel1, string kslabel2)  |  0.3  |  5  |  jdob   |  done  |    |  |
|   kickstart.profile.comparePackages(string sessionKey, string kslabel1, string kslabel2)  |  0.3  |  5  |  jdob   |  done  |    |  |
|   kickstart.profile.compareAdvancedOptions(string sessionKey, string kslabel1, string kslabel2)  |  0.3  |  5  |  jdob   |  done  |    |  |
|   kickstart.profile.getAdvancedOptions(string sessionKey, string kslabel)  |  0.3  |  2  |  skarmark  |  done  |    |  |
|   kickstart.profile.setAdvancedOptions(string sessionKey, string kslabel, array[struct[optionName, boolean enabled, string value](string))  |  0.3  |  4  |  skarmark  |  done  |  Set advanced options on the profile.  The optionNames supported are based on those available in the UI and includes: autostep, interactive, install, upgrade, text, network, cdrom, harddrive, nfs, url, lang, langsupport keyboard, mouse, device, deviceprobe, zerombr, clearpart, bootloader, timezone, auth, rootpw, selinux, reboot, firewall, xconfig, skipx, key, ignoredisk, autopart, cmdline, firstboot, graphical, iscsi, iscsiname, logging, monitor, multipath, poweroff, halt, service, shutdown, user, vnc and zfcp. The following options do not have a value; therefore, if one is provided it will be ignored: interactive, install, upgrade, text, cdrom, reboot, skipx, autopart, cmdline, graphical, poweroff, halt and shutdown.  |
|   kickstart.profile.getCustomOptions(string sessionKey, string kslabel)  |  0.3  |  2  |  coec  |  done  |    |  |
|   kickstart.profile.setCustomOptions(string sessionKey, string kslabel, string options)  |  0.3  |  4  |  coec  |  done  |  Set the "custom" advanced options on the profile.  The user may provide any options desired; therefore, it should be noted that providing an invalid option can cause kickstarts to fail.  Also, the option text is limited to 2048 characters.  |


|   kickstart.profile.listIpRanges(string sessionKey, int ksLabel)  |  0.2  |  3  |  jsherrill   |  done  |  list ip ranges for a kickstart  |
| --- | --- | --- | --- | --- | --- | --- |
|   kickstart.profile.addIpRange(string sessionKey, int ksLabel)  |  0.2  |  4  |  jsherrill   |  done  |  ksLabel is an int?  |
|   kickstart.profile.removeIpRange(string sessionKey, ksLabel, ip)  |  0.2  |  3  |  jsherrill  | done  |   |  |
|   kickstart.profile.getKickstartTree(string sessionKey, string ksLabel)  |  0.5  |  2  |  bbuckingham  | done  |  Retrieve the label of the kickstart tree associated with the profile. |
|   kickstart.profile.getChildChannels(string sessionKey, string ksLabel)  |  0.5  |  2  |  bbuckingham  | done  |  Retrieve the child channel labels associated with the profile. |
|   ----    |    |    |    |    |    |  |
|   *Namespace: kickstart.profile.system*  |    |    |    |    |    |  |
|   kickstart.profile.system.setNetworkConnection(string sessionKey, string kslabel, boolean dhcp, string interface)  |  0.2  |  3  | partha | done   |  Set the network connection details.  If dhcp is true, dhcp will be utilized; otherwise, it will be static IP.  |
|   kickstart.profile.system.getNetworkConnection(string sessionKey, string kslabel)  |  0.5  |  2  |  bbuckingham | done   |  Get the network connection properties.  |
|   kickstart.profile.system.setSELinux(string sessionKey, string kslabel, string enforcingMode)  |  0.2  |  2  |  partha  | done  |  Set the SELinux preference where enforcingMode is enforcing, permissive or disabled.  |
|   kickstart.profile.system.getSELinux(string sessionKey, string kslabel)  |  0.5  |  2  |  bbuckingham  | done  |  Get the SELinux preference where return is enforcing, permissive or disabled.  |
|   kickstart.profile.system.checkConfigManagement(string sessionKey, string kslabel)  |  0.5  |  2  |  bbuckingham   | done   |  Check the configuration management status for a kickstart profile.  |
|   kickstart.profile.system.enableConfigManagement(string sessionKey, string kslabel)  |  0.2  |  2  | partha   | done   |    |  |
|   kickstart.profile.system.disableConfigManagement(string sessionKey, string kslabel)  |  0.2  |  2  | partha   | done   |    |  |
|   kickstart.profile.system.checkRemoteCommands(string sessionKey, string kslabel)  |  0.5  |  2  |  bbuckingham  | done   |  Check the remote command status for a kickstart profile.  |
|   kickstart.profile.system.enableRemoteCommands(string sessionKey, string kslabel)  |  0.2  |  2  | partha   | done   |    |  |
|   kickstart.profile.system.disableRemoteCommands(string sessionKey, string kslabel)  |  0.2  |  2  | partha   | done  |    |  |
|   kickstart.profile.system.getLocale(string sessionKey, string kslabel)  |  0.3  |  3  |  bbuckingham  |  done  |  * Retrieve the locale information for the kickstart profile.[[BR]] * Returns struct[locale, int useUtc](string)  |
|   kickstart.profile.system.setLocale(string sessionKey, string kslabel, string locale, boolean useUtc)  |  0.3  |  3  |  bbuckingham  |  done  |    |  |
|   kickstart.profile.system.listFilePreservations(string sessionKey, string kslabel)  |  0.4  |  3  |  bbuckingham  |  done  |  List the file preservation lists associated with the profile.  |
|   kickstart.profile.system.addFilePreservations(string sessionKey, fllabels)  |  0.4  |  4  |  bbuckingham  |  done  |  Add a list of file preservation lists to the profile.  |
|   kickstart.profile.system.removeFilePreservations(string sessionKey, fllabels)  |  0.4  |  4  |  bbuckingham  |  done  |  Remove a list of file preservation lists from the profile.  |
|   kickstart.profile.system.listKeys(string sessionKey, string kslabel)  |  0.3  |  3  |  jdob  |  done  | * Retrieve the GPG and SSL keys associated with the kickstart profile.[[BR]]** Renamed from listAssociatedKeys to listKeys in 0.4 - bbuckingham*  |
|   kickstart.profile.system.addKeys(string sessionKey, string kslabel, array[description](string))  |  0.3  |  4  |  jdob  |  done  | * Add GPG and SSL keys to the kickstart profile.[[BR]]** Renamed from associateKeys to addKeys in 0.4 - bbuckingham*  |
|   kickstart.profile.system.removeKeys(string sessionKey, string kslabel, array[description](string))  |  0.4  |  4  |  bbuckingham  |  done  |  Remove GPG and SSL keys from the kickstart profile. |
|   ----    |    |    |    |    |    |  |
|   *Namespace: kickstart.profile.software*  |    |    |    |    |    |  |


|   kickstart.profile.software.appendToSoftwareList(string sessionKey)  |  0.3  |  2  |  bbuckingham  |  done  |    |  |
| --- | --- | --- | --- | --- | --- |
|   kickstart.profile.software.getSoftwareList(string sessionKey)  |  0.3  |  2  |  jortel  |  done  |    |  |
|   kickstart.profile.software.setSoftwareList(string sessionKey)  |  0.3  |  3  |  jortel  |  done  |     |  |
|   ----    |    |    |    |    |    |  |
|   *Namespace: kickstart.profile.keys*  |    |    |    |    |    |  |
|   kickstart.profile.keys.addActivationKey(string sessionKey, string kslabel, string key)  |  0.3  |  3  |  bbuckingham  |  done  |    |  |
|   kickstart.profile.keys.removeActivationKey(string sessionKey, string kslabel, string key)  |  0.3  |  3  |  bbuckingham  |  done  |    |  |
|   kickstart.profile.keys.getActivationKeys(string sessionKey, string kslabel)  |  0.3  |  3  |  bbuckingham   |  done  |    |  |
|   ----    |    |    |    |    |    |  |
|   *Namespace: kickstart.profile.scripts*  |    |    |    |    |    |  |
|   ----    |    |    |    |    |    |  |
|   *Namespace: kickstart.keys*  |    |    |    |    |    |  |
|   kickstart.keys.listAllKeys(string sessionKey)  | 0.3   |  2  |  jdob  |  done  |  * List all GPG and SSL keys for all kickstarts.[[BR]]* Returns array[struct[description, string type](string)]  |
|   kickstart.keys.create(string sessionKey, string description, string type, content)  | 0.3   |  4  |  jdob  |  done  |  * Create a GPG or SSL key. This key may then be associated with a kickstart profile (via kickstart.profile.system.associateKeys()). [[BR]]* Returns 1 on success, exception thrown otherwise.  |
|   kickstart.keys.delete(string sessionKey, string description)  | 0.3   |  4  |  jdob  |  done  |  * Remove a GPG or SSL key. [[BR]]* Returns 1 on success, exception thrown otherwise.  |
|   kickstart.keys.getDetails(string sessionKey, string description)  | 0.3  |  4  |  jdob  |  done  |  * Obtain details on a GPG or SSL key. [[BR]]* Returns struct[description, string type, content](string)  |
|   ----    |    |    |    |    |    |  |
|   *Namespace: kickstart.distributions*  |    |    |    |    |    |  |
|   ----    |    |    |    |    |    |  |
|   *Namespace: kickstart.filepreservation*  |    |    |    |    |    |  |
|   kickstart.filepreservation.listAllFilePreservations(string sessionKey)  |  0.4  |  2  |  bbuckingham  |  done  |  List all file preservation lists.  |
|   kickstart.filepreservation.create(string sessionKey, string name, array[fileName](string))  |  0.4  |  3  |  bbuckingham  |  done  |  Create a file preservation list.  |
|   kickstart.filepreservation.delete(string sessionKey, string name)  |  0.4  |  3  |  bbuckingham  |  done  |  Delete a file preservation list.  |
|   kickstart.filepreservation.getDetails(string sessionKey, string name)  |  0.4  |  3  |  bbuckingham  |  done  |  Retrieve details on a file preservation list.  |
|   ----    |    |    |    |    |    |  |
|   *Namespace: org*  |    |    |    |    |    |  |
|   org.listOrgs(string sessionKey)    |  0.4  |  4  |  jortel  |  done  | (Multi Org Phase2 support)[[BR]]* List the orgs defined by the Spacewalk. [[BR]]* Update the existing API to include the number of "trusts".[[BR]]* The user executing the method must be a Spacewalk Admin. |
|   org.listSoftwareEntitlements(string sessionKey, string label, boolean includeUnentitled)    |  0.6  |  4  |  bbuckingham  |  done  |  List each org's allocation of an entitlement.  If includeUnentitled=TRUE, orgs that have 0 defined for that entitlement will also be included. |
|   org.listSystemEntitlements(string sessionKey, string label, boolean includeUnentitled)    |  0.6  |  4  |  bbuckingham  |  done  |  List each org's allocation of an entitlement.  If includeUnentitled=TRUE, orgs that have 0 defined for that entitlement will also be included. |
|   org.migrateSystems(string sessionKey, array[serverId](int), int toOrgId)    |  0.4  |  24  |  bbuckingham  |  done  | (Multi Org Phase2 support)[[BR]]* Migrate a list of systems from the organization associated with the user executing the request to the destination org provided.  The user executing the request must be an org admin and the destination org must be within that user's org trust. |
|   ----    |    |    |    |    |    |  |
|   *Namespace: org.trusts* ^NEW^  |    |    |    |    |    |  |
|  org.trusts.listTrusts(string sessionKey, int orgId)  |  0.4  |  4  |  jortel   |  done  | (Multi Org Phase2 support)[[BR]]* Lists the orgs that the organization (orgId) may define as trusted and info on whether or not each org (trustOrgId) is in the oganization's trust ring (i.e. org enabled/checked).[[BR]]* Returns array[ struct[ int orgId, string orgName, boolean trustEnabled ] ].[[BR]]* The user executing this method must be a Spacewalk Admin. |
|  org.trusts.addTrust(string sessionKey, int orgId, int trustOrgId)  |  0.4  |  4  |  jortel  |  done  | (Multi Org Phase2 support)[[BR]]* Add an org (trustOrgId) to an organization's (orgId) trust ring.[[BR]]* Only orgs from the organization's trust list (e.g. listTrusts) may be added.[[BR]] * Returns 1 on success, exception thrown otherwise.[[BR]]* The user executing this method must be a Spacewalk Admin. |
|  org.trusts.removeTrust(string sessionKey, int orgId, int trustOrgId)  |  0.4  |  4  |  jortel  |  done  | (Multi Org Phase2 support)[[BR]]* Remove an org (trustOrgId) from an organization's (orgId) trust ring.[[BR]]* Returns 1 on success, exception thrown otherwise.[[BR]]* The user executing this method must be a Spacewalk Admin |
|  org.trusts.listSystemsAffected(string sessionKey, int orgId, int trustOrgId)  |  0.4  |  4  |  jortel  |  done  | (Multi Org Phase2 support)[[BR]]* Retrieve the list of systems in the trusted org (trustOrgId) affected by the trust with an organization (orgId).[[BR]]* Returns ->  array [ struct [ int systemId, string systemName ] ][[BR]]* Note: perhaps this could be combined w/another api.[[BR]]* The user executing this method must be a Spacewalk Admin.  |


|  org.trusts.listOrgs(string sessionKey)  |  0.4  |  2  |  bbuckingham  |  done  | (Multi Org Phase2 support)[[BR]]* List all organizations trusted by the user's organization.[[BR]]* Results similar to multiorg/Organizations from web interface.[[BR]]* User must be either Satellite or Org Admin. |
| --- | --- | --- | --- | --- | --- |
|  org.trusts.getDetails(string sessionKey, int trustedOrgId)  |  0.4  |  2  |  bbuckingham  |  done  | (Multi Org Phase2 support)[[BR]]* Results similar to multiorg/OrgTrustDetails from web interface.[[BR]]* Retrieve details on the trusted organization provided.[[BR]]* User must be either Satellite or Org Admin. |
|  org.trusts.listChannelsProvided(string sessionKey, int trustedOrgId)  |  0.4  |  2  |  bbuckingham  |  done  | (Multi Org Phase2 support)[[BR]]* List all software channels provided by the trusted organization.[[BR]]* Results similar to multiorg/channels/Provided from web interface.[[BR]]* User must be either Satellite or Org Admin. |
|  org.trusts.listChannelsConsumed(string sessionKey, int trustedOrgId)  |  0.4  |  2  |  bbuckingham  |  done  | (Multi Org Phase2 support)[[BR]]* List all software channels consumed by the trusted organization.[[BR]]* Results similar to multiorg/channels/Consumed from web interface.[[BR]]* User must be either Satellite or Org Admin. |


|   ----    |    |    |    |    |    |  |
| --- | --- | --- | --- | --- | --- |
|   *Namespace: packages*  |    |    |    |    |    |  |
|   packages.getDetails(*,downloadURL)    |  0.6  |    |    |    |    |  |
|   ----    |    |    |    |    |    |  |
|   *Namespace: schedule*  |    |    |    |    |    |  |
|   schedule.listAllActions(string sessionKey)  |  0.4  |  3  |  bbuckingham  |  done  |    |  |
|   schedule.cancelActions(string sessionKey, array[actionId](int))  |  0.4  |  3  |  bbuckingham  |  done  |    |  |
|   ----    |    |    |    |    |    |  |
|   *Namespace: supportuser*  |  0.6  |    |    |    |  TODO: A new API handler will be defined to support the [SupportUser](Features/SupportUser) feature.  Details will be coming as the content for Support user are finalized.  |
|   ----    |    |    |    |    |    |  |
|   *Namespace: system*  |    |    |    |    |    |  |
|   system.getConnectionPath(string sessionKey, int sid)  |  0.5  |  5  |  bbuckingham  |  done  |  Retrieve the list of proxies the system is connected to in order to reach the server.  |
|   system.getDetails(string sessionKey, int sid)  |  0.5  |  1  |  bbuckingham  |  done  |  Existing API to retrieve details on a system.  Add "boolean lock_status" to the return result.  |
|   system.getName(string sessionKey, int sid)  |  0.3  |  3  |  bbuckingham  |  done  |  Get system name and last check in information for the given system ID.  |
|   system.getRelevantErrataByType(string sessionKey, int sid, string advisoryType)  |  0.3  |  3  |  bbuckingham  |  done  |  Returns a list of all errata of the specified type that are relevant to the system.  |
|   system.listActivationKeys(string sessionKey, int systemId)  |  0.5  |  4  |  bbuckingham  |  done  |  Return the list of activation keys used during registration of the given system.  If no key was used, the list will be empty.  |
|   system.provisionSystem(sid, kslabel, date)  |  0.3  |  4  |  skarmark  |  done  |    |  |
|   system.provisionSystem(sid, kslabel)  |  0.3  |  1  |  skarmark  |  done  |    |  |
|   system.comparePackageProfile(string sessionKey, int sid, string profileLabel)  |  0.3  |  3  |  bbuckingham  |  done  |  Compare the system to an existing package profile.  |
|   system.schedulePackageRefresh(string sessionKey, int serverId, dateTime.iso860 earliestOccurrence)  |  0.3  |  4  |  bbuckingham  |  done  |  * Schedule a package list refresh for a system.[[BR]]* Returns the id of the scheduled action (int actionId), exception thrown otherwise.  |
|   system.schedulePackageRemove(string sessionKey, int serverId, array[packageId](int), dateTime.iso860 earliestOccurrence)  |  0.4  |  4  |  bbuckingham  |  done  |  * Schedule a package remove for a system.[[BR]]* Returns the id of the scheduled action (int actionId), exception thrown otherwise.  |
|   system.setBaseChannel(string sessionKey, int serverId, string channelLabel)  |  0.5  |  3  |  bbuckingham  |  done  |  * Set a system's base channel.  This will deprecate the existing setBaseChannel API that takes an int channelId as an input.   |
|   system.setChildChannels(string sessionKey, int serverId, array[channelId OR string channelLabel](int))  |  0.5  |  4  |  bbuckingham  |  done  |  * Set a system's child channels.  This is an existing API that takes an array[channelId](int) that has been enhanced to take an array[channelLabel](string) instead.  The intent is to deprecate the usage of array[channelId](int).  |
|   system.setLockStatus(string sessionKey, int serverId, boolean status)  |  0.5  |  3  |  bbuckingham  |  done  |  * Set a system's status to locked or unlocked.  Note: status=true indicates locked.   |
|   system.deleteCustomValues(string sessionKey, int serverId, array[customInfoLabel](string))  |  0.5  |  4  |  bbuckingham  |  done  |  * Delete one or more custom values from a system.   |
|   system.deleteNote(string sessionKey, int serverId, int noteId)  |  0.5  |  4  |  jdobies  |  done  |  Deletes a note from a server.  |
|   system.deleteNotes(string sessionKey, int serverId)  |  0.5  |  4  |  jdobies  |  done  |  Deletes all notes on a given server.  |
|   system.getDetails(string sessionKey, int sid)  |  0.6  |  1  |  jsherrill  |    |   Add last_boot  |






|   ----    |    |    |    |    |    |  |
| --- | --- | --- | --- | --- | --- | --- |
|   *Namespace: system.search*  |    |    |    |    |    |  |
|   system.search.nameAndDescription(string sessionKey, string searchTerm)  |  0.4  |  4  |  jmatthews  |  done  |  search for system by name and description   |
|   system.search.ip(string sessionKey, string searchTerm)  |  0.4  |  4  |  jmatthews  |  done  |  search for system by ip  |
|   system.search.hostname(string sessionKey, string searchTerm)  |  0.4  |  4  |  jmatthews  |  done  |  search for system by hostname  |
|   system.search.deviceVendorId(string sessionKey, string searchTerm)  |  0.4  |  4  |  jmatthews  |  done  |  search for a system by vendor id of a hardware device  |
|   system.search.deviceId(string sessionKey, string searchTerm)  |  0.4  |  4  |  jmatthews  |  done  |  search for a system by hardware device id  |
|   system.search.deviceDriver(string sessionKey, string searchTerm)  |  0.4  |  4  |  jmatthews  |  done  |  search for a system by hardware device driver  |
|   system.search.deviceDescription(string sessionKey, string searchTerm)  |  0.4  |  4  |  jmatthews  |  done  |  search for a system by hardware device description  |
|   ----    |    |    |    |    |    |  |
|   *Namespace: system.custominfo*  |    |    |    |    |    |  |
|   system.custominfo.createKey(string sessionKey, string keyLabel, string keyDescription)  |  0.3  |  3  |  coec  |  done   |  This API was originally implemented as system.createCustomValueKey; however, it was renamed and moved the custominfo namespace as additional APIs were added that namespace.  |
|   system.custominfo.deleteKey(string sessionKey, string keyLabel)  |  0.3  |  3  |  bbuckingham  |  done   |    |  |
|   system.custominfo.listAllKeys(string sessionKey)  |  0.3  |  3  |  bbuckingham  |  done   |    |  |
|   *Namespace: satellite*  |    |    |    |    |    |  |
|   satellite.getCertificateExpirationDate(string sessionKey)  |  0.4  |  3  |  jsherrill  |  done   |    |  |
### New Spacewalk 0.1 APIs



Below are APIs that have been included in Spacewalk 0.1, but that were not available for the Satellite 5.1 product; however, they will be incorporated in to a future Satellite release.


|   *Task*  |  *Target Release*  |   *Estimate (hours)*  |  *Developer*  |  *Status*  |  *Notes*  |
| --- | --- | --- | --- | --- | --- | --- |
|   ----    |    |    |    |    |    |  |
|   *Namespace: kickstart*  |    |    |    |    |    |  |
|   kickstart.list  |  0.1  |  2  |  jsherrill  |  Done  |    |  |
|   kickstart.listScripts()  |  0.1  |  2  |  jsherrill  |  Done  |  moved to kickstart.profile in 0.3  |
|   kickstart.addScript()  |  0.1  |  3  |  jsherrill  |  Done  |  moved to kickstart.profile in 0.3  |
|   kickstart.removeScript()  |  0.1  |  3  |  jsherrill  |  Done  |  moved to kickstart.profile in 0.3  |
|   kickstart.downloadKickstart()  |  0.1  |  3  |  jsherrill  |  Done  |  moved to kickstart.profile in 0.3  |
|   kickstart.setKickstartTree(kslabel, tree_label)  |  0.1  |  3  |  skarmark  |  Done  |  moved to kickstart.profile in 0.3  |
|   kickstart.setChildChannels(kslabel, array(chan_lablel))  |  0.1  |  3  |  skarmark  |  Done  |  moved to kickstart.profile in 0.3  |
|   kickstart.getPartioningScheme()  |  0.1  |  2  |  jortel  |  Done  |  moved to kickstart.profile in 0.3  |
|   kickstart.setPartitioningScheme()  |  0.1  |  3  |  jortel  |  Done  |  moved to kickstart.profile in 0.3  |
|   ----    |    |    |    |    |    |  |
|   *Namespace: org*  |    |    |    |    |    |  |
|   org.create  |  0.1  |   |  dgoodwin  |  Done  |   |  |
|   org.delete  |  0.1  |   |  dgoodwin  |  Done  |   |  |
|   org.listOrgs  |  0.1  |   |  partha  |  Done  |   |  |


|   org.listUsers  |  0.1  |   |  partha  |  Done  |   |  |
| --- | --- | --- | --- | --- | --- | --- |
|   org.getDetails(orgName)  |  0.1  |   |  partha  |  Done  |   |  |
|   org.getDetails(orgId)  |  0.1  |   |  partha  |  Done  |   |  |
|   org.updateName  |  0.1  |   |  partha  |  Done  |   |  |
|   org.listSoftwareEntitlements  |  0.1  |   |  dgoodwin  |  Done  |   |  |
|   org.listSoftwareEntitlementsForOrg  |  0.1  |   |  dgoodwin  |  Done  |   |  |
|   org.setSoftwareEntitlements  |  0.1  |   |  dgoodwin  |  Done  |   |  |
|   org.listSystemEntitlements  |  0.1  |   |  partha  |  Done  |   |  |
|   org.listSystemEntitlementsForOrg  |  0.1  |   |  partha  |  Done  |   |  |
|   org.setSystemEntitlements  |  0.1  |   |  dgoodwin  |  Done  |   |  |
|   ----    |    |    |    |    |    |  |
|   *Namespace: schedule*  |    |    |    |    |    |  |
|   schedule.listCompletedActions(sessionKey)  |  0.1  |  2  |  bbuckingham  |  Done  |    |  |
|   schedule.listInProgressActions(sessionKey)  |  0.1  |  2  |  bbuckingham  |  Done  |    |  |
|   schedule.listFailedActions(sessionKey)  |  0.1  |  2  |  bbuckingham  |  Done  |    |  |
|   schedule.listArchivedActions(sessionKey)  |  0.1  |  2  |  bbuckingham  |  Done  |    |  |
|   schedule.listCompletedSystems(sessionKey, aid)  |  0.1  |  2  |  bbuckingham  |  Done  |    |  |
|   schedule.listInProgressSystems(sessionKey, aid)  |  0.1  |  2  |  bbuckingham  |  Done  |    |  |
|   schedule.listFailedSystems(sessionKey, aid)  |  0.1  |  2  |  bbuckingham  |  Done  |    |  |
|   ----    |    |    |    |    |    |  |
|   *Namespace: system*  |    |    |    |    |    |  |
|   system.listInactiveSystems(date)  |  0.1  |  2  |  jsherrill  |  Done  |    |  |
|   system.whoRegistered(sid)  |  0.1  |  1  |  jsherrill  |  Done  |    |  |
|   system.listSystemsWithPackage(id)  |  0.1  |  3  |  jsherrill  |  Done  |    |  |
|   system.listSystemsWithPackage(name, version, release, epoch)  |  0.1  |  3  |  jsherrill  |  Done  |    |  |
|   system.listVirtHosts()  |  0.1  |  2  |  jsherrill  |  Done  |    |  |
|   system.listVirtGuests(sid)  |  0.1  |  2  |  jsherrill  |  Done  |    |  |
|   system.setGuestMemory(sid, memory)  |  0.1  |  2  |  jsherrill  |  Done  |    |  |
|   system.setGuestCpus(sid, cpus)  |  0.1  |  2  |  jsherrill  |  Done  |    |  |
|   system.scheduleGuestAction(sid, state)  |  0.1  |  4  |  jsherrill  |  Done  |    |  |
|   system.deleteSnapshot(sid, snapId)  |  0.1  |  5  |  jsherril  |  Done  |    |  |
|   system.deleteSnapshots(sid)  |  0.1  |  5  |  jsherrill  |  Done  |    |  |
|   system.listSnapshots(sid)  |  0.1  |  5  |  jsherrill  |  Done  |    |  |
|   system.listSnapshotPackages(snapid)  |  0.1  |  5  |  jsherrill  |  Done  |    |  |
|   system.listSnapshotConfigFiles(snapid)  |  0.1  |  5  |  jsherrill  |  Done  |    |  |
|   system.getOsadStatus(sid) (system.getDetails())  |  0.1  |  2  |  jsherrill  |  Done  |    |  |
|   ----    |    |    |    |    |    |  |
|   *Namespace: system.config*  |    |    |    |    |    |  |
|  system.config.scheduleImport([[sids]], [[files]]) |  0.6  |  5  |  paji  |   |    |  |
|   ----    |    |    |    |    |    |  |
|   *Namespace: systemgroup*  |    |    |    |    |    |  |
|   systemgroup.listActiveSystemsInGroup(group)  |  0.1  |  3  |  jsherrill  |  Done  |    |  |
|   systemgroup.listInActiveSystemsInGroup(group)  |  0.1  |  3  |  jsherrill  |  Done  |    |  |
## QA Notes

### New API Calls Introduced

 

||  *API name* || *Simple Testcase* || *Comprehensive Testcase* || *Notes* ||  *Assigned To* ||
||   ||   ||   ||   ||   ||||
||  activationkey.addPackages  || Tested ||    ||    ||    || 
||  activationkey.checkConfigDeployment  || Tested ||    ||    ||    || 
||  activationkey.disableConfigDeployment  || Tested ||    ||    ||    || 
||  activationkey.enableConfigDeployment  || Tested ||    ||    ||    || 
||  activationkey.removePackages  || Tested ||    ||    ||    || 
||  auth.checkAuthToken  || N/A - Unpublished ||    ||  test was commented out, reason being method isn't published  ||ssalevan|| 
||  channel.listAllChannels  || Tested ||    ||    ||    || 
||  channel.listMyChannels  || Tested ||    ||    ||    || 
||  channel.listPopularChannels  || Tested ||    ||    ||    || 
||  channel.listRedHatChannels  || Tested ||    ||    ||    || 
||  channel.listRetiredChannels  || Tested ||    ||    ||    || 
||  channel.listSharedChannels  || Tested ||    ||    ||    || 
||  channel.access.disableUserRestrictions  || Tested ||    ||    ||    || 
||  channel.access.enableUserRestrictions  || Tested ||    ||    ||    || 
||  channel.access.getOrgSharing  || Tested ||    ||    ||    || 
||  channel.access.setOrgSharing  || Tested ||    ||    ||    || 
||  channel.software.listErrataByType  || Tested ||    ||    ||    || 
||  channel.software.mergeErrata  || Tested ||    ||    ||    || 
||  channel.software.mergeErrata  || Tested ||    ||    ||    || 
||  channel.software.setContactDetails  || Tested ||    ||    ||    || 
||  configchannel.channelExists  || Tested ||    ||    ||    || 
||  configchannel.scheduleFileComparisons  || Tested ||    ||    ||    || 
||  configchannel.deleteFiles String String List  || Tested ||    ||    || skarmark ||
||  configchannel.lookupChannelInfo String List || Tested ||    ||    || skarmark ||
||  configchannel.lookupFileInfo String String List  || Tested ||    ||    || skarmark ||
||  errata.addPackages  || Tested ||    ||    ||skarmark|| 
||  errata.create String Map List List List Boolean List || Tested ||    ||    || skarmark || 
||  errata.findByCve  || Tested ||    ||    ||skarmark|| 
||  errata.removePackages  || Tested ||    ||    ||skarmark|| 
||  errata.setDetails  || Tested ||    ||    ||skarmark|| 
||  kickstart.deleteProfile  || Tested ||    ||    ||    || 
||  kickstart.findKickstartForIp  || Tested ||    ||    ||    || 
||  kickstart.importRawFile String String String String String || Tested ||    ||    || skarmark || 
||  kickstart.listAllIpRanges  || Tested ||    ||    ||    || 
||  kickstart.listKickstarts  || Tested ||    ||    ||    || 
||  kickstart.renameProfile  String String String || Tested ||    ||    || skarmark || 
||  kickstart.filepreservation.create  || Tested ||    ||    ||    || 
||  kickstart.filepreservation.delete  || Tested ||    ||    ||    || 
||  kickstart.filepreservation.getDetails  || Tested ||    ||    ||    || 
||  kickstart.filepreservation.listAllFilePreservations  || Tested ||    ||    ||    || 
||  kickstart.keys.create  || Tested ||    ||    ||    || 
||  kickstart.keys.delete  || Tested ||    ||    ||    || 
||  kickstart.keys.getDetails  || Tested ||    ||    ||    || 
||  kickstart.keys.listAllKeys  || Tested ||    ||    ||    || 
||  kickstart.profile.addIpRange  || Tested ||    ||    ||skarmark|| 
||  kickstart.profile.addScript  || Tested ||    ||    ||skarmark|| 
||  kickstart.profile.compareActivationKeys  || Tested ||    ||    ||skarmark|| 
||  kickstart.profile.compareAdvancedOptions  || Tested ||    ||    ||skarmark|| 
||  kickstart.profile.comparePackages  || Tested ||    ||    ||skarmark|| 
||  kickstart.profile.downloadKickstart  || Tested ||    ||    ||skarmark|| 
||  kickstart.profile.getAdvancedOptions  || Tested ||    ||    ||skarmark|| 
||  kickstart.profile.getCustomOptions  || Tested ||    ||    ||skarmark|| 
||  kickstart.profile.listIpRanges  || Tested ||    ||    ||skarmark|| 
||  kickstart.profile.listScripts  || Tested ||    ||    ||skarmark|| 
||  kickstart.profile.removeIpRange  || Tested ||    ||    ||skarmark|| 
||  kickstart.profile.removeScript  || Tested ||    ||    ||skarmark|| 
||  kickstart.profile.setAdvancedOptions  || Tested ||    ||    ||skarmark|| 
||  kickstart.profile.setChildChannels  || Tested ||    ||    ||skarmark|| 
||  kickstart.profile.setCustomOptions  || Tested ||    ||    ||skarmark|| 
||  kickstart.profile.setKickstartTree  || Tested ||    ||    ||skarmark|| 
||  kickstart.profile.keys.addActivationKey  || Tested ||    ||    ||    || 
||  kickstart.profile.keys.getActivationKeys  || Tested ||    ||    ||    || 
||  kickstart.profile.keys.removeActivationKey  || Tested ||    ||    ||    || 
||  kickstart.profile.software.appendToSoftwareList  || Tested ||    ||    ||    || 
||  kickstart.profile.software.getSoftwareList  || Tested ||    ||    ||    || 
||  kickstart.profile.software.setSoftwareList  || Tested ||    ||    ||    || 
||  kickstart.profile.system.addFilePreservations  || Tested ||    ||    ||skarmark|| 
||  kickstart.profile.system.addKeys  || Tested ||    ||    ||skarmark|| 
||  kickstart.profile.system.disableConfigManagement  || Tested ||    || kickstart.profile.system.checkConfigManagement was added to allow testing ||skarmark|| 
||  kickstart.profile.system.disableRemoteCommands  || Tested ||    ||  ||skarmark|| 
||  kickstart.profile.system.enableConfigManagement  || Tested||    ||  ||skarmark|| 
||  kickstart.profile.system.enableRemoteCommands  || Tested ||    ||  ||skarmark|| 
||  kickstart.profile.system.getLocale  || Tested ||    ||    ||skarmark|| 
||  kickstart.profile.system.getPartitioningScheme  || Tested ||    ||    ||skarmark|| 
||  kickstart.profile.system.listFilePreservations  || Tested ||    ||    ||skarmark|| 
||  kickstart.profile.system.listKeys  || Tested ||    ||    ||skarmark|| 
||  kickstart.profile.system.removeFilePreservations  || Tested ||    ||    ||skarmark|| 
||  kickstart.profile.system.removeKeys  || Tested ||    ||    ||skarmark|| 
||  kickstart.profile.system.setLocale  || Tested ||    ||    ||skarmark|| 
||  kickstart.profile.system.setNetworkConnection  || Tested ||    ||  ||skarmark|| 
||  kickstart.profile.system.setPartitioningScheme  || Tested ||    ||    ||skarmark|| 
||  kickstart.profile.system.setSELinux  || Tested ||    || kickstart.profile.system.getSELinux was added to allow test  ||skarmark|| 
||  kickstart.tree.create  || Tested ||    ||    || skarmark || 
||  kickstart.tree.delete  || Tested ||    ||    || skarmark || 
||  kickstart.tree.deleteTreeAndProfiles  || Tested ||    ||    || skarmark || 
||  kickstart.tree.list  || Tested ||    ||    || skarmark || 
||  kickstart.tree.listInstallTypes  || Tested ||    ||    || skarmark || 
||  kickstart.tree.rename  || Tested ||    ||    || skarmark || 
||  kickstart.tree.update  || Tested ||    ||    || skarmark || 
||  org.create  || Tested ||    ||    ||    || 
||  org.delete  || Tested ||    ||    ||    || 
||  org.getDetails  || Tested ||    ||    ||    || 
||  org.getDetails  || Tested ||    ||    ||    || 
||  org.listOrgs  || Tested ||    ||    ||    || 
||  org.listSoftwareEntitlements  || Tested ||    ||    ||    || 
||  org.listSoftwareEntitlements  || Tested ||    ||    ||    || 
||  org.listSoftwareEntitlementsForOrg  || Tested ||    ||    ||    || 
||  org.listSystemEntitlements  || Tested ||    ||    ||    || 
||  org.listSystemEntitlements  || Tested ||    ||    ||    || 
||  org.listSystemEntitlementsForOrg  || Tested ||    ||    ||    || 
||  org.listUsers  || Tested ||    ||    ||    || 
||  org.migrateSystems  || Tested ||    ||    ||    || 
||  org.setSoftwareEntitlements  || Tested ||    ||    ||    || 
||  org.setSystemEntitlements  || Tested ||    ||    ||    || 
||  org.updateName  || Tested ||    ||    ||    || 
||  org.trusts.addTrust  || Tested ||    ||    ||    || 
||  org.trusts.getDetails  || Tested ||    ||    ||    || 
||  org.trusts.listChannelsConsumed  || Tested ||    ||    ||    || 
||  org.trusts.listChannelsProvided  || Tested ||    ||    ||    || 
||  org.trusts.listOrgs  || Tested ||    ||    ||    || 
||  org.trusts.listSystemsAffected  || Tested ||    ||    ||    || 
||  org.trusts.listTrusts  || Tested ||    ||    ||    || 
||  org.trusts.removeTrust  || Tested ||    ||    ||    || 
||  packages.getPackage  || Tested ||    ||    ||    || 
||  packages.getPackageUrl  || Tested ||    ||    ||    || 
||  packages.provider.associateKey  || Tested ||    ||    ||    || 
||  packages.provider.list  || Tested ||    ||    ||    || 
||  packages.provider.listKeys  || Tested ||    ||    ||    || 
||  proxy.isProxy  || Tested ||    ||    ||ssalevan|| 
||  satellite.getCertificateExpirationDate  || Tested ||    ||    ||    || 
||  schedule.cancelActions  || Tested ||    ||    ||    || 
||  schedule.listAllActions  || Tested ||    ||    ||    || 
||  schedule.listArchivedActions  || Tested ||    ||    ||    || 
||  schedule.listCompletedActions  || Tested ||    ||    ||    || 
||  schedule.listCompletedSystems  || Recheck - call params don't match definition ||    || docs need update   ||    || 
||  schedule.listFailedActions  || Tested ||    ||    ||    || 
||  schedule.listFailedSystems  || Recheck - call params don't match definition ||    ||  docs need update  ||    || 
||  schedule.listInProgressActions  || Tested ||    ||    ||    || 
||  schedule.listInProgressSystems  || Recheck - call params don't match definition ||    ||  docs need update  ||    || 
||  system.comparePackageProfile  || Tested ||    ||    ||    || 
||  system.deleteCustomValues String Integer List || Tested ||    ||    || skarmark || 
||  system.deleteSnapshot  || Tested ||    ||    ||    || 
||  system.deleteSnapshots  || Tested ||    ||    ||    || 
||  system.getName  || Tested ||    ||    ||    || 
||  system.getRelevantErrataByType  || Tested ||    ||    ||    || 
||  system.listInactiveSystems  || Tested ||    ||    ||    || 
||  system.listInactiveSystems  || Tested ||    ||    ||    || 
||  system.listNotes  || Tested ||    ||    ||    || 
||  system.listSnapshotConfigFiles  || Tested ||    ||    || skarmark || 
||  system.listSnapshotPackages  || Tested ||    ||    ||    || 
||  system.listSnapshots  || Tested ||    ||    ||    || 
||  system.listSubscribableBaseChannels String Integer  || Tested ||    ||    || skarmark || 
||  system.listSystemsWithPackage  || Tested ||    ||    ||    || 
||  system.listSystemsWithPackage  || Tested ||    ||    ||    || 
||  system.listVirtualGuests  || Tested ||    ||    ||    || 
||  system.listVirtualHosts  || Tested ||    ||    ||    || 
||  system.provisionSystem  || Tested ||    ||    ||    || 
||  system.provisionSystem  || Tested ||    ||    ||    || 
||  system.scheduleGuestAction  || Tested ||    ||    ||    || 
||  system.scheduleGuestAction  || Tested ||    ||    ||    || 
||  system.schedulePackageRefresh  || Tested ||    ||    ||    || 
||  system.schedulePackageRemove  || Tested ||    ||    ||    || 
||  system.setGuestCpus  || Tested ||    ||    ||    || 
||  system.setGuestMemory  || Tested ||    ||    ||    || 
||  system.setLockStatus String Integer Boolean  || Tested ||    ||    ||  jmatthews  ||
||  system.whoRegistered  || Tested ||    ||    ||    || 
||  system.config.createOrUpdatePath String Integer Boolean String Map Integer   || Tested ||    ||    || skarmark ||
||  system.config.deleteFiles String Integer List Boolean  || Tested ||    ||    || skarmark ||
||  system.config.deployAll String List Date  || Tested ||    || Not able to check fully, no way to enable config management on a system through api. Minimal checking added  || skarmark ||
||  system.config.lookupFileInfo String Integer List Integer  || Tested ||    ||    || skarmark ||
||  system.config.removeChannels String List List || Tested ||    ||    || skarmark ||
||  system.config.setChannels String List List || Tested ||    ||    || skarmark ||
||  system.custominfo.createKey  || Tested ||    ||    ||    || 
||  system.custominfo.deleteKey  || Tested ||    ||    ||    || 
||  system.custominfo.listAllKeys  || Tested ||    ||    ||    || 
||  system.search.deviceDescription  || Tested ||    ||    ||    || 
||  system.search.deviceDriver  || Tested ||    ||    ||    || 
||  system.search.deviceId  || Missing ||    || not in api tests - unsure how to add test data   ||    || 
||  system.search.deviceVendorId  || Missing ||  || not in api tests - unsure how to add test data   ||    || 
||  system.search.hostname  || Tested ||    ||    ||    || 
||  system.search.ip  || Tested ||    ||    ||    || 
||  system.search.nameAndDescription  || Tested ||    ||    ||    || 
||  systemgroup.listActiveSystemsInGroup  || Tested ||    ||    ||    || 
||  systemgroup.listInactiveSystemsInGroup  String String Integer || Tested ||    ||    ||    || 
||  systemgroup.listInactiveSystemsInGroup  String String || Tested ||    ||    || skarmark || 