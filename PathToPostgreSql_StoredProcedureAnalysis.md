# Stored Procedure Analysis



 * Quantitatively speaking we need to port the following components to Postgres.


|  Component  |  Number  |  Queried Method  |
| --- | --- | --- |
|  Tables   |  405  |  select count(distinct TABLE_NAME) from ALL_TABLES  where owner='SPACEWALK';  |
|  VIews   |  60  |   select count(distinct VIEW_NAME) from ALL_VIEWS  where owner='SPACEWALK';  |
|  Constraints   |  2675  |    select count(distinct CONSTRAINT_NAME) from  ALL_CONSTRAINTS where owner='SPACEWALK';  |
|  Triggers   |  209  |  select count(distinct trigger_name) from all_triggers where OWNER='SPACEWALK';  |
|  Indexes  |  776  |   select count(distinct Index_name) from all_indexes where OWNER='SPACEWALK';  |
|   Procedures and Functions  |  38  |  See below  |
|  Packages Functions  |  90  |  See below  |



From fervent grepping through /eng/web, /eng/backend, /eng/java, /eng/client/tools, /eng/schema/views, /eng/schema/tables, /eng/schema/synonyms and mapping that with `select distinct name, type  from all_source where owner='SPACEWALK'` the following list was obtained. Basically this is the list of procedures that will likely need to be ported for PostgreSQL or replaced. (NOTE: keep an eye out still for procedures called by dead code, these can still be carefully deleted)



| Stored Procedures to Port/Replace |
| --- |
| create_new_user |
| create_pxt_session |
| id_join |
| label_join |
| lookup_arch_type |
| lookup_cf_state |
| lookup_channel_arch |
| lookup_client_capability |
| lookup_config_filename |
| lookup_config_info |
| lookup_evr |
| lookup_feature_type |
| lookup_first_matching_cf |
| lookup_package_arch |
| lookup_package_capability |
| lookup_package_name |
| lookup_package_nevra |
| lookup_server_arch |
| lookup_sg_type |
| lookup_snapshot_invalid_reason |
| lookup_tag |
| lookup_transaction_package |
| lookup_virt_sub_level |
| name_join |
| rhnhistoryview_erratalist |
| rhnhistoryview_pkglist |
| create_first_org |
| create_new_org |


| delete_channel |
| --- |
| delete_errata |
| delete_server |
| delete_server_bulk |
| pxt_session_cleanup |
| queue_errata |
| queue_server |
| rhn_install_org_satellites |
| rhn_synch_probe_state |
| set_ks_session_history_message |
| rhn_entitlements.modify_org_service |
| rhn_channel.subscribe_server |
| rhn_org.delete_user |
| rhn_entitlements.repoll_virt_guest_entitlements |
| rhn_channel.unsubscribe_server |
| rhn_server.insert_into_servergroup |
| rhn_server.snapshot_server |
| rhn_entitlements.assign_system_entitlement |
| rhn_server.delete_from_servergroup |
| rhn_channel.channel_priority |
| rhn_entitlements.remove_server_entitlement |


| rhn_user.find_mailable_address |
| --- |
| rhn_config.delete_revision |
| rhn_entitlements.set_family_count |
| rhn_server.remove_action |
| rhn_entitlements.assign_channel_entitlement |
| rhn_entitlements.entitle_server |
| rhn_entitlements.can_entitle_server |
| rhn_config_channel.get_user_chan_access |
| rhn_config_channel.action_diff_revision_status |
| rhn_channel.bulk_server_basechange_from |
| rhn_org.delete_org |
| rhn_channel.user_role_check_debug |
| rhn_entitlements.activate_system_entitlement |
| rhn_entitlements.unentitle_server |
| rhn_entitlements.activate_channel_entitlement |
| rhn_server.bulk_snapshot |
| rhn_entitlements.subscribe_newest_servers |
| rhn_server.bulk_snapshot_tag |
| rpm.vercmp |
| rhn_config.insert_revision |
| rhn_channel.entitle_customer |
| rhn_server.check_user_access |
| rhn_channel.update_channel |
| rhn_entitlements.get_server_entitlement |
| rhn_server.set_custom_value |
| rhn_entitlements.entitlement_grants_service |
| rhn_server.insert_set_into_servergroup |
| rhn_server.insert_into_servergroup_maybe |
| rhn_config.insert_channel |
| rhn_channel.bulk_guess_server_base_from |
| rhn_server.tag_delete |
| rhn_exception.raise_exception |
| rhn_server.delete_set_from_servergroup |
| rhn_cache.update_perms_for_user |
| rhn_entitlements.can_switch_base |
| rhn_user.remove_from_usergroup |
| rhn_user.add_servergroup_perm |
| rhn_server.get_ip_address |
| rhn_config.insert_file |
| rhn_channel.bulk_guess_server_base |
| rhn_channel.get_org_access |
| rhn_channel.base_channel_for_release_arch |
| rhn_config.delete_file |
| rhn_server.bulk_set_custom_value |
| rhn_channel.org_channel_setting |
| rhn_user.check_role |
| rhn_user.remove_servergroup_perm |
| rhn_config_channel.get_user_revision_access |
| rhn_user.add_to_usergroup |
| rhn_channel.guess_server_base |
| rhn_channel.refresh_newest_package |
| rhn_channel.license_consent |
| rhn_channel.get_license_path |
| rhn_config_channel.get_user_file_access |
| rhn_server.system_service_level |
| rhn_package.canonical_name |
| rhn_channel.user_role_check |
| rhn_channel.normalize_server_arch |
| rhn_channel.get_cfam_org_access |
| rhn_user.add_users_to_usergroups |
| rhn_user.remove_users_from_servergroups |
| rhn_exception.raise_exception_val |
| rhn_channel.available_chan_subscriptions |
| rhn_config.delete_channel |
| rhn_server.clear_servergroup |
| rhn_channel.bulk_server_base_change |
| rpm.isalpha |
| rhn_server.tag_snapshot |
| rhn_bel.lookup_email_state |
| rhn_bel.is_org_paid |
| rhn_cache.update_perms_for_server |
| rhn_channel.update_family_counts |
| rhn_channel.get_org_id |
| rhn_user.get_org_id |
| rpm.isdigit |
| rhn_channel.loose_user_role_check |
| rhn_channel.bulk_unsubscribe_server |
| rhn_channel.bulk_subscribe_server |
| rhn_entitlements.set_customer_nonlinux |


Total number of stored procedures to convert 39 (stand alone functions or procedures) + (90 functions or procedures in packages) = 129

Most expensive stored procedures to port from Oracle to PostgreSQL


|  Package Name  |  Procedure Name  |  Number of lines |
| --- | --- | --- |
|  RPM  |  rpmstrcmp  | 132  |
|  RHN_ENTITLEMENTS  |  modify_org_service  | 127  |
|  RHN_CHANNEL  |  subscribe_server  | 120  |
|  RHN_ORG  |  delete_user  | 119  |
|  RHN_ENTITLEMENTS  |  repoll_virt_guest_entitlements  | 109  |
|  RHN_CHANNEL  |  unsubscribe_server  | 100  |
|  RHN_SERVER  |  insert_into_servergroup  | 95  |
|  RHN_SERVER  |  snapshot_server  | 89  |
|  RHN_ENTITLEMENTS  |  assign_system_entitlement  | 85  |
|  RHN_ENTITLEMENTS  |  prune_group  | 81  |
|  RHN_SERVER  |  delete_from_servergroup  | 78  |
|  RHN_CHANNEL  |  channel_priority  | 77  |
|  RHN_ENTITLEMENTS  |  remove_server_entitlement  | 73  |
|  RHN_USER  |  find_mailable_address  | 71  |
|  RHN_CONFIG  |  delete_revision  | 70  |
|  RHN_ENTITLEMENTS  |  set_family_count  | 63  |
|  RHN_SERVER  |  remove_action  | 62  |
|  RHN_ENTITLEMENTS  |  assign_channel_entitlement  | 62  |
|  RHN_ENTITLEMENTS  |  set_group_count  | 58  |
|  RHN_ENTITLEMENTS  |  prune_family  | 56  |
|  RHN_ENTITLEMENTS  |  entitle_server  | 55  |
|  RHN_ENTITLEMENTS  |  can_entitle_server  | 55  |
|  RHN_CONFIG_CHANNEL  |  get_user_chan_access  | 53  |
|  RHN_CONFIG_CHANNEL  |  action_diff_revision_status  | 50  |
|  RHN_ENTITLEMENTS  |  prune_everything  | 50  |
|  RHN_CHANNEL  |  bulk_server_basechange_from  | 46  |
|  RHN_CHANNEL  |  available_family_subscriptions  | 46  |
|  RHN_ORG  |  delete_org  | 45  |
|  RHN_CHANNEL  |  user_role_check_debug  | 44  |
|  RHN_ENTITLEMENTS  |  activate_system_entitlement  | 41  |
|  RHN_ENTITLEMENTS  |  unentitle_server  | 41  |
|  RHN_CHANNEL  |  base_channel_rel_archid  | 40  |
|  RHN_ENTITLEMENTS  |  activate_channel_entitlement  | 40  |
|  RHN_CHANNEL  |  delete_server_channels  | 38  |
|  RHN_SERVER  |  bulk_snapshot  | 37  |
|  RHN_ENTITLEMENTS  |  subscribe_newest_servers  | 36  |
|  RHN_ENTITLEMENTS  |  entitle_last_modified_servers  | 36  |
|  RHN_ENTITLEMENTS  |  remove_org_entitlements  | 35  |
|  RHN_SERVER  |  bulk_snapshot_tag  | 31  |
|  RPM  |  vercmp  | 30  |
|  RHN_CONFIG  |  insert_revision  | 30  |
|  RHN_ENTITLEMENTS  |  find_compatible_sg  | 29  |
|  RHN_CHANNEL  |  can_server_consume_virt_channl  | 29  |
|  RHN_CHANNEL  |  entitle_customer  | 28  |
|  RHN_CHANNEL  |  set_family_maxmembers  | 27  |
|  RHN_SERVER  |  check_user_access  | 27  |
|  RHN_CHANNEL  |  update_channel  | 27  |
|  RHN_ENTITLEMENTS  |  get_server_entitlement  | 26  |
|  RHN_SERVER  |  set_custom_value  | 26  |
|  RHN_ENTITLEMENTS  |  entitlement_grants_service  | 26  |
|  RHN_SERVER  |  insert_set_into_servergroup  | 26  |
|  RHN_SERVER  |  insert_into_servergroup_maybe  | 25  |
|  RHN_CONFIG  |  insert_channel  | 25  |
|  RHN_SERVER  |  can_server_consume_virt_slot  | 25  |
|  RHN_CHANNEL  |  bulk_guess_server_base_from  | 25  |
|  RHN_SERVER  |  tag_delete  | 24  |
|  RHN_QUOTA  |  recompute_org_quota_used  | 24  |
|  RHN_EXCEPTION  |  raise_exception  | 24  |
|  RHN_SERVER  |  delete_set_from_servergroup  | 23  |
|  RHN_CACHE  |  update_perms_for_server_group  | 23  |
|  RHN_CACHE  |  update_perms_for_user  | 23  |
|  RHN_CHANNEL  |  channel_family_current_members  | 23  |
|  RHN_ENTITLEMENTS  |  can_switch_base  | 23  |
|  RHN_USER  |  remove_from_usergroup  | 23  |
|  RHN_USER  |  add_servergroup_perm  | 21  |
|  RHN_SERVER  |  get_ip_address  | 21  |
|  RHN_CONFIG  |  insert_file  | 20  |
|  RHN_CHANNEL  |  bulk_guess_server_base  | 20  |
|  RHN_CHANNEL  |  get_org_access  | 20  |
|  RHN_CHANNEL  |  base_channel_for_release_arch  | 20  |
|  RHN_QUOTA  |  set_org_quota_total  | 20  |
|  RHN_ENTITLEMENTS  |  lookup_entitlement_group  | 19  |
|  RHN_CONFIG  |  delete_file  | 19  |
|  RHN_ENTITLEMENTS  |  create_entitlement_group  | 19  |
|  RHN_SERVER  |  bulk_set_custom_value  | 19  |
|  RHN_CHANNEL  |  direct_user_role_check  | 19  |
|  RHN_CHANNEL  |  org_channel_setting  | 19  |
|  RHN_USER  |  check_role_implied  | 18  |
|  RHN_USER  |  check_role  | 18  |
|  RHN_USER  |  remove_servergroup_perm  | 18  |
|  RHN_CONFIG_CHANNEL  |  get_user_revision_access  | 18  |
|  RHN_USER  |  add_to_usergroup  | 18  |
|  RHN_CHANNEL  |  guess_server_base  | 17  |
|  RHN_DATE_MANIP  |  get_reporting_period_end  | 17  |
|  RHN_CHANNEL  |  refresh_newest_package  | 17  |
|  RHN_CHANNEL  |  license_consent  | 17  |
|  RHN_DATE_MANIP  |  get_reporting_period_start  | 17  |
|  RHN_CHANNEL  |  get_license_path  | 16  |
|  RHN_CHANNEL  |  clear_subscriptions  | 16  |
|  RHN_CONFIG_CHANNEL  |  get_user_file_access  | 16  |
|  RHN_PACKAGE  |  channel_occupancy_string  | 16  |
|  RHN_SERVER  |  system_service_level  | 16  |
|  RHN_PACKAGE  |  canonical_name  | 15  |
|  RHN_EXCEPTION  |  lookup_exception  | 15  |
|  RHN_SERVER  |  can_change_base_channel  | 15  |
|  RHN_CHANNEL  |  user_role_check  | 15  |
|  RHN_QUOTA  |  get_org_for_config_content  | 14  |
|  RHN_CHANNEL  |  family_for_channel  | 14  |
|  RHN_CHANNEL  |  normalize_server_arch  | 14  |
|  RHN_CHANNEL  |  get_cfam_org_access  | 14  |
|  RHN_SERVER  |  delete_from_org_servergroups  | 14  |
|  RHN_CONFIG  |  get_latest_revision  | 13  |
|  RHN_USER  |  add_users_to_usergroups  | 13  |
|  RHN_USER  |  remove_users_from_servergroups  | 13  |
|  RHN_EXCEPTION  |  raise_exception_val  | 12  |
|  RHN_CHANNEL  |  available_chan_subscriptions  | 12  |
|  RHN_CONFIG  |  delete_channel  | 12  |
|  RHN_SERVER  |  clear_servergroup  | 11  |
|  RHN_CHANNEL  |  unsubscribe_server_from_family  | 11  |
|  RHN_CHANNEL  |  update_channels_by_package  | 11  |
|  RHN_CHANNEL  |  update_channels_by_errata  | 11  |
|  RPM  |  isalphanum  | 11  |
|  RHN_CHANNEL  |  bulk_server_base_change  | 11  |
|  RPM  |  isalpha  | 10  |
|  RHN_SERVER  |  tag_snapshot  | 10  |
|  RHN_BEL  |  lookup_email_state  | 10  |
|  RHN_BEL  |  is_org_paid  | 10  |
|  RHN_ORG  |  find_server_group_by_type  | 10  |
|  RHN_CACHE  |  update_perms_for_server  | 10  |
|  RHN_CHANNEL  |  update_family_counts  | 10  |
|  RHN_CHANNEL  |  get_org_id  | 9  |
|  RHN_USER  |  get_org_id  | 9  |
|  RPM  |  isdigit  | 9  |
|  RHN_CHANNEL  |  loose_user_role_check  | 8  |
|  RPM  |  vercmpResetCounter  | 8  |
|  RHN_QUOTA  |  update_org_quota  | 7  |
|  RHN_CHANNEL  |  bulk_unsubscribe_server  | 7  |
|  RHN_CHANNEL  |  bulk_subscribe_server  | 7  |
|  RHN_CONFIG  |  prune_org_configs  | 6  |
|  RHN_ENTITLEMENTS  |  unset_customer_monitoring  | 5  |
|  RHN_ENTITLEMENTS  |  unset_customer_enterprise  | 5  |
|  RHN_ENTITLEMENTS  |  set_customer_nonlinux  | 5  |
|  RHN_ENTITLEMENTS  |  set_customer_provisioning  | 5  |
|  RHN_ENTITLEMENTS  |  unset_customer_nonlinux  | 5  |
|  RPM  |  vercmpCounter  | 5  |
|  RHN_ENTITLEMENTS  |  set_customer_monitoring  | 5  |
|  RHN_ENTITLEMENTS  |  set_customer_enterprise  | 5  |
|  RHN_ENTITLEMENTS  |  unset_customer_provisioning  | 5  |
## Stored procs used in Perl



| XXRH_OAI_WRAPPER.sync_address |
| --- |
| XXRH_OAI_WRAPPER.sync_contact |
| XXRH_OAI_WRAPPER.sync_customer |
| dbms_utility.db_version |
| evr.as_vre_simple |
| rhn_bel.is_org_paid |
| rhn_channel.available_chan_subscriptions |
| rhn_channel.channel_priority |
| rhn_channel.get_license_path |
| rhn_channel.guess_server_base |
| rhn_channel.license_consent |
| rhn_channel.org_channel_setting |
| rhn_channel.refresh_newest_package |
| rhn_channel.update_channel |
| rhn_channel.user_role_check |
| rhn_config.delete_channel |
| rhn_config.delete_file |
| rhn_config.insert_channel |
| rhn_config_channel.action_diff_revision_status |
| rhn_config_channel.get_user_chan_access |
| rhn_config_channel.get_user_file_access |
| rhn_entitlements.can_entitle_server |
| rhn_entitlements.can_switch_base |
| rhn_entitlements.remove_server_entitlement |
| rhn_ep.entitlement_queue_pending |
| rhn_ep.process_queue_batch |
| rhn_org.delete_user |
| rhn_server.bulk_set_custom_value |
| rhn_server.bulk_snapshot |
| rhn_server.bulk_snapshot_tag |
| rhn_server.delete_set_from_servergroup |
| rhn_server.remove_action |
| rhn_server.set_custom_value |
| rhn_server.snapshot_server |
| rhn_server.system_service_level |
| rhn_server.tag_snapshot |
| rhn_user.add_to_usergroup |
| rhn_user.add_users_to_usergroups |
| rhn_user.delete_from_usergroup |
| rhn_user.remove_from_usergroup |
| rhn_user.remove_users_from_servergroups |
| rhn_user.remove_users_from_usergroups |
| rpm.vercmp |
## Stored Procs used in Java




| rhn.delete_channel |
| --- |
| rhn_cache.update_perms_for_server |
| rhn_cache.update_perms_for_user |
| rhn_channel.available_chan_subscriptions |
| rhn_channel.channel_priority |
| rhn_channel.get_license_path |
| rhn_channel.get_org_access |
| rhn_channel.guess_server_base |
| rhn_channel.loose_user_role_check |
| rhn_channel.org_channel_setting |
| rhn_channel.refresh_newest_package |
| rhn_channel.subscribe_server |
| rhn_channel.unsubscribe_server |
| rhn_channel.update_channel |
| rhn_channel.user_role_check |
| rhn_channel.user_role_check_debug |
| rhn_config.delete_channel |
| rhn_config.delete_file |
| rhn_config.delete_revision |
| rhn_config.insert_channel |
| rhn_config.insert_file |
| rhn_config.insert_revision |
| rhn_config_channel.get_user_chan_access |
| rhn_config_channel.get_user_file_access |
| rhn_config_channel.get_user_revision_access |
| rhn_entitlements.assign_channel_entitlement |
| rhn_entitlements.assign_system_entitlement |
| rhn_entitlements.can_entitle_server |
| rhn_entitlements.entitle_server |
| rhn_entitlements.remove_server_entitlement |
| rhn_entitlements.unentitle_server |
| rhn_ep.poll_customer_internal |
| rhn_ep.process_queue_batch |
| rhn_org.delete_org |
| rhn_org.delete_user |
| rhn_server.delete_from_servergroup |
| rhn_server.insert_into_servergroup_maybe |
| rhn_server.remove_action |
| rhn_server.snapshot_server |
| rhn_server.system_service_level |
| rhn_user.add_servergroup_perm |
| rhn_user.add_to_usergroup |
| rhn_user.check_role |
| rhn_user.remove_from_usergroup |
| rhn_user.remove_servergroup_perm |
| rpm.vercmp |
| web_disable_user_pkg.disable_user |
| web_disable_user_pkg.reenable_user |
| xxrh_oai_wrapper.sync_contact |
| xxrh_oai_wrapper.sync_customer |
# Delete Server Case Study



Contents of an email sent examining the effort in porting delete_server to application code vs PL/pgSQL.

create or replace
procedure delete_server (
	server_id_in in number
) is
	cursor servergroups is
		select	server_id, server_group_id
		from	rhnServerGroupMembers sgm
		where	sgm.server_id = server_id_in;
	cursor configchannels is
		select	cc.id
		from	rhnConfigChannel cc,
			rhnConfigChannelType cct,
			rhnServerConfigChannel scc
		where	1=1
			and scc.server_id = server_id_in
			and scc.config_channel_id = cc.id
			-- these config channel types are reserved
			-- for use by a single server, so we don't
			-- need to check for other servers subscribed
			and cct.label in
				('local_override','server_import')
			and cct.id = cc.confchan_type_id;
	cursor filelists is
		select	spfl.file_list_id id
		from	rhnServerPreserveFileList spfl
		where	spfl.server_id = server_id_in
			and not exists (
				select	1
				from	rhnServerPreserveFileList
				where	file_list_id =
spfl.file_list_id and server_id != server_id_in
				union
				select	1
				from	rhnKickstartPreserveFileList
				where	file_list_id =
spfl.file_list_id );
	cluster_id number;
    is_virt number := 0;
begin
	rhn_channel.delete_server_channels(server_id_in);
	-- rhn_channel.clear_subscriptions(server_id_in);

	for filelist in filelists loop
		delete from rhnFileList where id = filelist.id;
	end loop;

	for configchannel in configchannels loop
		rhn_config.delete_channel(configchannel.id);
	end loop;

    begin
      select unique 1 into is_virt
        from rhnServerEntitlementView
       where server_id = server_id_in
         and label in ('virtualization_host',
'virtualization_host_platform'); exception
      when no_data_found then
        is_virt := 0;
    end;

	for sgm in servergroups loop
		rhn_server.delete_from_servergroup(
			sgm.server_id, sgm.server_group_id);
	end loop;

    if is_virt = 1 then
        rhn_entitlements.repoll_virt_guest_entitlements(server_id_in);
    end if;

	-- we're handling this instead of letting an "on delete
	-- set null" do it so that we don't run the risk
	-- of setting off the triggers and killing us with a
	-- mutating table

	-- we cna't use a cursor to do this because we need "case"
	-- in a select to do that, and 8.1.7.3.0 doesn't support it.
	update rhnKickstartSession
		set old_server_id = null
		where old_server_id = server_id_in;
	update rhnKickstartSession
		set new_server_id = null
		where new_server_id = server_id_in;

	rhn_channel.clear_subscriptions(server_id_in,1);

    	-- A little complicated here, but the goal is to
	-- delete records from rhnVirtualInstace only if we don't
	-- care about them anymore.  We don't care about records
	-- in rhnVirtualInstance if we are deleting the host
	-- system and the virtual system is already null, or
	-- vice-versa.  We *do* care about them if either the
	-- host or virtual system is still registered because we
	-- still want them to show up in the UI.
				
        delete from rhnVirtualInstance
	      where host_system_id = server_id_in
		and virtual_system_id is null;
        delete from rhnVirtualInstance
	      where virtual_system_id = server_id_in
	        and host_system_id is null;

    -- If there's a newer row in rhnVirtualInstance with the same
    -- uuid, this guest must have been re-registered, so we can clean
    -- this data up.
    delete from rhnVirtualInstance vi            
    where vi.virtual_system_id = server_id_in
    and vi.modified < (select max(vi2.modified)
                    from rhnVirtualInstance vi2
                    where vi2.uuid = vi.uuid);
						
        update rhnVirtualInstance
	   set host_system_id = null
	 where host_system_id = server_id_in;

	update rhnVirtualInstance
	   set virtual_system_id = null
	 where virtual_system_id = server_id_in;
		
	update rhnVirtualInstanceEventLog
	   set old_host_system_id = null
         where old_host_system_id = server_id_in;

	update rhnVirtualInstanceEventLog
	   set new_host_system_id = null
         where new_host_system_id = server_id_in;
		 
	-- We're deleting everything with a foreign key to rhnServer
	-- here, now.  I'm hoping this will help aleviate our deadlock
	-- problem.

	delete from rhnActionConfigChannel where server_id =
server_id_in; delete from rhnActionConfigRevision where server_id =
server_id_in; delete from rhnActionPackageRemovalFailure where
server_id = server_id_in; delete from rhnChannelFamilyLicenseConsent
where server_id = server_id_in; delete from rhnClientCapability where
server_id = server_id_in; delete from rhnCpu where server_id =
server_id_in; -- there's still a cascade here, because the constraint
keeps the -- table locked for too long to rebuild it.  Ugh...
	delete from rhnDevice where server_id = server_id_in;
	delete from rhnProxyInfo where server_id = server_id_in;
	delete from rhnRam where server_id = server_id_in;
	delete from rhnRegToken where server_id = server_id_in;
	delete from rhnSNPServerQueue where server_id = server_id_in;
	delete from rhnSatelliteChannelFamily where server_id =
server_id_in; delete from rhnSatelliteInfo where server_id =
server_id_in; -- this cascades to rhnActionConfigChannel and
rhnActionConfigFileName delete from rhnServerAction where server_id =
server_id_in; delete from rhnServerActionPackageResult where server_id
# server_id_in; delete from rhnServerActionScriptResult where server_id
 server_id_in; delete from rhnServerActionVerifyResult where server_id

= server_id_in; delete from rhnServerActionVerifyMissing where
server_id = server_id_in; -- counts are handled above.  this should be
a delete_ function. delete from rhnServerChannel where server_id =
server_id_in; delete from rhnServerConfigChannel where server_id =
server_id_in; delete from rhnServerCustomDataValue where server_id =
server_id_in; delete from rhnServerDMI where server_id = server_id_in;
	delete from rhnServerMessage where server_id = server_id_in;
	-- this gets rhnServerMessage (only) on cascade; it's handled
just above delete from rhnServerEvent where server_id = server_id_in;
	delete from rhnServerHistory where server_id = server_id_in;
	delete from rhnServerInfo where server_id = server_id_in;
	delete from rhnServerInstallInfo where server_id = server_id_in;
	delete from rhnServerLocation where server_id = server_id_in;
	delete from rhnServerLock where server_id = server_id_in;
	delete from rhnServerNeededPackageCache where server_id =
server_id_in; delete from rhnServerNeededErrataCache where server_id =
server_id_in; delete from rhnServerNetwork where server_id =
server_id_in; delete from rhnServerNotes where server_id = server_id_in;
	-- I'm not removing the foreign key from rhnServerPackage;
that'll -- take forever.  Do the delete anyway.
	delete from rhnServerPackage where server_id = server_id_in;
	delete from rhnServerTokenRegs where server_id = server_id_in;
	delete from rhnSnapshotTag where server_id = server_id_in;
	-- this cascades to:
	--   rhnSnapshotChannel, rhnSnapshotConfigChannel,
rhnSnapshotPackage, --   rhnSnapshotConfigRevision,
rhnSnapshotServerGroup, --   rhnSnapshotTag.
	-- We may want to consider delete_snapshot() at some point, but
	--   I don't think we need to yet.
	delete from rhnSnapshot where server_id = server_id_in;
	delete from rhnTransaction where server_id = server_id_in;
	delete from rhnUserServerPrefs where server_id = server_id_in;
	-- hrm, this one's interesting... we _probably_ should delete
	-- everything for the parent server_id when we delete the proxy,
	-- but we don't currently.
	delete from rhnServerPath where server_id_in in (server_id,
proxy_server_id); delete from rhnUserServerPerms where server_id =
server_id_in;

	delete from rhn_interface_monitoring where server_id =
server_id_in; delete from rhnServerNetInterface where server_id =
server_id_in; delete from rhn_server_monitoring_info where recid =
server_id_in;

	delete from rhnAppInstallSession where server_id = server_id_in;
	delete from rhnServerUuid where server_id = server_id_in;
    -- first we delete all the probes running directly against this 
    -- system
    DELETE FROM rhn_probe_state PS WHERE PS.probe_id in  
    (SELECT CP.probe_id     
       FROM rhn_check_probe CP  
      WHERE CP.host_id = server_id_in
    );

    DELETE FROM rhn_probe P  WHERE P.recid in 
    (SELECT CP.probe_id  
       FROM rhn_check_probe CP
      WHERE CP.host_id = server_id_in
    );                   
    -- Now we delete any probes that were using this Server
    -- as a Proxy Scout.
    DELETE FROM rhn_probe_state PS
      WHERE PS.probe_id in 
      (SELECT CP.probe_id 
       FROM rhn_check_probe CP
      WHERE CP.sat_cluster_id in
    (SELECT SN.sat_cluster_id
       FROM rhn_sat_node SN
      WHERE SN.server_id = server_id_in));

    DELETE FROM rhn_probe P
       WHERE P.recid in 
      (SELECT CP.probe_id 
         FROM rhn_check_probe CP
         WHERE CP.sat_cluster_id in
      (SELECT SN.sat_cluster_id
         FROM rhn_sat_node SN
         WHERE SN.server_id = server_id_in));

	delete from rhn_check_probe where host_id = server_id_in;
	delete from rhn_host_probe where host_id = server_id_in;

    delete from rhn_sat_cluster where recid in
      ( select sat_cluster_id from rhn_sat_node where server_id =
server_id_in );

	delete from rhn_sat_node where server_id = server_id_in;
	
	-- now get rhnServer itself.
	delete
	from	rhnServer
		where id = server_id_in;

	delete
	from	rhnSet
	where	1=1
		and user_id in (
			select	wc.id
			from	rhnServer rs,
				web_contact wc
			where	rs.id = server_id_in
				and rs.org_id = wc.org_id
		)
		and label = 'system_list'
		and element = server_id_in;
end delete_server;
/
show errors;





For the record that's about 212 lines excluding comments. 

Appears to be called once in the Python backend in
backend/server/rhnServer/server_kickstart.py:

    # First delete all the guest server objects:
    h = rhnSQL.prepare(_query_lookup_guests_for_host)
    h.execute(server_id=server_id)
    delete_server = rhnSQL.Procedure("delete_server")
    log_debug(4, "Deleting guests")
    while 1:
        row = h.fetchone_dict()
        if not row:
            break
        guest_id = row['virtual_system_id']
        log_debug(4, 'Deleting guest server: %s'% guest_id)
        try:
            if guest_id != None:
                delete_server(guest_id)
        except rhnSQL.SQLError:
            log_error("Error deleting server: %s" % guest_id)

And once in Perl in modules/rhn/RHN/DB/Server.pm:

sub delete_server {
  my $self = shift;

  my $dbh = RHN::DB->connect();
  my $query;
  my $sth;

  eval {
    $query = <<EOQ;
BEGIN
  delete_server(?);
END;
EOQ
    $sth = $dbh->prepare($query);
    $sth->execute($self->id);

    $dbh->commit;
  };

  if ($@) {
    my $E = $@;
    $dbh->rollback;

    die $E;
  }
}

As well as once in Java in
code/src/com/redhat/rhn/common/db/datasource/xml/System_queries.xml:

<callable-mode name="delete_server">
    <query params="server_id">
BEGIN
  delete_server(:server_id);
END;
    </query>
</callable-mode>

When appears to be used about twice.

So how long will this take to port to application code if we were to
choose that route? Most of the sql appears fairly straightforward, a
big factor is how much of this code is already represented in
Hibernate, particularly with the cursors. (as with all the
delete/update statements we can always fall back to just executing
similar sql statements, just not from a stored procedure) I'll break up
the estimate into two parts, implementation and unit testing.

So examining the cursors:

rhnServerGroupMembers - Appears we're already set up to work with this
table in Hibernate mappings, at least somewhat.

rhnConfigChannel, rhnConfigChannelType, rhnServerConfigChannel - All
look to be mapped in Hibernate.

rhnServerPreserveFileList, rhnKickstartPreserveFileList - Do not appear
to be mapped by Hibernate. Both however appear trivial to map and
appear to only have a relationship with rhnFileList, which is mapped in
Hibernate already.

Next up, calls to other stored procedures:

rhn_channel.delete_server_channels(server_id_in);

No references to this in Java. Wouldn't be trivial to add one but
perhaps there's equivalent application code already. Procedure appears
to basically do an update on rhnPrivateChannelFamily (which is mapped
in hibernate) and also does lookups in rhnChannelFamilyMembers
(mapped), rhnServerChannel (mapped, I think),
rhnChannelFamilyVirtSubLevel (mapped), rhnSGTypeVirtSubLevel (NOT
mapped), rhnServerEntitlementView (NOT mapped), and rhnVirtualInstance
(mapped). Could proceed two ways here, just add a call to the stored
procedure using our Datasource queries (see delete_server in
System_queries.xml referenced above), or bite the bullet and write
application code for this procedure as well. (leaving it in place with
a comment that it's been ported for when we eventually are ready to
remove references to it)

rhn_config.delete_channel(configchannel.id);

Appears to be called several times in our Datasource queries and once
in an actual Hibernate query as well. Shouldn't be too much trouble.

rhn_server.delete_from_servergroup(
            sgm.server_id, sgm.server_group_id);    

Appears to already be called in Java same as previous proc.

rhn_entitlements.repoll_virt_guest_entitlements(server_id_in);

Doesn't seem to be called. Would probably advise adding a Datasource
call to this procedure as well until we're ready to port it to
application code. (it's pretty scary)

Now to expose the call to Perl/Python, we have to check if their calls
to the proc are nested within some big transaction that contains other
things.

First for the Python: usage of delete_server is only for deleting
guests following a host kickstart. (i.e. the guests obviously no longer
exist) It appears to be bundled with (a) an update to the kickstart
status (IMO this is unimportant to have in the same transaction, the
host has been kickstarted successfully), (b) a simple delete sql
statement to clear out virtual instances. Worst case scenario, we add
an RPC call that calls the delete server application code + an sql call
to delete virtual instances. Best case we decide we don't care that
these are in the same transaction.

Now for the Perl: I think this is only used in
modules/sniglets/Sniglets/Servers.pm in sub delete_server_cb. The
transaction looks clean, I think the call to delete_server is the only
SQL writing going on.

Exposing the Java application code over RPC to these two layers then
(assuming Thrift works out ok) would be a relatively straightforward
couple of methods on SpacewalkService.java and
SpacewalkServiceHandler.java (see the remote postgresql branch in git
for a do nothing sample call) which in turn would call the Manager
method where we implemented our application code. 

So these look like the tricky points in migrating delete_server to
application code. The rest of the procedure just looks like fairly
simple loops and update/delete statements. Speaking strictly of
implementing (no testing), exposing over the RPC API, and updating the
couple Perl/Python refs, I think this code could be mirrored in Java
using a combination of Hibernate and Datasource (which can be a pain to
get the hibernate session right but we do it all the time) somewhere
around 3-4 days.

Now for the testing. The java calls to delete_server appear to all
filter through ServerFactory.delete(), called by
SystemManager.deleteServer().  SystemManagerTest.java already has 3
tests for this call including some which test virtual guest/host
deletion. Coverage is not 100%. This could stand to be expanded to
include some tests that the config channels and file preservation code
as well as some of the other more tricky looking portions of the
procedure. Testing will likely be the part of this that takes the most
time, depending on how careful we want to be. However it takes time
because it should have been done in the first place and wasn't. I would
estimate the additional testing may take somewhere between 3-5 days.

So to the alternate approach, how long would this procedure take to
port straight to PL/pgSQL? I really don't know, as far as moving the
straightforward procedure code over I don't have experience but I can't
imagine a skilled PL/SQL -> PL/pgSQL developer would take much longer
than 1-2 days to do it.

Then we consider, do we intend to just let Perl/Python keep calling the
procedure? If so, does the call have to change or will it be made
identical to Oracle? If not we have to go in and update the references
to basically say:

if (postgresql):
  call_me_like_this
else if (oracle):
  call_me_like_that

Which sucks but it's not the end of the world. (although it will
balloon with every call made and probably make for an unmanageable
mess) We could drop a quick and dirty framework in to call stored
procedures properly based on some key quite easily. Note that in this
approach, we don't have to worry about the transaction issue as
described above when discussing porting to application code.

If instead we chose to tuck it away in Java and expose it via an RPC
call, we have to deal with the transaction problem as well, in this
case it appears to be minimal impact, maybe a couple extra hours to
sort out.

And the last big issue, if we port straight to PL/pgSQL, do we demand
the same level of unit testing we would if we were porting to
application code? IMO yes we should, it's too risky otherwise as we
just can't get enough coverage without automating. If this is the case
I believe the testing time will be equal for both approaches. Also note
that EnterpriseDB contractors presumably will not be able to write Java
unit tests for the procedures they port, we will have to either do it
ourselves or trust their ability to test components in our product.

My conclusion, I'd estimate a straight PL/pgSQL port to take around 7
days if we demand the same level of testing, and a port to application
logic to take around 9. For those extra days we end up with 200 odd
less lines of duplicated code to maintain and one less stored procedure
to have to worry about on the road to being DB agnostic. If we choose
PL/pgSQL to get us up and running now, and then later we do the work to
achieve database independence, we invest approximately the sum of both
approaches, 16 days.

Does this analysis seem reasonable? Let me know what you think.

Cheers,

Devan

