## Java edit on place
 


Let say you want to edit single .java file on existing running machine. Let say the file you want to edit is KickstartCommand.java (picked up by random):




     cd /tmp
     mkdir -p com/redhat/rhn/domain/kickstart/
     scp yourmachine:/yourdir/java/src/com/redhat/rhn/domain/kickstart/KickstartCommand.java  com/redhat/rhn/domain/kickstart/
     javac -extdirs /var/lib/tomcat5/webapps/rhn/WEB-INF/lib/ com/redhat/rhn/domain/kickstart/KickstartCommand.java
     jar uf /usr/share/rhn/lib/rhn.jar com/redhat/rhn/domain/kickstart/KickstartCommand.class
     /etc/init.d/tomcat5 restart

