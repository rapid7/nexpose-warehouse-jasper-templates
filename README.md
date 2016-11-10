# Nexpose Warehouse Jasper Templates

Nexpose Warehouse Jasper Templates is a set of report templates designed for use against a dimensional data warehouse populated by the Nexpose Data Warehouse feature. Once Nexpose exports data through a periodic ETL process into the warehouse it is available for consumption using any Business Intelligence tool. This project caters to those that select [TIBCO Jaspersoft® Studio](http://community.jaspersoft.com/project/jaspersoft-studio). Refer to the [Jaspersoft Community](http://community.jaspersoft.com/documentation) for more documentation.

## Compatibility

* [Nexpose](https://www.rapid7.com/products/nexpose/) v6.4.6+
* [TIBCO Jaspersoft® Studio](http://community.jaspersoft.com/project/jaspersoft-studio) v3.6+ (community edition)

## Installation

This project contains [self-contained directory](src/main/resources) of templates, sub-reports, and resources. To configure a [Jaspersoft® Studio](http://community.jaspersoft.com/project/jaspersoft-studio) workspace to edit and execute these templates, follow these steps:

1. Clone this repository (e.g. `git clone git@github.com:rapid7/nexpose-warehouse-jasper-templates.git`)
2. [Download](http://community.jaspersoft.com/project/jaspersoft-studio/releases) Install the latest version of [TIBCO Jaspersoft® Studio](http://community.jaspersoft.com/project/jaspersoft-studio)
2. Open Jaspersoft® Studio
3. Create a new or select an existing workspace
4. Create a project in your workspace
  * "File" -> "New Project"
  * Select "JaspertReports Project"
  * "Next"
  * Name the project
  * "Finish"
5. Remove the JRE System Library from the project
  * Right click on the project in the Project Explorer pane
  * "Properties"
  * Select "Java Build Path"
  * Navigate to the "Libraries" tab
  * Select "JRE System Library"
  * "Remove"
  * "Ok"
6. Add a linked resource folder
  * Right click on the project in the Project Explorer pane
  * "New" -> "Folder"
  * Click ">> Advanced"
  * Select "Link to alternate location (Linked Folder)"
  * Select the `/src/main/resources` sub-directory of the location you cloned this repository
  * "Finish"

## Conventions

In order to ensure interoperability across workspaces, all paths defined using relative paths. Each JRXML file has at minimum one parameter defined, called `BASE_DIR`. This represents the relative path from the current JRXML file to the root of the `source/main/resources` folder. In most circumstances this will be set to "../.." but may varying dependening on the depth of the directory structure. All references to image or sub-report resources within the templates must use the `BASE_DIR` as the prefix for the path expression.

JRXML files can be divided into two categories:
  * *reports* - A printable report that includes headers, pagination, appendices and other sections that comprise a complete report output. Each report defines the layout or order of sub-reports to include in the output, with parameters specifying the input that adjust the desired output of the template. This report is what is used to execute and output a final report for consumption and distribution.
  * *sub-reports* - A sub-report is a reusable component of an overall template, specificall designed to visualize output of data for a specific query. Typically there is a one-to-one relationship between a query and a sub-report. Sub-reports can include charts, lists, tables, and other visualizations, and may contain multiple report elements. Sub-reports do not contain pagination or similar features and may be placed anywhere within a template. Sub-reports may include other sub-report elements. Sub-reports usually include at least one, but typically a few, parameters that can be used to customize the presentation (such as filters, sort expressions, and limits).

## Layout

The resource in this project are organized into several sub-directories. 

### `_templates`

This directory includes master template files that are the basis for reports and sub-reports. One of these templates should always be copied from when building a new report. This ensures consistent look and feel, and reduces the need to start the report authoring process from scratch. These templates usually include common report elements such as coverage page, headers, footer with pagination, and basic input parameters.

* `report-template.jrxml`
* `report-with-trend-template.jrxml`
* `subreport-template.jrxml`

### `images`

TODO

### `reports`

TODO

### `subreports`

TODO

## Releases

No official public releases are performed on this project, as the source files are the consumed artifacts.

## Contributions

TODO

This project uses [Semantic Versioning](http://semver.org/).

## License

The project is provided under the 3-Clause BSD License. See [license](license.txt) for details.
