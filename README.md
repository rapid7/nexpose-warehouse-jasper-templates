# Nexpose Warehouse Jasper Templates

Nexpose Warehouse Jasper Templates is a set of report templates designed for use against a dimensional data warehouse populated by the Nexpose Data Warehouse feature. Once Nexpose exports data through a periodic ETL process into the warehouse it is available for consumption using any Business Intelligence tool. This project caters to those that select [TIBCO Jaspersoft® Studio](http://community.jaspersoft.com/project/jaspersoft-studio). Refer to the [Jaspersoft Community](http://community.jaspersoft.com/documentation) for more documentation.

[![License](https://img.shields.io/badge/License-BSD%203--Clause-orange.svg)](License)

## Compatibility

[![Jaspersoft Studio](https://img.shields.io/badge/TIBCO%20Jaspersoft®%20Studio-3.6%2B-green.svg)]()
[![Nexpose](https://img.shields.io/badge/Nexpose-6.4.6%2B-green.svg)]()

## Installation

This project contains [self-contained directory](src/main/resources) of templates, sub-reports, and resources. To configure a [Jaspersoft® Studio](http://community.jaspersoft.com/project/jaspersoft-studio) workspace to edit and execute these templates, follow these steps:

1. Clone this repository (e.g. `git clone git@github.com:rapid7/nexpose-warehouse-jasper-templates.git`)
2. [Download](http://community.jaspersoft.com/project/jaspersoft-studio/releases) andi nstall the latest version of [TIBCO Jaspersoft® Studio](http://community.jaspersoft.com/project/jaspersoft-studio)
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

JRXML files can be divided into two categories:
  * *reports* - A printable report that includes headers, pagination, appendices and other sections that comprise a complete report output. Each report defines the layout or order of sub-reports to include in the output, with parameters specifying the input that adjust the desired output of the template. This report is what is used to execute and output a final report for consumption and distribution.
  * *sub-reports* - A sub-report is a reusable component of an overall template, specificall designed to visualize output of data for a specific query. Typically there is a one-to-one relationship between a query and a sub-report. Sub-reports can include charts, lists, tables, and other visualizations, and may contain multiple report elements. Sub-reports do not contain pagination or similar features and may be placed anywhere within a template. Sub-reports may include other sub-report elements. Sub-reports usually include at least one, but typically a few, parameters that can be used to customize the presentation (such as filters, sort expressions, and limits).

### Parameters

Input parameters in a report should use clear, verbose names that indicate the purpose of the input, with appropriate data types. There are a common set of input parameters, which should generally follow the same conventions. 

#### Base Directory

In order to ensure interoperability across workspaces, all paths defined using relative paths. Each JRXML file has at minimum one parameter defined, called `BASE_DIR`. This represents the relative path from the current JRXML file to the root of the `source/main/resources` folder. In most circumstances this will be set to "../.." but may varying dependening on the depth of the directory structure. All references to image or sub-report resources within the templates must use the `BASE_DIR` as the prefix for the path expression.

#### Reports

All reports must include the following parameters:
* `REPORT_NAME` - the textual name of the report
* `TITLE_PAGE` - boolean value indicating whether a title page is displayed (true), or whether a condensed page header is displayed (false)
* `SCOPE_APPENDIX` - whether to show an appendix for the scope of the report

#### Filters

* Report filters
  * May filter based on site, asset group, tag, asset, vulnerability, etc.
  * Names usually indicate the entities being filtered, and preferrably filter by name, e.g.:
    * `SITE_NAMES`
    * `ASSET_GROUP_NAMES`
    * `TAG_NAMES`
  * Can include the asset and vulnerability sub-report raw filter (see below)
* Sub-reports
  * Should **only** filter on vulnerabilities or assets. This ensures that sub-reports are reusable for different types of scope, and are all asset-based.
  * Should be named `WHERE_FILTER_ASSET` and `WHERE_FILTER_VULNERABILITY`
  * Defined as `WHERE` clause conditional expression, without the `WHERE` prefixed (e.g. `asset_id = 5`, `asset_id IN (SELECT asset_id FROM dim_site_asset WHERE site_id = 6)`)

#### Sorting

The sorting input parameter is a SQL ordering sub-expression and is named `ORDER_BY` (e.g. `risk_score DESC`). This value does not contain the "ORDER BY" keyword.

#### Limits

The limit condition of a query can be passed using a parameter named `LIMIT`. This value does not contain the "LIMIT" keyword.

### Sizes

All reports should be printable portrait orientation (except for rare cases when a landscape orientation is absolutely required). The default sizes are:

* Report
  * Format: A4 (595 x 842 px)
  * Orientation: Portrait
  * Margins
    * Top: 36 px
    * Bottom: 7 px
    * Left: 36 px
    * Right: 36 px
* Sub-report
  * Format: Custom (523 x Variable px)
  * Orientation: Portrait
  * Margin: 0 px

## Layout

The resource in this project are organized into several sub-directories. 

### `_templates`

This directory includes master template files that are the basis for reports and sub-reports. One of these templates should always be copied from when building a new report. This ensures consistent look and feel, and reduces the need to start the report authoring process from scratch. These templates usually include common report elements such as coverage page, headers, footer with pagination, and basic input parameters.

* `report-template.jrxml` - base template for a report that does not use trending
* `report-with-trend-template.jrxml` - base template for a report that uses trending (a start and end date input parameter)
* `subreport-template.jrxml` - base template for a sub-report

### `images`

Contains all graphical images. The Portable Network Graphics (PNG) format is preferred, and images should favor transparent backgrounds whenever possible.

### `reports`

Contains all reports, organized by a sub-directory with the name of the report. Each directory should contain one and only one report.

### `subreports`

Contains all sub-reports, organized into sub-directories by the entity that is subject to the sub-report (such as assets or sites). This allows report authors to easily find sub-reports by the type of content they display. Two additional sub-directories contains sub-reports that are unrelated to a specified content type, and are used for any type of report. The `_appendices` contains appendices that can be placed at the front or end of a report layout, and include sections such as title pages, scope overviews, etc. The `_functions` section includes sub-reports design to invoke functions within the dimensional warehouse and are typically used for trending purposes.

## Authoring

To ease authoring a report or sub-report, it is always recommended that you start from a template and copy/paste. One the template is copied, change the name of the JRXML (which defaults to "CHANGE-ME") before you continued editing the report.

## Releases

No official public releases are performed on this project, as the source files are the consumed artifacts.

## Contributions

Contributions are welcomed in this project. Ensure you develop according to the conventions outlined above. To contribute, fork this repository and open a pull-request.
