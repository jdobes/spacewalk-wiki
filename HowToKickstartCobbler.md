## Introducing Spacewalk and Cobbler



Version 0.4 of Spacewalk adds significantly enhanced operating system installation capabilities by bundling the [Cobbler](https://cobbler.github.io/) Linux installation server.

_Note:  There are a lot of power features documented on the Cobbler project page, see https://cobbler.github.io/ for those._

New features in 0.4 include:

 * Installing and manage qemu/KVM based virtual machines and Xen fullvirt installs (previously, only Xen paravirt was supported)
 * Installing guests to LVM backed storage
 * Creating guests with multiple disks 
 * Creating guests with multiple virtual NICs on multiple bridges
 * First-class support for Fedora and CentOS in addition to RHEL
 * Evolving support for Debian, SuSE, and Ubuntu (limited exposure in Spacewalk)
 * Automatic PXE setup with auto-generated PXE menus for physical deployments
 * Integrated DHCP and DNS management (can be enabled, but is not surfaced in Spacewalk)
 * Powerful kickstart file templating using Cheetah (see https://fedorahosted.org/cobbler/wiki/KickstartTemplating and https://fedorahosted.org/cobbler/wiki/KickstartSnippets)
 * But wait, there's more! (see [Cobbler Wiki](https://github.com/cobbler/cobbler/wiki))
## Quick Start Guide for Spacewalk



 1. Create a Distro
    a. Initial steps
	1. Find some distribution isos and mount them.
		a. mkdir -p /var/iso-images
		a. downloadFedora-9-i386-DVD.iso for example to that directory
		a. mkdir -p /var/distro-trees/Fedora-9
		a. mount -oloop /var/iso-images/Fedora-9-i386-DVD.iso /var/distro-trees/Fedora-9
			* You may find it useful to include an entry in /etc/fstab to ensure this mount persists across reboots
        1. Create a Spacewalk channel for the distribution (if you have one set up already, go to the next step)
		a. In the Spacewalk GUI, select "Channels|Manage Software Channels"
                a. select "+ create new channel"
		a. Fill in the required fields as described - name: "Fedora 9" with a label "fedora-9"
		a. Populate the channel with packages using rhnpush
			* rhnpush --server localhost -u admin -p spacewalk  --channel fedora-9 /var/distro-trees/Fedora-9/Packages/*.rpm
			* Note that this step takes some time. We're working on the speed issue for the next release.
	1. Having made the Spacewalk base channel, you must now create a child channel for the kickstart helper tools
		a. in the GUI, select "Channels|Manage Software Channels" and select "+ create new channel"
		a. Fill in the required fields as described - call it "Fedora 9 with Tools" and label "fedora-9-with-tools"
		a. Download the following packages from  the client repo here http://spacewalk.redhat.com/yum/:
			* pyOpenSSL (in rawhide for F11)
			* rhnlib (in rawhide for F11)
			* libxml2-python (in rawhide for F11)
			* spacewalk-koan*
		a. Populate your new child channel with these rpms:  "rhnpush --server localhost -u admin -p spacewalk  --channel fedora-9-with-tools /path/to/the/helper/rpms/*.rpm"
		a. You're now ready to create a distribution.
    a. Select "Systems|Kickstart|Distributions" and select "+ create new distribution"
	1. On the "Create Kickstart Distribution" page, fill in the items as described
		a. "Distribution label" in this case might be "Fedora-9"
		a. "Tree path" should be "/var/distro-trees/Fedora-9"
		a. "Base channel" should be "Fedora 9 with Tools"
		a. "Installer generation" should be Fedora 9 in this case.
		a. Once you've pressed the "Create Kickstart Distribution" button you'll see a table that includes your new entry.
		a. If you run "cobbler distro list" at your spacewalk system's command line, you'll now see these entries in addition to any others you may have already defined.  This will show that things were created successfully.   These are named automatically by Spacewalk using the pattern <spacewalk distribution tag>:<organization ID>:<organization name>.  You may rename them in Cobbler if you like.
			* Fedora-9:1:Spacewalk-Public-Cert
			* Fedora-9:xen:1:Spacewalk-Public-Cert

 1. Having created your kickstartable distribution, you can now create a new Spacewalk kickstart profile:
    a. select "Systems|Kickstart|Profiles" and then select "+ create new kickstart profile"
    a. Give your new profile a label - in this case "Fedora-9-No-Tools"
    a. Select the "base channel" you created earlier
    a. Select the "kickstartable tree" you created earlier
    a. Select the "virtualization type" you intend the target machine to utilize.  (xen paravirt or qemu/KVM)
    a. Click "Next", and on the next screen just accept the "Default Download Location" and click "Next"
    a. Give your new machine a root password, and then click "Finish"
    a. If you run "cobbler profile list" you'll now see this entry in addition to those you've already defined:
            * Fedora-9:1:Spacewalk-Public-Cert
 1. Use Spacewalk to kickstart a system to a given profile
     a. Kickstarting physical machines
        1. You can kickstart physical machines with spacewalk:
            a. On your network
                1. Setup a dhcp server that sets the ''next-server'' variable to point to your spacewalk machine
                1. make sure your spacewalk machine address can be found in DNS.
            a. On the spacewalk server
                * edit /etc/xinetd.d/tftp and change the value of "disabled" to "no".  
                * generally, running "cobbler check" is an excellent way to see if your install server configuration has any problems like this that should be addressed.
                * in particular, make sure that TFTP is unblocked in your firewall rules.
            a. Some lab setup instructions that cover the same information can be found at [https://fedorahosted.org/spacewalk/wiki/kickstartingLabNetworkingSetup here]
        1. Bare metal machines can be kickstarted in one of two ways:
            a. By profile (interacting with the PXE menu)
                1. Connect a machine to the network, having already ensured DHCP and next-server are configured
                1. Power on the machine.
                    a. A disk with no OS will roll over to PXE boot after searching through available disks
                    a. Alternatively, set the machine to PXE first in the BIOS order. 
                1. Choose what profile you want to install from the PXE menu.
                1. The box will finish installation automatically
            a. By system record
                1. From a prompt on your spacewalk server: '''cobbler system add --name <nameOfYourSystem> --mac <mac addr of netboot interface> --profile <a profile from 'cobbler profile list'>'''.   
                1. Boot the machine, ideally set so it PXE's first in the BIOS order.  The install will continue without any user interaction.
                1. To reinstall the machine at any time, "cobbler system edit --name=foo --netboot-enabled=1" and cycle the system power.  Cobbler contains power management features if you want to use them.
        a. Reinstalling or upgrading machines that already have an OS on them
	   1. With koan:  "koan --replace-self --server=cobbler.example.org [[--profile=profile-name]] [[--system=system-name]]" and then reboot
	   1. Note that upgrades require an upgrade kickstart, and this is an operation that will destroy the existing system in most cases
       
Virtual machines:

Koan can be run on any Spacewalk managed machine to install new VMs.


    
    man koan, read the instructions for --virt
    

If you use profiles set up in Spacewalk, virtual machines will be registered to the Spacewalk server automatically. 

* For various other tips and tricks see  https://cobbler.github.io, in particular, you will probably be interested in the kickstart templating features under "User Documentation".
## How Cobbler and Spacewalk are linked internally



(This is development information only and should probably be a seperate topic.)

Spacewalk and Cobbler integration is bi-directionally synced: relevant elements of object definition - distros, profiles, and systems - are stored across both system's databases.  Spacewalk's awareness of these objects is both by keeping records for itself that contain information about UIDs for each cobbler object.

Object definition is done over an XMLRPC-based protocol. As far as configuration is concerned in XMLRPC communication between Spacewalk and Cobbler, with one exception (Cobbler back-authentication to Spacewalk), Spacewalk is always the "client", and the client presents UIDs generated by Cobbler as the primary key for the various object types being discussed in XMLRPC conversations.

This allows someone to mostly interact with Spacewalk or Cobbler directly as they see fit.  (For instance, it allows usage of features available in Cobbler but not in Spacewalk, and for the most part Spacewalk does not need to know... there are some technicalities in this WRT spacewalk system records, for instance, Cobbler does more with networking ATM than Spacewalk is aware of.  This should change in a future release.)

Every time Spacewalk asks Cobbler to do something via XMLRPC, Cobbler makes an XMLRPC call *back* to Spacewalk to authenticate the tokens passed into it.  A few settings in Cobbler are required to set this up.

 * redhat_management_server - this is the hostname (preferable) or ip address of the Spacewalk system
 * redhat_management_type - in the case of Spacewalk, this will be set to "site", signifying that the Cobbler instance is running on either an RHN Satellite or a Spacewalk instance. 

The call sequence is

        spacewalk_url = "https://%s/rpc/api" % server
        client = xmlrpclib.Server(spacewalk_url, verbose=0)
        valid = client.auth.checkAuthToken(username,password)

with *username* and *password* coming from [[PARTHA]].  This call is made when obtaining a cobbler token for interaction with Cobbler.  Once a token is established this callback does
not occur again. 

The following describes in some detail the interplay between the two systems in regards Distros, Profiles, and Systems.  Bear in mind however that Cobbler holds a fair number of relevant default values from its own installation.  Spacewalk only sets Cobbler parameters to the extent required to cause Cobbler to work with Spacewalk - there's much more going on with Cobbler that this document won't cover.  If you need more detail, _man cobbler_ and see  https://cobbler.github.io

 * When you create a *Distribution* in the Spacewalk GUI, Spacewalk makes one ore more distros in Cobbler, depending on the distribution:
    * Two XMLRPC calls are made to Cobbler to create two versions of a distro: one with a Xen kernel, one without.  The call passes:
        * http_server and http_port set to point to the spacewalk/cobbler server's apache instance
        * distro_name set to <Spacewalk Distribution Label>:<spacewalk organization id>:<spacewalk organization name>
        * ks_meta with the key-value pair "tree"=><absolute path to distro tree in web space on the sw/cobbler server>
        * initrd set to the absolute path to the initrd for the distribution in question - calculated by spacewalk
        * kernel set to the absolute path to the kernel for the distribution in question - calculated by spacewalk
    * When Cobbler processes the creation request, it generates UIDs for the Xen and non-Xen distros.   These UIDs form the tie that binds Spacewalk's *Distribution* to Cobbler's *distro* - as long as the two exist, these UIDs serve to uniquely identify each distro.
    * On the Spacewalk side, the Cobbler distro UIDs are stored together with a Spacewalk Organization ID, the distribution's label text, the absolute path to the distribution tree's root (which, in the case of a Spacewalk managed distribution, is a web-space path to Spacewalk package management logic - _not_ a plain old directory of files on disk), the channel id in which the distribution's  packages reside, an RHNKSTREETYPE reference indicating that the distribution is managed by Spacewalk, and an RHNINSTALLTYPE reference indicating more specifically the type of installation tree the distro uses (e.g. RHEL_5 or Fedora_10).
    * Note that if you're managing the distro entirely from Spacewalk, the fact of the existence of the _two_ distros (Xen and non) in Cobbler will be transparent to you.  If you happen to select Xen virtualization, the Xen version gets used, else the other.
 * When you create a *Kickstart Profile* in the Spacewalk GUI, you make a corresponding *profile* in Cobbler:
        * One XMLRPC call is made to Cobbler to create a *profile*.  The call passes:
            * Profile name
            * Distro name
            * Virtualization type
            * Organization ID (in ks_meta as org)
            * VM install location
            * virtual bridge name
            * Kickstart kernel path
            * Kernel options for the kickstart operation itself
            * Kernel options the machine should boot with after it's installed
            * Redhat management key
            * If there is virtualization, then also:
                * Amount of ram vm should get
                * Number of cpus vm should get
                * Size of disk vm should get
        * On the Spacewalk side, an RHNKSDATA record is created that records the following facts:
            * The kickstart type - normally wizard, unless you upload your own ks file - see below
            * The spacewalk organization ID
            * The profile's label text
            * Any comments associated with the profile
            * Whether or not the profile is active
            * Whether or not the kickstart process should write output of custom post scripts to /root/ks-post.log
            * Whether or not the kickstart process should write output of pre- scripts to /root/ks-pre.log
            * Whether or not the kickstart process should write ks.cfg and all %include fragments to /root/
            * The Cobbler UID for the profile
            * Any additional arguments you might wish to pass to the anaconda (kickstart) kernel
            * Any additional arguments you might wish to pass to the installed kernel
            * Whether or not this profile should be considered the default one for the organization
 * When and why we create cobbler system records.
     * Note: currently Spacewalk needs to learn more about multiple interfaces in Cobbler, and when they exist, network objects (should be a Trac on this)
     * Spacewalk also needs to know about how to tolerate already existing cobbler system records in it's registration engine (should be a Trac on this)
 * What happens when the user creates cobbler objects and when they show up in Spacewalk, and where these operations are limited
 * taskomatic's job in syncing changes from spacewalk to cobbler
 * taskomatic's job in syncing changes from cobbler to cobbler
## Cobbler Tips And Tricks



See  https://cobbler.github.io for lots of them -- this section can be updated later with pointers to some of the more popular items.

