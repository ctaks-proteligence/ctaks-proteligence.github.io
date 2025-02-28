# Externally-generated inputs (Anthem)

# Common

As part of Anthem Phase 2, we are automating calculation of two sets of
scorecard inputs:

1.  **Average Speed of Answer**
    - Average Speed of Answer Volume
    - Average Speed of Answer (seconds)
2.  **Inquiries Resolved**
    - Home/Control Inquiries Resolved \<= 7 Days
    - Home/Control Inquiries Resolved \<= 30 Days
    - Home/Control Inquiries Resolved Total
    - Host/Par Inquiries Resolved \<= 7 Days
    - Host/Par Inquiries Resolved \<= 30 Days
    - Host/Par Inquiries Resolved Total

For simplicity, many requirements are consistent between the two.

## Files

Each set of inputs has its own stage, success, and failure directories,
configurable in the same manner as ETL's. Anthem will provide files for
each set of inputs to the appropriate stage directory.

The supported files have the following characteristics:

- ASCII
- Comma-separated, without quotation marks around each value
- Must include a column header row (titles specified below) when
  containing one or more records
- From some systems, fixed-width (space-padded) values (due to being
  generated on the mainframe)

Due to the last point, the process must trim whitespace from all values
read from a file.

Each file must not contain duplicates (defined as multiple rows having
the same key). If duplicates exist in a file, the behavior is undefined.

The process moves each file to the success or failure directory as
appropriate; success indicates all records in the file are able to be
parsed according to these requirements.

## Aggregation

Perform all calculations using the same rounding logic as the Datanet
scorecard calculations.

## First-Discovered Timestamp

When the process first discovers a file in a stage directory, it retains
the current local system time as the **first-discovered timestamp** for
that file. This timestamp must persist even if the process is restarted.
Files placed in the stage directory in quick succession or while the
process is stopped may have indeterminate first-discovered timestamps
relative to each other. Anthem should take care when placing multiple
files containing records for the same systems in a stage directory to
prevent older records from replacing newer records.

If multiple records with the same key exist, aggregation includes only
the record from the file with the latest first-discovered timestamp.

## Performance Level mapping

The files contain a column called Group. Each Group is mapped to one or
more Performance Levels, defined in a configuration file. More than one
Group may map to the same Performance Level, in which case all mapped
Groups' values are aggregated to that Performance Level. The
configuration file must specify only leaf Performance Levels.

The process follows the input-dependent logic specified below to
aggregate each Group. It then follows the same logic to aggregate the
Groups to each Performance Level as mapped.

## Input Storage

Store the specified inputs for all Performance Levels mapped in the
configuration file, including Performance Levels without any
corresponding rows in the files. Input values stored default to 0.

Store the inputs for the (configurable) most recent *n* Actual Dates;
determine the latest Actual Date by using the latest date of all records
from the Tied to Actual Date? column specified below.

# Average Speed of Answer

## Files

The files contain records with Number_Calls and Total_Seconds already
aggregated by key.

The following columns exist in the file:

<table>
  <tr>
    <th>Header Text in File</th>
    <th>Part of Key?</th>
    <th>Tied to Actual Date?</th>
    <th>Format</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>System</td>
    <td>'''Yes'''</td>
    <td>No</td>
    <td>String</td>
    <td>The system that generated the file</td>
  </tr>
  <tr>
    <td>Group</td>
    <td>'''Yes'''</td>
    <td>No</td>
    <td>String</td>
    <td>The group associated with the record; maps to Performance Level(s)</td>
  </tr>
  <tr>
    <td>Call_Date</td>
    <td>'''Yes'''</td>
    <td>'''Yes'''</td>
    <td>Date (YYYY-MM-DD)</td>
    <td>The date on which the calls were received</td>
  </tr>
  <tr>
    <td>Number_Calls</td>
    <td>No</td>
    <td>No</td>
    <td>Integer</td>
    <td>The number of calls received</td>
  </tr>
  <tr>
    <td>Total_Seconds</td>
    <td>No</td>
    <td>No</td>
    <td>Decimal</td>
    <td>The summed number of seconds taken to answer all calls received on Call_Date</td>
  </tr>
</table>
<br  />
## Aggregation

Sum Number_Calls and Total_Seconds separately for each Group-Call_Date
combination. Use the Performance Level mapping to sum the same columns
from Group-Call_Date to Performance Level-Call_Date.

These values are day-to-date; the scorecard inputs are month-to-date.
Therefore, now sum the same columns to Performance Level-Actual Date:
For each Actual Date, sum the same columns where Call_Date is within the
range \[(first day of Actual Date's month), Actual Date\], inclusive.

Using the Performance Level-Actual Date summed values, the scorecard
inputs are calculated as:

- **Average Speed of Answer Volume** = Number_Calls
- **Average Speed of Answer (seconds)** = Total_Seconds / Number_Calls

# Inquiries Resolved

## Files

The files contain detail records, one per inquiry.

The following columns exist in the file:

<table>
  <tr>
    <th>Header Text in File</th>
    <th>Part of Key?</th>
    <th>Tied to Actual Date?</th>
    <th>Format</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>System</td>
    <td>'''Yes'''</td>
    <td>No</td>
    <td>String</td>
    <td>The system that generated the file</td>
  </tr>
  <tr>
    <td>Group</td>
    <td>'''Yes'''</td>
    <td>No</td>
    <td>String</td>
    <td>The group associated with the record; maps to Performance Level(s)</td>
  </tr>
  <tr>
    <td>Host_Home_Ind</td>
    <td>'''Yes'''</td>
    <td>No</td>
    <td>String</td>
    <td>The perspective; the values match those used by IPDS ('1' = Host, '2' = Home)</td>
  </tr>
  <tr>
    <td>Inquiry_Number</td>
    <td>'''Yes'''</td>
    <td>No</td>
    <td>String</td>
    <td>The key in the source inquiry system</td>
  </tr>
  <tr>
    <td>Receipt_Date</td>
    <td>No</td>
    <td>No</td>
    <td>Date (YYYY-MM-DD)</td>
    <td>The start date of the inquiry</td>
  </tr>
  <tr>
    <td>Close_Date</td>
    <td>No</td>
    <td>'''Yes'''</td>
    <td>Date (YYYY-MM-DD)</td>
    <td>The end date of the inquiry</td>
  </tr>
</table>
<br  />
## Aggregation

For each record, calculate:

- **Cycle Time** = Days(Close Date) - Days(Receipt Date)

Each input increments its value by one according to the following:

<table>
    <caption>Increments?</caption>
  <tr>
    <th rowspan="4">rowspan="4"</th>
    <th>Input Title</th>
    <th colspan="6">colspan="6"</th>
    <th>Host/Home Ind</th>
  </tr>
  <tr>
    <th colspan="3">colspan="3"</th>
    <th>'1'</th>
    <th colspan="3">colspan="3"</th>
    <th>'2'</th>
  </tr>
  <tr>
    <th colspan="3">colspan="3"</th>
    <th>Cycle Time</th>
    <th colspan="3">colspan="3"</th>
    <th>Cycle Time</th>
  </tr>
  <tr>
    <th>&lt;= 7</th>
    <th>&lt;= 30</th>
    <th>&gt; 30</th>
    <th>&lt;= 7</th>
    <th>&lt;= 30</th>
    <th>&gt; 30</th>
  </tr>
  <tr>
    <td>Home/Control Inquiries Resolved &lt;= 7 Days</td>
    <td>No</td>
    <td>No</td>
    <td>No</td>
    <td>'''Yes'''</td>
    <td>No</td>
    <td>No</td>
  </tr>
  <tr>
    <td>Home/Control Inquiries Resolved &lt;= 30 Days</td>
    <td>No</td>
    <td>No</td>
    <td>No</td>
    <td>'''Yes'''</td>
    <td>'''Yes'''</td>
    <td>No</td>
  </tr>
  <tr>
    <td>Home/Control Inquiries Resolved Total</td>
    <td>No</td>
    <td>No</td>
    <td>No</td>
    <td>'''Yes'''</td>
    <td>'''Yes'''</td>
    <td>'''Yes'''</td>
  </tr>
  <tr>
    <td>Host/Par Inquiries Resolved &lt;= 7 Days</td>
    <td>'''Yes'''</td>
    <td>No</td>
    <td>No</td>
    <td>No</td>
    <td>No</td>
    <td>No</td>
  </tr>
  <tr>
    <td>Host/Par Inquiries Resolved &lt;= 30 Days</td>
    <td>'''Yes'''</td>
    <td>'''Yes'''</td>
    <td>No</td>
    <td>No</td>
    <td>No</td>
    <td>No</td>
  </tr>
  <tr>
    <td>Host/Par Inquiries Resolved Total</td>
    <td>'''Yes'''</td>
    <td>'''Yes'''</td>
    <td>'''Yes'''</td>
    <td>No</td>
    <td>No</td>
    <td>No</td>
  </tr>
</table>
<br  />
Increments?

Aggregate using the above logic for each Group-Close Date combination.
Use the Performance Level mapping to sum the input values from
Group-Close Date to Performance Level-Close Date.

These values are day-to-date; the scorecard inputs are month-to-date.
Therefore, now sum the input values to Performance Level-Actual Date:
For each Actual Date, sum the input values where Close Date is within
the range \[(first day of Actual Date's month), Actual Date\],
inclusive.

Store these values as the corresponding scorecard inputs.
