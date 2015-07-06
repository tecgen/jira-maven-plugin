Maven JIRA Plugin
=================

The initial code and older versions were originally placed in http://code.google.com/p/jira-maven-plugins/.

This Maven plugin allows performing of JIRA common actions, like releasing a version, create a new version and generate the release notes:

NOTE: This plugin is not installed in any Maven repository, so you must build and install locally before using it.

To build the plugin: 

    mvn clean install
    

Before you start using this plugin, you must have two configurations already set on your pom.xml:

issueManagement tag
=====================

        <issueManagement>
           <system>JIRA</system>
           <url>http://www.myjira.com/jira/browse/PROJECTKEY</url>
        </issueManagement>

Note: This is extremely important, as will use this information to connect on JIRA.

<server> entry in settings.xml with the authentication information
=====================

Put the following in the settings.xml file: 

    <servers>
        <server>
            <id>jira</id>
            <username>your_user</username>
            <password>your_password</password>
        </server>
    </servers>


Also, make sure your JIRA has SOAP access enabled.


release-jira-version goal
=====================

Add the following profile to be executed when released:

    <profile>
	    <id>release</id>
	    <activation>
		    <property>
			    <name>performRelease</name>
			    <value>true</value>
		    </property>
	    </activation>
	    <build>
		    <plugins>
			    <plugin>
				    <groupId>com.george.app</groupId>
				    <artifactId>jira-maven-plugin</artifactId>
				    <version>1.2</version>
				    <inherited>false</inherited>
				    <configuration>
					    <!- <server> entry in settings.xml -->
					    <settingsKey>jira</settingsKey>
				    </configuration>
				    <executions>
					    <execution>
						    <phase>deploy</phase>
						    <goals>
							    <goal>release-jira-version</goal>
						    </goals>
					    </execution>
				    </executions>
			    </plugin>
		    </plugins>
	    </build>
    </profile>

create-new-version
=====================

Creates a new JIRA version of this project (without the -SNAPSHOT suffix)

Place it on your pom.xml:

    <plugin>
	    <groupId>com.george.app</groupId>
	    <artifactId>jira-maven-plugin</artifactId>
	    <version>1.2</version>
	    <inherited>false</inherited>
	    <configuration>
		    <!- <server> entry in settings.xml -->
		    <settingsKey>jira</settingsKey>
	    </configuration>
	    <executions>
		    <execution>
			    <phase>deploy</phase>
			    <goals>
				    <goal>create-new-jira-version</goal>
			    </goals>
		    </execution>
	    </executions>
    </plugin>


TODO: create-new-ticket
=======================
 * JIRA ticket creation within the current JIRA version
 * prints out the name e.g. APP-1234
 * Goal: The ticket name can be used for the next commit message.

