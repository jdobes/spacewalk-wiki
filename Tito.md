
    #!div class="important" style="border: 2pt solid; text-align: center" 
    '''''DEPRECATED, NO LONGER USED''''' 

Tito is our homegrown tool for tagging packages, finding which packages need building, building tarballs and rpms, and shipping them off to the appropriate build system (straight to Koji or via CVS). It is designed to accomodate a git repository with a number of separate packages contained within. Tito has additional functionality geared towards building packages off an upstream project's git repository which you inherit from.

MORE COMING SOON
## Repository Configuration



Defined in rel-eng/tito.props at the root of your git repository. This file allows you to configure a number of options for how tito behaves, which builder/tagger classes to use, which packages get built where and how. Git branches allow you to customize this behavior for different releases and streams in the code.

An example configuration:


    [globalconfig]
    default_builder = spacewalk.releng.builder.Builder
    default_tagger = spacewalk.releng.tagger.VersionTagger
    tag_suffix = -mysuffix
    
    [koji]
    autobuild_tags = dist-f10-sw-0.5-candidate dist-5E-sw-0.5-candidate
    
    [dist-f10-sw-0.5-candidate]
    disttag = .fc10
    whitelist = some-package-name
    
    [dist-5E-sw-0.5-candidate]
    disttag = .el5
    blacklist = another-package-name
    
    [cvs]
    cvsroot = :gserver:cvs.example.com:/cvs/repo
    branches = CVS_BRANCH1 CVS_BRANCH2
### [[globalconfig]]




*default_builder* & *default_tagger* (required): The default builder and tagger classes to use. These are the defaults, specific packages can override them but this is seldom needed.

*tag_suffix* (optional): When tagging package releases this suffix will be appended to the git tag. Useful for projects inheriting from an upstream git repository, allowing you to identify which tags were created in your repository vs those that were pulled down from upstream.
### [[koji]]



*autobuild_tags*: Configures which koji tags packages should be built into. The actual koji configuration is your own koji client configuration. The default command used to submit packages can be customized in ~/.spacewalk-build-rc. (see section below)
### [[kojitag]]



Allow you to customize some behavior for a koji tag you're submitting a build to.

*disttag* (optional): Identifies the disttag to be applied to the source rpms you submit, otherwise the default for your OS will be used.

*blacklist* (optional): Prevent the packages listed from being built in the koji tag, and from appearing when checking for missing builds in that tag. Separate package names with a space.

*whitelist* (optional): Only allow the listed packages to be build in this koji tag. Implies all other packages are blacklisted and as such, if whitelist is defined the blacklist setting is ignored. This setting has no effect when checking for missing package builds. (yet)
## ~/.spacewalk-build-rc



This file allows for developer specific configuration of some tito behavior.


    $ cat ~/.spacewalk-build-rc 
    RPMBUILD_BASEDIR=/tmp/tito
    KOJI_OPTIONS=-c ~/.koji/koji-spacewalk.conf build --nowait




