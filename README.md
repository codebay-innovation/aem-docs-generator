# Issue repository  [![Codebay Innovation](https://www.codebay-innovation.com/components-auto-generator/codebay_logo.png)](https://www.codebay-innovation.com/)
This is the main repository for report issues related with the maven plugin "AEM Docs Generator"

## AEM Docs Generator 
[![Powered By Codebay Innovation](https://img.shields.io/badge/Powered%20By-Codebay%20Innovation-37c755?labelColor=27d1e0)](https://www.codebay-innovation.com/)

## Introduction
This plugin allows you to automatically generate documentation on an AEM project. More specifically, it collects and displays information about the following entities:
- AEM Components
- AEM Static Templates
- AEM Dynamic Templates
- Content Fragments Models
- OSGI Configurations
- I18n Dictionaries
- OAK Indexes

In this guide, the user will find the steps needed in order to run the plugin, how does the execution process work and which information is displayed for each one of the entities.

## How to use
In order to use the plugin within a project, the following steps must be performed:

1. Add the repository into your main POM.xml
```xml
<pluginRepositories>
	<pluginRepository>
		<id>codebay-products</id>
		<name>plugins-release</name>
		<url>https://artifactory.codeland.it/artifactory/codebay-products</url>
		<releases>
			<enabled>true</enabled>
		</releases>
		<snapshots>
			<enabled>false</enabled>
		</snapshots>
	</pluginRepository>
</pluginRepositories>
```
2. Add the following dependency on the dependencyManagement section of your main POM.xml
```xml
<dependency>
	<groupId>com.codebay.plugins</groupId>
	<artifactId>aemdocs-generator</artifactId>
	<version>1.0.0</version>
	<type>maven-plugin</type>
</dependency>
```
3. Add the following profile in your main POM.xml.
```xml
<profiles>
	...
	<profile>
		<id>docs</id>
		<properties>
			<report.pathToApps>ui.apps/src/main/content/jcr_root/apps/myProject</report.pathToApps>
			<report.pathDynamicTemplates>ui.content/src/main/content/jcr_root/conf/myProject/settings/wcm/templates</report.pathDynamicTemplates>
			<report.pathOsgiConfigs>ui.config/src/main/content/jcr_root/apps/myProject/osgiconfig</report.pathOsgiConfigs>
			<report.pathContentFragmentModels>ui.content/src/main/content/jcr_root/conf/myProject/settings/dam/cfm/models</report.pathContentFragmentModels>
			<report.pathI18n>ui.apps/src/main/content/jcr_root/apps/myProject/i18n</report.pathI18n>
			<report.pathOakIndex>ui.apps/src/main/content/jcr_root/_oak_index</report.pathOakIndex>
			<report.docsPath>aemdocs</report.docsPath>
		</properties>
		<build>
			<plugins>
				<plugin>
					<groupId>com.codebay.plugins</groupId>
					<artifactId>aemdocs-generator</artifactId>
					<executions>
						<execution>
							<phase>compile</phase>
							<goals>
								<goal>touch</goal>
							</goals>
						</execution>
					</executions>
				</plugin>
			</plugins>
		</build>
	</profile>
	...
</profiles>
```
Some properties must be configured according to the paths of the desired project. Please refer to the example above to see the default configuration of a project called “myProject”.
4. In order to run the plugin, add the “docs” profile to the maven build. For example: ```mvn clean install -Pdocs```

## How the plugin works
When executing the maven build of the project with the required profile, the plugin will collect information about each of the entities according to the paths selected in the POM.xml configuration.

| Paths                            | Descripton                                                                                              |
|----------------------------------|---------------------------------------------------------------------------------------------------------|
| report.pathToApps                | Path containing components (below “components” folder) and static templates (below “templates” folder). |
| report.pathDynamicTemplates      | Path containing dynamic templates.                                                                      |
| report.pathOsgiConfigs           | Path containing OSGI configurations.                                                                    |
| report.pathContentFragmentModels | Path containing content fragment models.                                                                |
| report.pathI18n                  | Path containing i18n dictionaries.                                                                      |

For each of these entities, it will create an HTML page with its information. Additionally, an index page is created, including the link for each page.

These pages will be created in the path specified in the property report.path in the POM.xml. For example, if the path “aemdocs” is used, it will create a folder called aemdocs under the root folder the project.

## Pages definition
### Index
An index.html will be created with the following information:
- Left column
    - List of Components: Links to all components pages.
    - List of Templates: Links to all templates pages.
    - List of Dynamic Templates: Links to all dynamic templates pages.
    - List of Content Fragment Models: Links to all content fragment models pages.
    - List of OSGi Configurations: Links to all OSGi configurations pages. It will split configurations into folders.
    - List of i18n Dictionaries: Links to all i18n dictionary pages.
    - List of Oak Indexes: Links to all oak indexes pages.
- Central column
    - Summary
        - Number of components of the project.
        - Number of templates of the project.
        - Number of dynamic templates of the project.
        - Number of content fragment models of the project.
    - Components
        - Table with the list of components. It includes pagination, sorting and search functionality.
    - Templates
        - Table with the list of templates. It includes pagination, sorting and search functionality.
    - Dynamic Templates
        - Table with the list of dynamic templates. It includes pagination, sorting and search functionality.
    - Content Fragment Models
        - Table with the list of content fragment models. It includes pagination, sorting and search functionality.
### Components Pages
The following information of the component is displayed:
- Path
- Properties
	- Name (property jcr:title)
	- Description (property jcr:description)
 	- Group (property componentGroup)
	- Super Type (property sling:resourceSuperType)
- List of all dialog properties
	- Label
	- Name
	- Type
	- Required
### Templates Pages
The following information of the template is displayed:
- Path
- Properties
	- Title (property jcr:title)
	- Description (property jcr:description)
	- Allowed Paths (property allowedPaths)
	- Allowed Parents (property allowedParents)
	- Allowed Children (property allowedChildren)
	- Allowed Templates (property cq:AllowedTemplates)
### Dynamic Templates Pages
The following information of the dynamic template is displayed:
- Path
- Properties
	- Title (property jcr:title)
	- Description (property jcr:description)
	- Template Type (property cq:TemplateType)
	- Allowed Templates (property cq:allowedTemplates)
	- Status (property status)
### Content Fragment Models Pages
The following information of the content fragment model is displayed:
- Path
- List of all fields
	- Label
	- Name
	- Type
	- Required
### OSGi Configurations Pages
The following information of the OSGi configuration is displayed:
- Path
- List of properties
	- Property
	- Value
### I18n Dictionaries Pages
The following information of the i18n dictionary is displayed:
- Path
- List of entries
	- Key
	- Value
### Oak Indexes Pages
The following information of the oak index is displayed:
- Path
- Properties
	- Type
	- IncludedPath
	- QueryPath
- List of index rules
	- Name
	- Type
	- Ordered
	- Property Index
	- Analyzed
