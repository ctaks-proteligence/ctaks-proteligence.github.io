When extract data processing is complete, it must notify other processes
of its completion.

The following data must be included in every notification:

1.  refId - a reference identifier

*//TODO: add other status info*

**Extract Data Characteristics**

During extract data processing, extract data characteristics must be
captured. If extract data processing completes successfully, the
following data must be included in the notification:

1.  extractDataSource - the extract data source (e.g., IPDS, IPOR, LCD,
    etc.)
2.  minRecordDate - the earliest date on a logical record in the extract
    data
3.  maxRecordDate - the latest date on a logical record in the extract
    data
