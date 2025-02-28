# Oversight Enrichment (Anthem)

# Purpose

The purpose of Oversight Enrichment is to add the latest available
director name and manager name to each applicable record in the same
user interface and with the same functionality used for ITS and
Blue<sup>2</sup> records. This includes showing/hiding columns, sorting,
and filtering, as well as exporting.

# FlexReport Requirements

For Anthem, all ITS and Blue<sup>2</sup> reports, both Home and Host,
will have the following two fields added as Available columns:

- Name - Director
- Name - Manager

These columns should be filterable, sortable, and suggestible and should
be hidden by default.

# Mapping Requirements

### Home Records

Home Records will be mapped to the appropriate names based on a
combination of Prefix and record or message type.

The specific names to map per Prefix and record or message type will be
stored in a DB2 database that is configurable in ETL's property file.
Anthem will be responsible for making sure the mapping database is
available and accessible to ETL.

The mapping table should be named DNT_ANTHEM_OVERSIGHT and should
contain at least the following columns:

<table>
  <tr>
    <th>Title</th>
    <th>Type</th>
    <th>Length</th>
    <th>Nullable</th>
    <th>Comments</th>
  </tr>
  <tr>
    <td>Prefix</td>
    <td>VARCHAR</td>
    <td>3</td>
    <td>No</td>
  </tr>
  <tr>
    <td>ITS_Director</td>
    <td>VARCHAR</td>
    <td>100</td>
    <td>Yes</td>
    <td>Applies to all ITS records</td>
  </tr>
  <tr>
    <td>ITS_Manager</td>
    <td>VARCHAR</td>
    <td>100</td>
    <td>Yes</td>
    <td>Applies to all ITS records</td>
  </tr>
  <tr>
    <td>ADJUST_Director</td>
    <td>VARCHAR</td>
    <td>100</td>
    <td>Yes</td>
  </tr>
  <tr>
    <td>ADJUST_Manager</td>
    <td>VARCHAR</td>
    <td>100</td>
    <td>Yes</td>
  </tr>
  <tr>
    <td>CLMAPPL_Director</td>
    <td>VARCHAR</td>
    <td>100</td>
    <td>Yes</td>
  </tr>
  <tr>
    <td>CLMAPPL_Manager</td>
    <td>VARCHAR</td>
    <td>100</td>
    <td>Yes</td>
  </tr>
  <tr>
    <td>ESCL_Director</td>
    <td>VARCHAR</td>
    <td>100</td>
    <td>Yes</td>
    <td>Applies to ESCL1 and ESCL2 messages</td>
  </tr>
  <tr>
    <td>ESCL_Manager</td>
    <td>VARCHAR</td>
    <td>100</td>
    <td>Yes</td>
    <td>Applies to ESCL1 and ESCL2 messages</td>
  </tr>
  <tr>
    <td>GENINQ_Director</td>
    <td>VARCHAR</td>
    <td>100</td>
    <td>Yes</td>
  </tr>
  <tr>
    <td>GENINQ_Manager</td>
    <td>VARCHAR</td>
    <td>100</td>
    <td>Yes</td>
  </tr>
  <tr>
    <td>MEDREC_Director</td>
    <td>VARCHAR</td>
    <td>100</td>
    <td>Yes</td>
  </tr>
  <tr>
    <td>MEDREC_Manager</td>
    <td>VARCHAR</td>
    <td>100</td>
    <td>Yes</td>
  </tr>
</table>
<br  />
Any Prefix or message type not included in the table will not receive
either a director name or a manager name.

### Host Records

Host records are not currently eligible for Oversight Enrichment.

# Methodology

### Mapping Criteria Updates

When ETL runs a merge process, the database will be checked for the
latest mapping criteria. If the latest mapping criteria cannot be pulled
from the database, the latest version of the mapping criteria that ETL
has available will be used.

### FlexReport Updates

The detail data available in FlexReports will be updated one of two
ways: file-based mapping and remediation mapping.

##### File-based Mapping

When ETL merges data received via IPDS or LCD extract files, only data
that is specifically included in or updated by the data in the files
will be updated to include the latest applicable director name and
manager name.

##### Remediation Mapping

To ensure all data has the latest mappings, including data not updated
via files since the last mapping criteria update, Anthem will be able to
call an external ETL process to remediate the mappings. This process
will initiate a merge that checks all of the stored data against the
latest mappings so that each record available in FlexReports has the
latest applicable director name and manager name.

# Other Considerations

To keep from unintentionally removing mapped names due to issues
retrieving the mapping data, the following precautions are being
included for both mapping options:

- Use the cached mapping data if the count of rows in the mapping
  database is less than the count of rows in the cache; otherwise,
  replace the cache with the latest mapping data from the database
  before continuing.
- Fail the merge and/or remediation process and log an error if Oversite
  Enrichment is configured but no mapping data is available, meaning the
  cache is empty and the mapping information can't be retrieved from the
  database.
