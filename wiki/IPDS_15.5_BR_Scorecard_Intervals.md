**Scorecard Calculation Observer**

A Scorecard Calculation Observer will receive Extract Data
Notifications. If the notification indicates that extract data
processing completed successfully, a Scorecard Calculation Request will
be stored with the following data:

- id - a generated unique id
- scorecardType - the type of scorecard calculation based on the
  extractDataSource from the Extract Data Characteristics (e.g., IPP for
  IPDS, etc.)
- startDate - the minRecordDate from the Extract Data Characteristics
- endDate - the maxRecordDate from the Extract Data Characteristics
- refId - the refId. the system may use this to associate scorecard
  processing with extract data processing
- processed - a boolean; a value of true indicates that the request was
  successfully processed by scorecard logic, and a value of false
  indicates that the request has not been processed or was
  unsuccessfully processed by scorecard logic. The default value of
  processed for a new entry is false.

**IPP Interval Range Determination for Scorecard Processing**

From the set of scorecard calculation requests where processed is false
and scorecard type is IPP, select the following for start and end date:

- the start date for the range is the maximum of the following:
  - the minimum post date of all logical records fact records in the
    extract data file. for purge records, the minimum last update
    timestamp will be used
  - the minimum scorecard config date
  - the date-based purge date (derived from config)
- the end date for the range is the maximum of the following:
  - the maximum post date of all logical fact records in the extract
    data file. for purge records, the maximum last update timestamp will
    be used
  - the maximum endDate of successfully processed scorecard requests

If scorecard processing completes successfully, all requests used in
scorecard interval determination will have processed set to true.

**Special Considerations:**

- There are scenarios in which scorecard input calculation may create
  confusing values; however, these scenarios will largely be due to data
  loading issues (e.g., manually created data sets with extreme dates
  (past or future), unprocessed daily extracts for a long period of time
  followed by a successful gap extract, etc.). These scenarios will be
  handled by support on a case by case basis, and do not need to be
  handled by the data server.
