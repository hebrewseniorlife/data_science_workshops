## Method Name
Export Records

## Description
This method allows you to export a set of records for a project.

**Note about export rights**: Please be aware that Data Export user rights will be applied to this API request. For example, if you have 'No Access' data export rights in the project, then the API data export will fail and return an error. And if you have 'De-Identified' or 'Remove all tagged Identifier fields' data export rights, then some data fields *might* be removed and filtered out of the data set returned from the API. To make sure that no data is unnecessarily filtered out of your API request, you should have 'Full Data Set' export rights in the project.

## URL
https://ifar-edc.hsl.harvard.edu/redcap/api/

## Supported Request Method
POST

## Permissions Required
To use this method, you must have API Export privileges in the project.

## Parameters (case sensitive)
### Required

#### token
The API token specific to your REDCap project and username (each token is unique to each user for each project). See the section on the left-hand menu for obtaining a token for a given project.

#### content
record

#### format
csv, json, xml [default], odm ('odm' refers to CDISC ODM XML format, specifically ODM version 1.3.1)

#### type
- flat - output as one record per row [default]
- eav - output as one data point per row
- Non-longitudinal: Will have the fields - record*, field_name, value
Longitudinal: Will have the fields - record*, field_name, value, redcap_event_name
* 'record' refers to the record ID for the project

### Optional
#### records
an array of record names specifying specific records you wish to pull (by default, all records are pulled)

#### fields
an array of field names specifying specific fields you wish to pull (by default, all fields are pulled)

#### forms
an array of form names you wish to pull records for. If the form name has a space in it, replace the space with an underscore (by default, all records are pulled)

#### events
an array of unique event names that you wish to pull records for - only for longitudinal projects

#### rawOrLabel
raw [default], label - export the raw coded values or labels for the options of multiple choice fields

#### rawOrLabelHeaders
raw [default], label - (for 'csv' format 'flat' type only) for the CSV headers, export the variable/field names (raw) or the field labels (label)

#### exportCheckboxLabel
true, false [default] - specifies the format of checkbox field values specifically when exporting the data as labels (i.e., when rawOrLabel=label) in flat format (i.e., when type=flat). When exporting labels, by default (without providing the exportCheckboxLabel flag or if exportCheckboxLabel=false), all checkboxes will either have a value 'Checked' if they are checked or 'Unchecked' if not checked. But if exportCheckboxLabel is set to true, it will instead export the checkbox value as the checkbox option's label (e.g., 'Choice 1') if checked or it will be blank/empty (no value) if not checked. If rawOrLabel=false or if type=eav, then the exportCheckboxLabel flag is ignored. (The exportCheckboxLabel parameter is ignored for type=eav because 'eav' type always exports checkboxes differently anyway, in which checkboxes are exported with their true variable name (whereas the 'flat' type exports them as variable___code format), and another difference is that 'eav' type *always* exports checkbox values as the choice label for labels export, or as 0 or 1 (if unchecked or checked, respectively) for raw export.)

#### returnFormat
csv, json, xml - specifies the format of error messages. If you do not pass in this flag, it will select the default format for you passed based on the 'format' flag you passed in or if no format flag was passed in, it will default to 'xml'.

#### exportSurveyFields
true, false [default] - specifies whether or not to export the survey identifier field (e.g., 'redcap_survey_identifier') or survey timestamp fields (e.g., instrument+'_timestamp') when surveys are utilized in the project. If you do not pass in this flag, it will default to 'false'. If set to 'true', it will return the redcap_survey_identifier field and also the survey timestamp field for a particular survey when at least one field from that survey is being exported. NOTE: If the survey identifier field or survey timestamp fields are imported via API data import, they will simply be ignored since they are not real fields in the project but rather are pseudo-fields.

#### exportDataAccessGroups
true, false [default] - specifies whether or not to export the 'redcap_data_access_group' field when data access groups are utilized in the project. If you do not pass in this flag, it will default to 'false'. NOTE: This flag is only viable if the user whose token is being used to make the API request is *not* in a data access group. If the user is in a group, then this flag will revert to its default value.

#### filterLogic
String of logic text (e.g., [age] > 30) for filtering the data to be returned by this API method, in which the API will only return the records (or record-events, if a longitudinal project) where the logic evaluates as TRUE. This parameter is blank/null by default unless a value is supplied. Please note that if the filter logic contains any incorrect syntax, the API will respond with an error message.

#### dateRangeBegin
To return only records that have been created or modified *after* a given date/time, provide a timestamp in the format YYYY-MM-DD HH:MM:SS (e.g., '2017-01-01 00:00:00' for January 1, 2017 at midnight server time). If not specified, it will assume no begin time.
dateRangeEnd
To return only records that have been created or modified *before* a given date/time, provide a timestamp in the format YYYY-MM-DD HH:MM:SS (e.g., '2017-01-01 00:00:00' for January 1, 2017 at midnight server time). If not specified, it will use the current server time.

#### csvDelimiter
Set the delimiter used to separate values in the CSV data file (for CSV format only). Options include: comma ',' (default), 'tab', semi-colon ';', pipe '|', or caret '^'. Simply provide the value in quotes for this parameter.

#### decimalCharacter
If specified, force all numbers into same decimal format. You may choose to force all data values containing a decimal to have the same decimal character, which will be applied to all calc fields and number-validated text fields. Options include comma ',' or dot/full stop '.', but if left blank/null, then it will export numbers using the fields' native decimal format. Simply provide the value of either ',' or '.' for this parameter.