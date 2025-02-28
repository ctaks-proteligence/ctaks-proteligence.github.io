# Gap Audit

Gap Audit provides a mechanism for requesting specific ITS transactions
and Blue<sup>2</sup> messages. Datanet uses this for records that are
loaded without an earlier portion of their claim experience ("orphans")
to retrieve the records that make up that earlier portion.

## Gap Process (BCBSA)

### Gap Audit File

The gap process takes as input a file known as the Gap Audit file. This
file is a fixed-width EBCDIC file that lists SCCF IDs and Message IDs,
for which the gap process extracts matching ITS (for SCCF ID) and
Blue<sup>2</sup> (for Message ID) records.

For more information, please see the [latest BCBSA
documentation](https://vizzini.proteligence.com/docs/BCBSA/IPDS/IPDS_Extract_Record_Layouts.docx),
which includes the record layout (section 2.2). Both the ITS and
Blue<sup>2</sup> headers are required even if the file does not include
both ITS and Blue<sup>2</sup> records (the gap process always generates
both flat files and XML files, and the XML file generation errors out if
both headers are not present). Note that section 2.2.1.3 describes the
IPOR-specific SCCF file layout, not the Gap Audit file.

While the Gap Extract Start Date and Gap Extract End Date in the header
do not affect the gap process, it does validate that they are valid
dates and that Start \< End.

### Gap Files

The IPDS/IPOR common extract gap process produces as output gap files
for all file types; however, the AS file never contains records. The
files produced contain all records found matching the SCCF IDs and
Message IDs specified in the Gap Audit file.

### Usage

#### Datanet

Datanet provides a date-based purge to cap how many records are
available. However, a Plan can adjust a claim experience at any time,
including one with records earlier than Datanet's retention period or
the start timestamp used when generating initial files. While the
Blue<sup>2</sup> file contains entire message experiences (i.e. all
messages for the Root Message ID), the ITS files only contain the new
and updated transactions. Therefore, when a Plan adjusts a claim
experience older than the retention period, the daily IPOR flat files
will contain only the ITS transactions resulting from the adjustment,
and Datanet will not have the earlier transactions for those claim
experiences.

Therefore, Datanet must use the gap process to retrieve the earlier ITS
transactions that are missing.

#### IPOR

Like Datanet, IPOR provides a date-based purge and therefore requires a
similar solution for retrieving missing ITS transactions. Unlike
Datanet, IPOR does not load an ITS transaction while it is missing an
earlier transaction in its claim experience. Instead, it writes the
transaction to a retry file, and adds that SCCF Base to the day's SCCF
file.

The SCCF file is then sent to the source mainframe (if this is a target
IPOR) and run through the gap process. IPOR loads the gap files
produced, then loads the retry files.

This is known as the retry process or adjustment process; while the gap
files have identical layouts as IPDS, the actual jobs used are different
and are part of the IPOR install. Additionally, the SCCF file provided
by IPOR is not the same as the Gap Audit file produced by IPDS and
Datanet, though internal to the gap process the Gap Audit file is
converted into the SCCF file before generating the gap files.

#### IPDS

IPDS is intended to contain all records from all Plans. On a regular
schedule (currently monthly), IPDS determines which records it received
from one Plan but not the mirror Plan. For example, a Host Plan may not
have sent some records to the BCBSA that a Home Plan it interacted with
did. IPDS then creates a Gap Audit file listing those records and sends
it to the Host Plan. The Host Plan runs the gap process with that Gap
Audit file as input and sends the resulting gap files back to the BCBSA
to load into IPDS.

## ETL Utility (Datanet)

Datanet ETL provides a command-line utility that generates Gap Audit
files, one per Master Plan Code-Master Station Code-Process ID
combination specified in the Plan exit.

This utility adheres to the standard behaviors of all ETL utilities
regarding, for example, what happens when ETL is actively loading a file
or calculating scorecard.

### Command-Line Arguments

The utility takes the following command-line arguments:

<table>
  <tr>
    <th>Argument</th>
    <th>Required?</th>
    <th>Default</th>
  </tr>
  <tr>
    <td>Output Path</td>
    <td>Yes</td>
  </tr>
  <tr>
    <td>Gap Extract Start Date</td>
    <td>No</td>
    <td>Yesterday (local time)</td>
  </tr>
  <tr>
    <td>Gap Extract End Date</td>
    <td>No</td>
    <td>Today (local time)</td>
  </tr>
</table>
<br  />
The utility should error out if only one of the Dates are provided;
additionally, the utility should error out if either date is invalid, or
Start \>= End.

### Output

When run, the utility generates all Gap Audit files. The utility creates
a subdirectory named for each Master Plan Code-Master Station
Code-Process ID combination in the Plan exit within the specified Output
Path if not already present. Within the subdirectory for each
combination, the utility writes the Gap Audit file for that combination;
the filename is the name of the subdirectory with a UTC timestamp
appended indicating when the utility was run. This timestamp is the same
for all files generated by that run. The utility does not delete any
files already present.

(Having a separate subdirectory for each is beneficial for the mainframe
FTP process Plans use.)

### Missing Record Identification

A claim experience is missing records if any of the following are true:

- It contains only Blue<sup>2</sup> messages, but the messages are
  claim-correlated (i.e. specify a SCCF ID).
- It does not contain all ITS transactions from the SF to its latest ITS
  transaction present (see [Order of ITS
  Transactions](ITS_transactions#Order_of_Transactions "wikilink")),
  when restricted to records matching all of the following:

<table>
  <tr>
    <th>Field</th>
    <th></th>
    <th>Value</th>
    <th>Explanation</th>
  </tr>
  <tr>
    <td>Status Code</td>
    <td>==</td>
    <td>V</td>
    <td>'''Valid''': Only valid records move the claim experience forward.</td>
  </tr>
  <tr>
    <td>Estimate Indicator</td>
    <td>!=</td>
    <td>E</td>
    <td>'''Not Estimate''': Only non-estimates move the claim experience forward.</td>
  </tr>
  <tr>
    <td>Program Code</td>
    <td>!=</td>
    <td>6</td>
    <td>'''Not Custom''': Logic for Custom records is Plan-specific and is therefore currently out of scope.</td>
  </tr>
</table>
<br  />
- It contains a Home Blue<sup>2</sup> ADJUST message where ADJ_SF_IND =
  'Y' and XREF_SCCF_ID IS NOT NULL but the XREF_SCCF_ID is not present,
  though in this case, the corresponding XREF_SCCF_ID's claim experience
  needs to be requested.

While the Gap Audit file is meant to list all records that may be
missing in the target system, note that there is no way to determine if
records later than those present are missing or simply do not exist;
therefore, the above algorithm only concerns itself with records earlier
than those loaded. For example, an SF could load with Open/Closed
Indicator indicating Closed, but the corresponding DF is not loaded. In
this case, the DF might have been created after the most-recent DF IPOR
flat file was generated, and therefore will simply come through on the
next DF file.

#### Examples

##### DF only

The following claim experience is in the CDR:

<table>
  <tr>
    <th>Status Code</th>
    <th>Estimate Indicator</th>
    <th>Program Code</th>
    <th>CFA Code</th>
    <th>SCCF Suffix</th>
    <th>Format</th>
    <th>Disposition Code</th>
  </tr>
  <tr>
    <td>V</td>
    <td></td>
    <td>A</td>
    <td>1</td>
    <td>00</td>
    <td>DF</td>
    <td>Non-Void</td>
  </tr>
</table>
<br  />
According to the order of ITS transactions, the DF is the latest ITS
transaction present, and the SF is missing. Therefore, the second
condition above is met and the claim experience is considered to be
missing records.

##### SF only

The following claim experience is in the CDR:

<table>
  <tr>
    <th>Status Code</th>
    <th>Estimate Indicator</th>
    <th>Program Code</th>
    <th>CFA Code</th>
    <th>SCCF Suffix</th>
    <th>Format</th>
    <th>Disposition Code</th>
  </tr>
  <tr>
    <td>V</td>
    <td></td>
    <td>A</td>
    <td>1</td>
    <td>00</td>
    <td>SF</td>
  </tr>
</table>
<br  />
According to the order of ITS transactions, the SF is the latest ITS
transaction present, and no earlier record is missing. Therefore, the
claim experience is not considered to be missing records.

##### SF, DF only

The following claim experience is in the CDR:

<table>
  <tr>
    <th>Status Code</th>
    <th>Estimate Indicator</th>
    <th>Program Code</th>
    <th>CFA Code</th>
    <th>SCCF Suffix</th>
    <th>Format</th>
    <th>Disposition Code</th>
  </tr>
  <tr>
    <td>V</td>
    <td></td>
    <td>A</td>
    <td>1</td>
    <td>00</td>
    <td>SF</td>
  </tr>
  <tr>
    <td>V</td>
    <td></td>
    <td>A</td>
    <td>1</td>
    <td>00</td>
    <td>DF</td>
    <td>Non-Void</td>
  </tr>
</table>
<br  />
According to the order of ITS transactions, the DF is the latest ITS
transaction present, and no earlier record is missing. Therefore, the
claim experience is not considered to be missing records.

##### Multiple SCCF Suffixes

The following claim experience is in the CDR:

<table>
  <tr>
    <th>Status Code</th>
    <th>Estimate Indicator</th>
    <th>Program Code</th>
    <th>CFA Code</th>
    <th>SCCF Suffix</th>
    <th>Format</th>
    <th>Disposition Code</th>
  </tr>
  <tr>
    <td>V</td>
    <td></td>
    <td>A</td>
    <td>1</td>
    <td>00</td>
    <td>SF</td>
  </tr>
  <tr>
    <td>V</td>
    <td></td>
    <td>A</td>
    <td>1</td>
    <td>01</td>
    <td>DF</td>
    <td>Non-Void</td>
  </tr>
</table>
<br  />
According to the order of ITS transactions, the 01 Non-Void DF is the
latest ITS transaction present, and earlier records 00 Non-Void DF, 00
Non-Void RF, 00 Void DF, and 00 Void RF are missing. Therefore, the
second condition above is met and the claim experience is considered to
be missing records.

##### Custom

The following claim experience is in the CDR:

<table>
  <tr>
    <th>Status Code</th>
    <th>Estimate Indicator</th>
    <th>Program Code</th>
    <th>CFA Code</th>
    <th>SCCF Suffix</th>
    <th>Format</th>
    <th>Disposition Code</th>
  </tr>
  <tr>
    <td>V</td>
    <td></td>
    <td>6</td>
    <td>1</td>
    <td>00</td>
    <td>SF</td>
  </tr>
  <tr>
    <td>V</td>
    <td></td>
    <td>6</td>
    <td>1</td>
    <td>00</td>
    <td>RF</td>
    <td>Non-Void</td>
  </tr>
</table>
<br  />
No record with Program Code != 6 exists. Therefore, the claim experience
is not considered to be missing records.

##### XREF

The following claim experiences are in the CDR:

<table>
  <tr>
    <th>Message Type</th>
    <th>Perspective</th>
    <th>BOID</th>
    <th>Adjustment SF Indicator</th>
    <th>SCCF ID</th>
    <th>XREF SCCF ID</th>
  </tr>
  <tr>
    <td>ADJUST</td>
    <td>Home</td>
    <td>XXXX</td>
    <td>Y</td>
    <td>XXXXXXXXXXXXXXXXX</td>
    <td>YYYYYYYYYYYYYYYYY</td>
  </tr>
</table>
<br  />
<table>
  <tr>
    <th>Status Code</th>
    <th>Estimate Indicator</th>
    <th>Program Code</th>
    <th>Perspective</th>
    <th>BOID</th>
    <th>SCCF ID</th>
    <th>XREF SCCF ID</th>
    <th>Format</th>
    <th>Submission Type</th>
  </tr>
  <tr>
    <td>V</td>
    <td>!=E</td>
    <td>!=6</td>
    <td>Home</td>
    <td>XXXX</td>
    <td>YYYYYYYYYYYYYYYYY</td>
    <td>XXXXXXXXXXXXXXXXX</td>
    <td>SF</td>
    <td>3 or 4</td>
  </tr>
</table>
<br  />
The Home Blue<sup>2</sup> ADJUST message has ADJ_SF_IND='Y' and a value
for XREF_SCCF_ID, and the corresponding SF exists. Therefore, the claim
experience is not considered to be missing records. If the corresponding
SF did not exist, the gap data should include a request for the claim
experience with SCCF_BASE_ID YYYYYYYYYYYYYYY.

NOTE: As of 21.0, this is not yet planned for implementation due to the
implications of the changes needed to account for the XREF check in DN3
and the changes needed to include the XREF record and account for the
XREF check in DNA.

### Gap Audit File Generation

For each claim experience with missing records, write 100 records to the
Gap Audit file. These 100 records have the SCCF Base of the claim
experience, with the SCCF Suffix iterating from '00' to '99'. This
matches IPOR behavior. This behavior is due to not being able to
determine which records are missing.

### Possible Future Enhancements

1.  The current requirements only determine missing records within each
    claim experience and a specific XREF scenario as it applies to the
    Scorecard. The utility could additionally take into account other
    Adjustment SF and Cross-Reference SCCF scenarios, which would
    determine missing records for end-to-end experiences. Additionally,
    it could account for a purged Adjustment SF after the cross
    reference record is added to the B2 XREF claim experience. This is
    all currently considered out of scope.
2.  Though the Blue<sup>2</sup> file includes entire message experiences
    based on Root Message ID, adjustment cancellation responses have the
    Root Message ID of the adjustment cancellation request rather than
    of the adjustment request. This means it may be possible to receive
    a message experience with only the adjustment cancellation portion
    rather than the full adjustment message experience. The utility
    could include this scenario for Missing Record Identification. This
    is currently considered out of scope.
