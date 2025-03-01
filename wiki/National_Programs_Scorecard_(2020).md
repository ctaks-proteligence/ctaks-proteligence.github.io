# National Programs Scorecard (2020)

## Overview

The National Programs Scorecard is available for Month-to-Date,
Quarter-to-Date, and Half-to-Date (2 intervals per year, the first being
quarters 1 and 2 and the second being quarters 3 and 4). However, not
all inputs are available Month-to-Date. The list of inputs below
indicates each input's smallest available interval length. All inputs
are calculated/entered for the smallest available interval length and
then rolled up as necessary to the scorecard result's interval length.

Each calculated input considers certain records (those ITS transactions
and/or Blue<sup>2</sup> messages listed below in the timeline for that
input) in each claim experience to determine whether those records
should count towards the input value. Unless otherwise stated, a claim
experience may contain records such that it matches the input multiple
times and therefore counts multiple times.

The minimum allowable value for all fields is 0, inclusive. The maximum
allowable value for percentage fields is 100% and for all other fields
is not defined.

### Date and Age

Each input uses a Start Date and a Finish Date. The Finish Date must be
within the inclusive bounds of an interval to count towards the input
for that interval.

Currently, all calculated inputs come in pairs; each pair consists of a
Total and an age-restricted subset. The logic for the subset input
exactly matches that of the Total input except for the addition of the
age restriction indicated in the input's title. The age is calculated as
Finish Date - Start Date.

### Joining

With the exception of Adjustment SFs (used in the 2 Adjustment Processed
measures), all inputs join only transactions and messages within the
same claim experience. Therefore, all records possible for joining
except Adjustment SFs share the same SCCF Base.

For the purpose of tying records together within a claim experience to
form an input's timeline, use the first rule that applies:

1.  Join Blue<sup>2</sup> request and response messages of the same
    message type using request Message ID and response Root Message ID
    (unless otherwise mentioned, a request can only have one response).
2.  Join Blue<sup>2</sup> adjustment request and adjustment cancellation
    request using the previous rule, treating the adjustment request as
    the request and the adjustment cancellation request as the response.
3.  Join an ITS DF or RF transaction and Blue<sup>2</sup> adjustment or
    adjustment cancellation message by SCCF ID.
4.  Join ITS (except SF) transactions by SCCF ID.

(Blue<sup>2</sup> messages not mentioned above and SFs always have SCCF
Suffix 00 and therefore do not have any joining logic beyond sharing the
same SCCF Base.)

Data issues can arise, such that the rule in combination with the input
timeline's additional restrictions are not sufficient to determine a
single record to use for each step in the input's timeline. If this
occurs, exclude those records from consideration for the input being
calculated; log that the data issue exists, including the relevant
information (BOID, SCCF Base, input ID, and input title).

### Claim Experience-wide restrictions

Regardless of input, only consider a transaction or message if it meets
the following criteria:

<table>
  <tr>
    <th>Record</th>
    <th>Field</th>
    <th>Operator</th>
    <th>Value</th>
  </tr>
  <tr>
    <td>ITS and Blue&lt;sup&gt;2&lt;/sup&gt;</td>
    <td>Perspective</td>
    <td>==</td>
    <td>(Measure's perspective)</td>
  </tr>
  <tr>
    <td>ITS and Blue&lt;sup&gt;2&lt;/sup&gt;</td>
    <td>Program</td>
    <td>!=</td>
    <td>6</td>
  </tr>
  <tr>
    <td>ITS (only SF, DF)</td>
    <td>Estimate?</td>
    <td>!=</td>
    <td>E</td>
  </tr>
  <tr>
    <td>ITS</td>
    <td>Status</td>
    <td>==</td>
    <td>V</td>
  </tr>
</table>
<br  />
Additionally, an input for a processed measure considers a
Blue<sup>2</sup> message only if its latest Message Status is PRSD or
FNAL; and unless otherwise noted it considers a request-response pair
only if the request is closed (Open/Closed? == 2). Currently, all
calculated inputs belong to processed measures. (Blue<sup>2</sup>
messages move to FNAL only from PRSD; therefore, the PRSD or FNAL
restriction simply ensures the Blue<sup>2</sup> message has been
processed \[i.e. PRSD exists in its Message State history\].)

## Categories, Fields, and Inputs

### Transactional

#### Home DFs Processed \> 30 Days

##### Field

<table>
  <tr>
    <th>Points Possible</th>
    <td>5.00</td>
  </tr>
  <tr>
    <th>Standard Range</th>
    <td>0.5 - 3.0%</td>
  </tr>
  <tr>
    <th>Points Formula</th>
    <td>Linear (lower is better); includes no-volume logic</td>
  </tr>
  <tr>
    <th>Score Formula</th>
    <td>Percentage</td>
  </tr>
  <tr>
    <th>Class</th>
    <td>% Processed</td>
  </tr>
  <tr>
    <th>Description</th>
    <td>The percentage of valid DFs created more than 30 calendar days from receipt of SFs at the Home Plan.
      <br />
      <br />Calculation of days is based on Formats Database Home SF Receipt Post Date to the Post Date of the valid DF.</td>
  </tr>
  <tr>
    <th>Considerations</th>
    <td>Reporting Date is based on the Home DF Formats Post Date and Home SF Post Date.</td>
  </tr>
</table>
<br  />

##### Inputs

<table>
  <tr>
    <th rowspan="2">rowspan="2"  Title</th>
    <th rowspan="2">rowspan="2"  Calculated?</th>
    <th rowspan="2">rowspan="2"  Shared with Enhanced IPP Scorecard 2012?</th>
    <th colspan="2">colspan="2"  Interval Length</th>
  </tr>
  <tr>
    <th>Smallest Cumulative</th>
    <th>Frequency</th>
  </tr>
  <tr>
    <td>Home DF Proc &gt; 30 Days</td>
    <td>Yes</td>
    <td>No</td>
    <td>Month-to-Date</td>
    <td>daily</td>
  </tr>
  <tr>
    <td>Home DF Proc Total</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>Month-to-Date</td>
    <td>daily</td>
  </tr>
</table>
<br  />
##### Timeline and Restrictions

Even though the SF is used only for calculating the Age for the subset
input, the SF must be present for the DF to count towards either input.

Each claim experience can have at most one (valid) original DF, which
this measure targets; therefore, each claim experience can count at most
once towards each input for this measure.

<table>
  <tr>
    <th rowspan="2">rowspan="2"</th>
    <th rowspan="2">rowspan="2"  Date Field</th>
    <th rowspan="2">rowspan="2"  Order</th>
    <th rowspan="2">rowspan="2"  Record</th>
    <th colspan="2">colspan="2"  Restriction</th>
  </tr>
  <tr>
    <th>Field</th>
    <th>Criteria</th>
  </tr>
  <tr>
    <td>Start</td>
    <td>Date Posted</td>
    <td align="center">align="center"  1</td>
    <td>SF</td>
    <td colspan="2">colspan="2"  (none)</td>
  </tr>
  <tr>
    <td rowspan="3">rowspan="3"  Finish</td>
    <td rowspan="3">rowspan="3"  Date Created</td>
    <td rowspan="3" align="center">rowspan="3" align="center"  2</td>
    <td rowspan="3">rowspan="3"  DF</td>
    <td>SCCF Suffix</td>
    <td>00 (Original)</td>
  </tr>
  <tr>
    <td>Disposition</td>
    <td>!= 4 (not Void)</td>
  </tr>
  <tr>
    <td>Adj Closeout DF?</td>
    <td>!= Y</td>
  </tr>
</table>
<br  />
#### Host SFs Processed \> 10 Days

##### Field

<table>
  <tr>
    <th>Points Possible</th>
    <td>5.00</td>
  </tr>
  <tr>
    <th>Standard Range</th>
    <td>0.75 - 3.50%</td>
  </tr>
  <tr>
    <th>Points Formula</th>
    <td>Linear (lower is better); includes no-volume logic</td>
  </tr>
  <tr>
    <th>Score Formula</th>
    <td>Percentage</td>
  </tr>
  <tr>
    <th>Class</th>
    <td>% Processed</td>
  </tr>
  <tr>
    <th>Description</th>
    <td>The percentage of valid SFs processed aged greater than 10 calendar days from Host Claim Receipt Date to Host SF Post Date.
      <br />
      <br />Calculation of days is based on the number of calendar days from Host Plan Claim Receipt Date to the date the corresponding valid SF is posted to the Host Plan's Formats Database.</td>
  </tr>
  <tr>
    <th>Considerations</th>
    <td>Reporting Date is based on the Host SF Post Date and Original Claim Receipt Date.</td>
  </tr>
</table>
<br  />

##### Inputs

<table>
  <tr>
    <th rowspan="2">rowspan="2"  Title</th>
    <th rowspan="2">rowspan="2"  Calculated?</th>
    <th rowspan="2">rowspan="2"  Shared with Enhanced IPP Scorecard 2012?</th>
    <th colspan="2">colspan="2"  Interval Length</th>
  </tr>
  <tr>
    <th>Smallest Cumulative</th>
    <th>Frequency</th>
  </tr>
  <tr>
    <td>Host SF Proc &gt; 10 Days</td>
    <td>Yes</td>
    <td>No</td>
    <td>Month-to-Date</td>
    <td>daily</td>
  </tr>
  <tr>
    <td>Host SF Proc Total</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>Month-to-Date</td>
    <td>daily</td>
  </tr>
</table>
<br  />
##### Timeline and Restrictions

<table>
  <tr>
    <th rowspan="2">rowspan="2"</th>
    <th rowspan="2">rowspan="2"  Date Field</th>
    <th rowspan="2">rowspan="2"  Order</th>
    <th rowspan="2">rowspan="2"  Record</th>
    <th colspan="2">colspan="2"  Restriction</th>
  </tr>
  <tr>
    <th>Field</th>
    <th>Criteria</th>
  </tr>
  <tr>
    <td>Start</td>
    <td>Date Host Rcpt</td>
    <td rowspan="2" align="center">rowspan="2" align="center"  1</td>
    <td rowspan="2">rowspan="2"  SF</td>
    <td rowspan="2">rowspan="2"  Submission Type</td>
    <td rowspan="2">rowspan="2"  1, 2 (Original, Resubmitted Original)</td>
  </tr>
  <tr>
    <td>Finish</td>
    <td>Date Created</td>
  </tr>
</table>
<br  />
====Home Adjustments Processed \<= 14 Days====

##### Field

<table>
  <tr>
    <th>Points Possible</th>
    <td>5.00</td>
  </tr>
  <tr>
    <th>Standard Range</th>
    <td>95.0 - 99.5%</td>
  </tr>
  <tr>
    <th>Points Formula</th>
    <td>Linear (higher is better); includes no-volume logic</td>
  </tr>
  <tr>
    <th>Score Formula</th>
    <td>Percentage</td>
  </tr>
  <tr>
    <th>Class</th>
    <td>% Processed</td>
  </tr>
  <tr>
    <th>Description</th>
    <td>As a Home Plan as a receiver, the total non-cancelled Blue² Adjustment requests processed within 14 calendar days of receipt divided by the total non-cancelled Blue² Adjustment requests processed.
      <br />
      <br />Calculation of cycle time (days) is based on either:
      <br />- Home Plan Adjustment request received message Create Date; or
      <br />- If the adjustment cross reference indicator on the adjustment is set to Y, then the later of the Home Plan Adjustment request received message Create Date or the Home Plan Formats Post Date of the Adjustment SF
      <br />
      <br />To either (in this hierarchy):
      <br />- Adjustment Approval response sent message Create Date;
      <br />- Home Void DF Post Date; or
      <br />- Adjustment Denial response sent message Create Date.
    </td>
  </tr>
  <tr>
    <th>Considerations</th>
    <td>Cancelled Approved Adjustment requests are excluded. High Volume Adjustments are excluded. Streamline Adjustments are excluded. NASCO-to-NASCO adjustments are incorporated into this measure based on a total sum of total NASCO-to-NASCO Control and Blue² Home Adjustments Processed within 14 calendar days and a total sum of total NASCO-to-NASCO Control and Blue² Home Adjustments Processed.</td>
  </tr>
</table>
<br  />

##### Inputs

<table>
  <tr>
    <th rowspan="2">rowspan="2"  Title</th>
    <th rowspan="2">rowspan="2"  Calculated?</th>
    <th rowspan="2">rowspan="2"  Shared with Enhanced IPP Scorecard 2012?</th>
    <th colspan="2">colspan="2"  Interval Length</th>
  </tr>
  <tr>
    <th>Smallest Cumulative</th>
    <th>Frequency</th>
  </tr>
  <tr>
    <td>Home Adjustment Messages Proc &lt;= 14 Days - as receiver</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>Month-to-Date</td>
    <td>daily</td>
  </tr>
  <tr>
    <td>NASCO Control Adjustment Messages Proc &lt;= 14 Days - as receiver</td>
    <td>No</td>
    <td>Yes</td>
    <td>Month-to-Date</td>
    <td>daily</td>
  </tr>
  <tr>
    <td>Home Adjustment Messages Proc Total - as receiver</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>Month-to-Date</td>
    <td>daily</td>
  </tr>
  <tr>
    <td>NASCO Control Adjustment Messages Proc Total - as receiver</td>
    <td>No</td>
    <td>Yes</td>
    <td>Month-to-Date</td>
    <td>daily</td>
  </tr>
</table>
<br  />
##### Timeline and Restrictions

Only Path A counts towards this input. If the adjustment cancellation
request and response are present (Path B), the input excludes the
corresponding adjustment response from consideration.

This considers request-response pairs whether or not the request is
closed.

The Cross-Reference SF is considered only if the adjustment request has
Adj SF? == Y.

Note: The only differences from Host Adjustments Processed \<= 7 Days
are the perspective, Reason Codes, and age cut-off, as well as the
optional presence of the Cross-Reference SF.

<table>
  <tr>
    <th rowspan="2">rowspan="2"</th>
    <th rowspan="2" colspan="2">colspan="2" rowspan="2"  Date Field</th>
    <th colspan="2">colspan="2"  Order</th>
    <th rowspan="2">rowspan="2"  Record</th>
    <th colspan="2">colspan="2"  Restriction</th>
  </tr>
  <tr>
    <th>Path A</th>
    <th>Path B</th>
    <th>Field</th>
    <th>Criteria</th>
  </tr>
  <tr>
    <td rowspan="3">rowspan="3"  Start</td>
    <td rowspan="2">rowspan="2"  Date Created</td>
    <td rowspan="3">rowspan="3"  (the later of those records present and considered)</td>
    <td rowspan="2" colspan="2" align="center">colspan="2" rowspan="2" align="center"  1</td>
    <td rowspan="2">rowspan="2"  ADJUST request</td>
    <td>Direction</td>
    <td>Received</td>
  </tr>
  <tr>
    <td>Reason</td>
    <td>!= (209, 220, 222, 273, 274, 275, 276)</td>
  </tr>
  <tr>
    <td>Date Posted</td>
    <td colspan="2" align="center">colspan="2" align="center"  2 (optional)</td>
    <td>SF (Cross-Reference)</td>
    <td>SCCF ID</td>
    <td>ADJUST request Cross-Reference SCCF ID</td>
  </tr>
  <tr>
    <td colspan="3">colspan="3"</td>
    <td></td>
    <td align="center">align="center"  3</td>
    <td>CNCLADJ request</td>
    <td colspan="2">colspan="2"  (none)</td>
  </tr>
  <tr>
    <td rowspan="2" colspan="3">colspan="3" rowspan="2"</td>
    <td rowspan="2">rowspan="2"</td>
    <td rowspan="2" align="center">rowspan="2" align="center"  4</td>
    <td rowspan="2">rowspan="2"  CNCLADJ response</td>
    <td>Action</td>
    <td>136 (Approved)</td>
  </tr>
  <tr>
    <td>Date Created</td>
    <td>&lt;= Interval end date</td>
  </tr>
  <tr>
    <td rowspan="3">rowspan="3"  Finish</td>
    <td rowspan="3">rowspan="3"  Date Created</td>
    <td rowspan="3">rowspan="3"  (the earlier of those records present and considered)</td>
    <td align="center">align="center"  3 (optional)</td>
    <td></td>
    <td>ADJUST response</td>
    <td>Direction</td>
    <td>Sent</td>
  </tr>
  <tr>
    <td rowspan="2" align="center">rowspan="2" align="center"  4</td>
    <td rowspan="2">rowspan="2"</td>
    <td rowspan="2">rowspan="2"  DF</td>
    <td>Disposition</td>
    <td>4 (Void)</td>
  </tr>
  <tr>
    <td>Streamline?</td>
    <td>!= Y</td>
  </tr>
</table>
<br  />
====Host Adjustments Processed \<= 7 Days====

##### Field

<table>
  <tr>
    <th>Points Possible</th>
    <td>5.00</td>
  </tr>
  <tr>
    <th>Standard Range</th>
    <td>95.0 - 99.5%</td>
  </tr>
  <tr>
    <th>Points Formula</th>
    <td>Linear (higher is better); includes no-volume logic</td>
  </tr>
  <tr>
    <th>Score Formula</th>
    <td>Percentage</td>
  </tr>
  <tr>
    <th>Class</th>
    <td>% Processed</td>
  </tr>
  <tr>
    <th>Description</th>
    <td>As a Host Plan as a receiver, the total non-cancelled Blue² Adjustment requests processed within 7 calendar days of receipt divided by the total non-cancelled Blue² Adjustments Processed.
      <br />Calculation of cycle time (days) is based on Host Plan Adjustment request received message Create Date to either (in this hierarchy):
      <br />- Adjustment Approval or Denial response sent message Create Date
      <br />- Host Void DF Post Date
    </td>
  </tr>
  <tr>
    <th>Considerations</th>
    <td>Cancelled Approved Adjustment requests are excluded. High Volume Adjustments are excluded. Streamlined Adjustments are excluded. NASCO-to-NASCO adjustments are incorporated into this measure based on a total sum of total NASCO-to-NASCO Par and Blue²/ITS Host Adjustments Processed with 7 calendar days and a total sum of total NASCO-to-NASCO Par and Blue² Host Adjustments Processed.</td>
  </tr>
</table>
<br  />

##### Inputs

<table>
  <tr>
    <th rowspan="2">rowspan="2"  Title</th>
    <th rowspan="2">rowspan="2"  Calculated?</th>
    <th rowspan="2">rowspan="2"  Shared with Enhanced IPP Scorecard 2012?</th>
    <th colspan="2">colspan="2"  Interval Length</th>
  </tr>
  <tr>
    <th>Smallest Cumulative</th>
    <th>Frequency</th>
  </tr>
  <tr>
    <td>Host Adjustment Messages Proc &lt;= 7 Days - as receiver</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>Month-to-Date</td>
    <td>daily</td>
  </tr>
  <tr>
    <td>NASCO Par Adjustment Messages Proc &lt;= 7 Days - as receiver</td>
    <td>No</td>
    <td>Yes</td>
    <td>Month-to-Date</td>
    <td>daily</td>
  </tr>
  <tr>
    <td>Host Adjustment Messages Proc Total - as receiver</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>Month-to-Date</td>
    <td>daily</td>
  </tr>
  <tr>
    <td>NASCO Par Adjustment Messages Proc Total - as receiver</td>
    <td>No</td>
    <td>Yes</td>
    <td>Month-to-Date</td>
    <td>daily</td>
  </tr>
</table>
<br  />
##### Timeline and Restrictions

Only Path A counts towards this input. If the adjustment cancellation
request and response are present (Path B), the input excludes the
corresponding adjustment response from consideration.

This considers request-response pairs whether or not the request is
closed.

Note: The only differences from Home Adjustments Processed \<= 14 Days
are the perspective, Reason Codes, and age cut-off, as well as no
consideration of a Cross-Reference SF.

<table>
  <tr>
    <th rowspan="2">rowspan="2"</th>
    <th rowspan="2" colspan="2">colspan="2" rowspan="2"  Date Field</th>
    <th colspan="2">colspan="2"  Order</th>
    <th rowspan="2">rowspan="2"  Record</th>
    <th colspan="2">colspan="2"  Restriction</th>
  </tr>
  <tr>
    <th>Path A</th>
    <th>Path B</th>
    <th>Field</th>
    <th>Criteria</th>
  </tr>
  <tr>
    <td rowspan="2">rowspan="2"  Start</td>
    <td rowspan="2">rowspan="2"  Date Created</td>
    <td rowspan="2">rowspan="2"  (the later of those records present and considered)</td>
    <td rowspan="2" colspan="2" align="center">colspan="2" rowspan="2" align="center"  1</td>
    <td rowspan="2">rowspan="2"  ADJUST request</td>
    <td>Direction</td>
    <td>Received</td>
  </tr>
  <tr>
    <td>Reason</td>
    <td>!= (209, 220, 222, 273, 274, 275)</td>
  </tr>
  <tr>
    <td colspan="3">colspan="3"</td>
    <td></td>
    <td align="center">align="center"  2</td>
    <td>CNCLADJ request</td>
    <td colspan="2">colspan="2"  (none)</td>
  </tr>
  <tr>
    <td rowspan="2" colspan="3">colspan="3" rowspan="2"</td>
    <td rowspan="2">rowspan="2"</td>
    <td rowspan="2" align="center">rowspan="2" align="center"  3</td>
    <td rowspan="2">rowspan="2"  CNCLADJ response</td>
    <td>Action</td>
    <td>136 (Approved)</td>
  </tr>
  <tr>
    <td>Date Created</td>
    <td>&lt;= Interval end date</td>
  </tr>
  <tr>
    <td rowspan="3">rowspan="3"  Finish</td>
    <td>Date Created</td>
    <td rowspan="3">rowspan="3"  (the earlier of those records present and considered)</td>
    <td align="center">align="center"  2 (optional)</td>
    <td></td>
    <td>ADJUST response</td>
    <td>Direction</td>
    <td>Sent</td>
  </tr>
  <tr>
    <td rowspan="2">rowspan="2"  Date Posted</td>
    <td rowspan="2" align="center">rowspan="2" align="center"  3</td>
    <td rowspan="2">rowspan="2"</td>
    <td rowspan="2">rowspan="2"  DF</td>
    <td>Disposition</td>
    <td>4 (Void)</td>
  </tr>
  <tr>
    <td>Streamline?</td>
    <td>!= Y</td>
  </tr>
</table>
<br  />
====Home MED REC Claims Adjudicated \<= 30 Days====

##### Field

<table>
  <tr>
    <th>Points Possible</th>
    <td>5.00</td>
  </tr>
  <tr>
    <th>Standard Range</th>
    <td>85 - 99%</td>
  </tr>
  <tr>
    <th>Points Formula</th>
    <td>Linear (higher is better); includes no-volume logic</td>
  </tr>
  <tr>
    <th>Score Formula</th>
    <td>Percentage</td>
  </tr>
  <tr>
    <th>Class</th>
    <td>% Processed</td>
  </tr>
  <tr>
    <th>Description</th>
    <td>The percent of claims, for which medical records were requested and received, that are adjudicated within 30 calendar days of receipt of the medical records via Blue².</td>
  </tr>
  <tr>
    <th>Considerations</th>
    <td>Start Time - Home Plan Receipt of Medical Records is determined by one of the following ways, the latter of:
      <br />- The last Medical Record response from the Host Plan closing the parent Medical Record request; or
      <br />- The response from the Host Plan comes in the form of an Info message with message code 173 or 178
      <br />
      <br />End Time/Reporting Date - Home Plan Claim Processing Post Date is determined by one of the following ways:
      <br />- The claim is processed by the Original DF Home Plan Post Date, upon the receipt of medical records; or
      <br />- The claim is processed by creating an adjusted DF. The adjustment for the DF may come in the form of a Streamline Adjustment or an Adjustment request and both must include the adjustment Reason Code 240
      <br />
      <br />One claim can have multiple medical record events where each event is captured as a separate transaction. Exclusions:
      <br />- Claims without a Medical Record request (either never present or cancelled), regardless of the presence of Info message with a Reason Code of 173 or 178
      <br />- Claims adjusted with Reason Code 240, but without a valid Medical Record request 165 or 171 present on Formats Database/Blue²
      <br />- Adjusted DFs using any other Reason Code than 240
    </td>
  </tr>
</table>
<br  />

##### Inputs

<table>
  <tr>
    <th rowspan="2">rowspan="2"  Title</th>
    <th rowspan="2">rowspan="2"  Calculated?</th>
    <th rowspan="2">rowspan="2"  Shared with Enhanced IPP Scorecard 2012?</th>
    <th colspan="2">colspan="2"  Interval Length</th>
  </tr>
  <tr>
    <th>Smallest Cumulative</th>
    <th>Frequency</th>
  </tr>
  <tr>
    <td>Home MED REC Claims Adjudicated &lt;= 30 Days</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>Month-to-Date</td>
    <td>daily</td>
  </tr>
  <tr>
    <td>Home MED REC Claims Adjudicated Total</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>Month-to-Date</td>
    <td>daily</td>
  </tr>
</table>
<br  />
##### Timeline and Restrictions

Both Path A and Path B count towards this input; a Plan can respond to
the MEDREC request with either a MEDREC response or an INFO one-way.

<table>
  <tr>
    <th rowspan="2">rowspan="2"</th>
    <th rowspan="2">rowspan="2"  Date Field</th>
    <th colspan="2">colspan="2"  Order</th>
    <th rowspan="2">rowspan="2"  Record</th>
    <th colspan="2">colspan="2"  Restriction</th>
  </tr>
  <tr>
    <th>Path A</th>
    <th>Path B</th>
    <th>Field</th>
    <th>Criteria</th>
  </tr>
  <tr>
    <td rowspan="2">rowspan="2"</td>
    <td rowspan="2">rowspan="2"</td>
    <td rowspan="2" colspan="2" align="center">colspan="2" rowspan="2" align="center"  1</td>
    <td rowspan="2">rowspan="2"  MEDREC Request</td>
    <td>Date Closed</td>
    <td>&lt;= DF Date Created</td>
  </tr>
  <tr>
    <td>Direction</td>
    <td>Sent</td>
  </tr>
  <tr>
    <td rowspan="7">rowspan="7"  Start</td>
    <td rowspan="7">rowspan="7"  Date Created (the later from those records present and considered)</td>
    <td rowspan="3" align="center">rowspan="3" align="center"  2</td>
    <td rowspan="3">rowspan="3"</td>
    <td rowspan="3">rowspan="3"  MEDREC Response</td>
    <td>Date Created</td>
    <td>Final: The request can have multiple responses; only consider the response with the latest Date Created. (Request must be closed for latest to be "Final"; already covered by request-response pairing above.)</td>
  </tr>
  <tr>
    <td>Date Created</td>
    <td>&lt;= DF Date Created</td>
  </tr>
  <tr>
    <td>Direction</td>
    <td>Received</td>
  </tr>
  <tr>
    <td rowspan="4">rowspan="4"</td>
    <td rowspan="4" align="center">rowspan="4" align="center"  2</td>
    <td rowspan="4">rowspan="4"  INFO One-way</td>
    <td>Date Created</td>
    <td>&gt;= MEDREC Request Date Created</td>
  </tr>
  <tr>
    <td>Date Created</td>
    <td>&lt;= DF Date Created</td>
  </tr>
  <tr>
    <td>Direction</td>
    <td>Received</td>
  </tr>
  <tr>
    <td>Reason</td>
    <td>173, 178</td>
  </tr>
  <tr>
    <td rowspan="2">rowspan="2"  Finish</td>
    <td rowspan="2">rowspan="2"  Date Created</td>
    <td rowspan="2" colspan="2" align="center">colspan="2" rowspan="2" align="center"  3</td>
    <td rowspan="2">rowspan="2"  DF</td>
    <td>Disposition</td>
    <td>1 (Non-Void)</td>
  </tr>
  <tr>
    <td>Reason Adj</td>
    <td>(blank), 40</td>
  </tr>
</table>
<br  />
====Host Misroutes Processed \<= 10 Days====

##### Field

<table>
  <tr>
    <th>Points Possible</th>
    <td>5.00</td>
  </tr>
  <tr>
    <th>Standard Range</th>
    <td>90 - 98%</td>
  </tr>
  <tr>
    <th>Points Formula</th>
    <td>Linear (higher is better); includes no-volume logic</td>
  </tr>
  <tr>
    <th>Score Formula</th>
    <td>Percentage</td>
  </tr>
  <tr>
    <th>Class</th>
    <td>% Processed</td>
  </tr>
  <tr>
    <th>Description</th>
    <td>As a Host Plan as a receiver, the total Blue² misroute messages processed within 10 calendar days of receipt from the partner Home Plan divided by the total Blue² misroute messages processed.
      <br />
      <br />Processed is defined as a misroute message response that has been sent to the partner Home Plan.</td>
  </tr>
  <tr>
    <th>Considerations</th>
    <td>Start Time - Host Plan Receipt message Create Date of a Misrouted Claim message from Home Plan in Blue². End Time - Host Plan response message Create Date to the Home Plan Misrouted Claim message in Blue².</td>
  </tr>
</table>
<br  />

##### Inputs

<table>
  <tr>
    <th rowspan="2">rowspan="2"  Title</th>
    <th rowspan="2">rowspan="2"  Calculated?</th>
    <th rowspan="2">rowspan="2"  Shared with Enhanced IPP Scorecard 2012?</th>
    <th colspan="2">colspan="2"  Interval Length</th>
  </tr>
  <tr>
    <th>Smallest Cumulative</th>
    <th>Frequency</th>
  </tr>
  <tr>
    <td>Host Misroutes Processed &lt;= 10 Days</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>Month-to-Date</td>
    <td>daily</td>
  </tr>
  <tr>
    <td>Host Misroutes Processed Total</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>Month-to-Date</td>
    <td>daily</td>
  </tr>
</table>
<br  />
##### Timeline and Restrictions

<table>
  <tr>
    <th rowspan="2">rowspan="2"</th>
    <th rowspan="2">rowspan="2"  Date Field</th>
    <th rowspan="2">rowspan="2"  Order</th>
    <th rowspan="2">rowspan="2"  Record</th>
    <th colspan="2">colspan="2"  Restriction</th>
  </tr>
  <tr>
    <th>Field</th>
    <th>Criteria</th>
  </tr>
  <tr>
    <td>Start</td>
    <td>Date Created</td>
    <td align="center">align="center"  1</td>
    <td>MISROUTE Request</td>
    <td>Direction</td>
    <td>Received</td>
  </tr>
  <tr>
    <td>Finish</td>
    <td>Date Created</td>
    <td align="center">align="center"  2</td>
    <td>MISROUTE Response</td>
    <td>Direction</td>
    <td>Sent</td>
  </tr>
</table>
<br  />
====Home Claim Appeals Resolved \<= 20====

##### Field

<table>
  <tr>
    <th>Points Possible</th>
    <td>5.00</td>
  </tr>
  <tr>
    <th>Standard Range</th>
    <td>85.0 - 97.0%</td>
  </tr>
  <tr>
    <th>Points Formula</th>
    <td>Linear (higher is better); includes no-volume logic</td>
  </tr>
  <tr>
    <th>Score Formula</th>
    <td>Percentage</td>
  </tr>
  <tr>
    <th>Class</th>
    <td>% Processed</td>
  </tr>
  <tr>
    <th>Description</th>
    <td>As a Home Plan as a receiver, the total Blue² Claim Appeals processed within 20 calendar days divided by the total Home Blue² Claim Appeals processed.</td>
  </tr>
  <tr>
    <th>Considerations</th>
    <td>The reporting date is based off the Home Plan Claim Appeals response message Create Date. Only request messages that are 'Closed' messages (Open/Closed? = 2) are included. Claim Appeals in a processed state are included. Claim Appeals in a retry state are excluded. The Host Plan is the initiator and sends a request to the Home Plan. The Start Time (trigger) is the Home Plan Receipt Date of the Claim Appeal request. The Stop Time is the Home Plan Response Date to the open request.</td>
  </tr>
</table>
<br  />
##### Inputs

<table>
  <tr>
    <th rowspan="2">rowspan="2"  Title</th>
    <th rowspan="2">rowspan="2"  Calculated?</th>
    <th rowspan="2">rowspan="2"  Shared with Enhanced IPP Scorecard 2012?</th>
    <th colspan="2">colspan="2"  Interval Length</th>
  </tr>
  <tr>
    <th>Smallest Cumulative</th>
    <th>Frequency</th>
  </tr>
  <tr>
    <td>Home Claim Appeals Resolved &lt;= 20 Days</td>
    <td>Yes</td>
    <td>No</td>
    <td>Month-to-Date</td>
    <td>daily</td>
  </tr>
  <tr>
    <td>Home Claim Appeals Resolved Total</td>
    <td>Yes</td>
    <td>No</td>
    <td>Month-to-Date</td>
    <td>daily</td>
  </tr>
</table>
<br  />
##### Timeline and Restrictions

<table>
  <tr>
    <th rowspan="2">rowspan="2"</th>
    <th rowspan="2">rowspan="2"  Date Field</th>
    <th rowspan="2">rowspan="2"  Order</th>
    <th rowspan="2">rowspan="2"  Record</th>
    <th colspan="2">colspan="2"  Restriction</th>
  </tr>
  <tr>
    <th>Field</th>
    <th>Criteria</th>
  </tr>
  <tr>
    <td>Start</td>
    <td>Date Created</td>
    <td align="center">align="center"  1</td>
    <td>CLMAPPL Request</td>
    <td>Direction</td>
    <td>Received</td>
  </tr>
  <tr>
    <td>Finish</td>
    <td>Date Created</td>
    <td align="center">align="center"  2</td>
    <td>CLMAPPL Response</td>
    <td>Direction</td>
    <td>Sent</td>
  </tr>
</table>
<br  />
### Plan-to-Plan Inquiry

====Home GEN INQ Finalized \<= 10 Days====

##### Field

<table>
  <tr>
    <th>Points Possible</th>
    <td>5.50</td>
  </tr>
  <tr>
    <th>Standard Range</th>
    <td>90.0 - 97.0%</td>
  </tr>
  <tr>
    <th>Points Formula</th>
    <td>Linear (higher is better); includes no-volume logic</td>
  </tr>
  <tr>
    <th>Score Formula</th>
    <td>Percentage</td>
  </tr>
  <tr>
    <th>Class</th>
    <td>% Processed</td>
  </tr>
  <tr>
    <th>Description</th>
    <td>As a Home Plan as a receiver, the total Blue² General Inquiries processed within 10 calendar days divided by the total Home Blue² General Inquiries processed.</td>
  </tr>
  <tr>
    <th>Considerations</th>
    <td>The reporting date is based off the Home Plan General Inquiry response message Create Date. Only request messages that are 'Closed' messages (Open/Closed? = 2) are included. General Inquiries in a processed state are included. General Inquiries in a retry state are excluded. General Inquiries with Reason Code 168 are excluded. The Host Plan is the initiator and sends a request to the Home Plan. The Start Time (trigger) is the Home Plan Receipt Date of the General Inquiry request. The Stop Time is the Home Plan Response Date to the open request.</td>
  </tr>
</table>
<br  />
##### Inputs

<table>
  <tr>
    <th rowspan="2">rowspan="2"  Title</th>
    <th rowspan="2">rowspan="2"  Calculated?</th>
    <th rowspan="2">rowspan="2"  Shared with Enhanced IPP Scorecard 2012?</th>
    <th colspan="2">colspan="2"  Interval Length</th>
  </tr>
  <tr>
    <th>Smallest Cumulative</th>
    <th>Frequency</th>
  </tr>
  <tr>
    <td>Home General Inquiries Proc &lt;= 10 Days</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>Month-to-Date</td>
    <td>daily</td>
  </tr>
  <tr>
    <td>Home General Inquiries Proc Total</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>Month-to-Date</td>
    <td>daily</td>
  </tr>
</table>
<br  />
##### Timeline and Restrictions

<table>
  <tr>
    <th rowspan="2">rowspan="2"</th>
    <th rowspan="2">rowspan="2"  Date Field</th>
    <th rowspan="2">rowspan="2"  Order</th>
    <th rowspan="2">rowspan="2"  Record</th>
    <th colspan="2">colspan="2"  Restriction</th>
  </tr>
  <tr>
    <th>Field</th>
    <th>Criteria</th>
  </tr>
  <tr>
    <td rowspan="2">rowspan="2"  Start</td>
    <td rowspan="2">rowspan="2"  Date Created</td>
    <td rowspan="2" align="center">rowspan="2" align="center"  1</td>
    <td rowspan="2">rowspan="2"  GENINQ Request</td>
    <td>Direction</td>
    <td>Received</td>
  </tr>
  <tr>
    <td>Reason</td>
    <td>!= 168</td>
  </tr>
  <tr>
    <td rowspan="2">rowspan="2"  Finish</td>
    <td rowspan="2">rowspan="2"  Date Created</td>
    <td rowspan="2" align="center">rowspan="2" align="center"  2</td>
    <td rowspan="2">rowspan="2"  GENINQ Response</td>
    <td>Direction</td>
    <td>Sent</td>
  </tr>
  <tr>
    <td>Reason</td>
    <td>!= 168</td>
  </tr>
</table>
<br  />
====Host GEN INQ Finalized \<= 10 Days====

##### Field

<table>
  <tr>
    <th>Points Possible</th>
    <td>5.50</td>
  </tr>
  <tr>
    <th>Standard Range</th>
    <td>90.0 - 97.0%</td>
  </tr>
  <tr>
    <th>Points Formula</th>
    <td>Linear (higher is better); includes no-volume logic</td>
  </tr>
  <tr>
    <th>Score Formula</th>
    <td>Percentage</td>
  </tr>
  <tr>
    <th>Class</th>
    <td>% Processed</td>
  </tr>
  <tr>
    <th>Description</th>
    <td>As a Host Plan as a receiver, the total Blue² General Inquiries processed within 10 calendar days divided by the total Host Blue² General Inquiries processed.</td>
  </tr>
  <tr>
    <th>Considerations</th>
    <td>The reporting date is based off the Host Plan General Inquiry response message Create Date. Only request messages that are 'Closed' messages (Open/Closed? = 2) are included. General Inquiries in a processed state are included. General Inquiries in a retry state are excluded. General Inquiries with Reason Code 168 are excluded. The Home Plan is the initiator and sends a request to the Host Plan. The Start Time (trigger) is the Host Plan Receipt Date of the General Inquiry request. The Stop Time is the Host Plan Response Date to the open request.</td>
  </tr>
</table>
<br  />
##### Inputs

<table>
  <tr>
    <th rowspan="2">rowspan="2"  Title</th>
    <th rowspan="2">rowspan="2"  Calculated?</th>
    <th rowspan="2">rowspan="2"  Shared with Enhanced IPP Scorecard 2012?</th>
    <th colspan="2">colspan="2"  Interval Length</th>
  </tr>
  <tr>
    <th>Smallest Cumulative</th>
    <th>Frequency</th>
  </tr>
  <tr>
    <td>Host General Inquiries Proc &lt;= 10 Days</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>Month-to-Date</td>
    <td>daily</td>
  </tr>
  <tr>
    <td>Host General Inquiries Proc Total</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>Month-to-Date</td>
    <td>daily</td>
  </tr>
</table>
<br  />
##### Timeline and Restrictions

<table>
  <tr>
    <th rowspan="2">rowspan="2"</th>
    <th rowspan="2">rowspan="2"  Date Field</th>
    <th rowspan="2">rowspan="2"  Order</th>
    <th rowspan="2">rowspan="2"  Record</th>
    <th colspan="2">colspan="2"  Restriction</th>
  </tr>
  <tr>
    <th>Field</th>
    <th>Criteria</th>
  </tr>
  <tr>
    <td>Start</td>
    <td>Date Created</td>
    <td align="center">align="center"  1</td>
    <td>GENINQ Request</td>
    <td>Direction</td>
    <td>Received</td>
  </tr>
  <tr>
    <td>Finish</td>
    <td>Date Created</td>
    <td align="center">align="center"  2</td>
    <td>GENINQ Response</td>
    <td>Direction</td>
    <td>Sent</td>
  </tr>
</table>
<br  />
#### Average Speed of Answer

##### Field

<table>
  <tr>
    <th>Points Possible</th>
    <td>5.0</td>
  </tr>
  <tr>
    <th>Standard Range</th>
    <td>45 - 90 sec.</td>
  </tr>
  <tr>
    <th>Points Formula</th>
    <td>Linear (lower is better)</td>
  </tr>
  <tr>
    <th>Score Formula</th>
    <td>Adjusted average</td>
  </tr>
  <tr>
    <th>Class</th>
    <td>N/A</td>
  </tr>
  <tr>
    <th>Description</th>
    <td>The average time (in seconds) that a caller from another Plan must wait to connect to an agent. The clock starts when the caller enters the system queue after answering the last automatic telephone prompt.</td>
  </tr>
  <tr>
    <th>Considerations</th>
    <td>N/A</td>
  </tr>
</table>
<br  />
##### Inputs

<table>
  <tr>
    <th rowspan="2">rowspan="2"  Title</th>
    <th rowspan="2">rowspan="2"  Calculated?</th>
    <th rowspan="2">rowspan="2"  Shared with Enhanced IPP Scorecard 2012?</th>
    <th colspan="2">colspan="2"  Interval Length</th>
  </tr>
  <tr>
    <th>Smallest Cumulative</th>
    <th>Frequency</th>
  </tr>
  <tr>
    <td>Average Speed of Answer (seconds)</td>
    <td>No</td>
    <td>Yes</td>
    <td>Month-to-Date</td>
    <td>daily</td>
  </tr>
  <tr>
    <td>Average Speed of Answer Volume</td>
    <td>No</td>
    <td>Yes</td>
    <td>Month-to-Date</td>
    <td>daily</td>
  </tr>
</table>
<br  />
====Inquiries Resolved \<= 14 Days====

##### Field

<table>
  <tr>
    <th>Points Possible</th>
    <td>9.0</td>
  </tr>
  <tr>
    <th>Standard Range</th>
    <td>90 - 98%</td>
  </tr>
  <tr>
    <th>Points Formula</th>
    <td>Linear (higher is better)</td>
  </tr>
  <tr>
    <th>Score Formula</th>
    <td>Percentage</td>
  </tr>
  <tr>
    <th>Class</th>
    <td>% Processed</td>
  </tr>
  <tr>
    <th>Description</th>
    <td>The percent of total Plan-to-Plan inquiries resolved in 14 calendar days or less.</td>
  </tr>
  <tr>
    <th>Considerations</th>
    <td>Percentage is determined by taking the total number of inquiries resolved within 14 calendar days and dividing it by the total number of inquiries resolved within the reporting period. The measure includes calls resolved up to and including day 14.</td>
  </tr>
</table>
<br  />
##### Inputs

<table>
  <tr>
    <th rowspan="2">rowspan="2"  Title</th>
    <th rowspan="2">rowspan="2"  Calculated?</th>
    <th rowspan="2">rowspan="2"  Shared with Enhanced IPP Scorecard 2012?</th>
    <th colspan="2">colspan="2"  Interval Length</th>
  </tr>
  <tr>
    <th>Smallest Cumulative</th>
    <th>Frequency</th>
  </tr>
  <tr>
    <td>Home/Control Inquiries Resolved &lt;= 14 Days</td>
    <td>No</td>
    <td>No</td>
    <td>Month-to-Date</td>
    <td>daily</td>
  </tr>
  <tr>
    <td>Host/Par Inquiries Resolved &lt;= 14 Days</td>
    <td>No</td>
    <td>No</td>
    <td>Month-to-Date</td>
    <td>daily</td>
  </tr>
  <tr>
    <td>Home/Control Inquiries Resolved Total</td>
    <td>No</td>
    <td>Yes</td>
    <td>Month-to-Date</td>
    <td>daily</td>
  </tr>
  <tr>
    <td>Host/Par Inquiries Resolved Total</td>
    <td>No</td>
    <td>Yes</td>
    <td>Month-to-Date</td>
    <td>daily</td>
  </tr>
</table>
<br  />
### End-to-End Claim Experience

====Claims/Adjustments Finalized \<= 30 Days====

##### Field

<table>
  <tr>
    <th>Points Possible</th>
    <td>12.50</td>
  </tr>
  <tr>
    <th>Standard Range</th>
    <td>90 - 96%</td>
  </tr>
  <tr>
    <th>Points Formula</th>
    <td>Linear (higher is better)</td>
  </tr>
  <tr>
    <th>Score Formula</th>
    <td>Percentage</td>
  </tr>
  <tr>
    <th>Class</th>
    <td>% Processed</td>
  </tr>
  <tr>
    <th>Description</th>
    <td>The percent of all End-to-End Inter-Plan claims finalized in 30 calendar days including all adjustments to the claim based on MTM consistent definition of First Day of Processing (FDP) and Last Day of Processing (LDP).</td>
  </tr>
  <tr>
    <th>Considerations</th>
    <td>Percentage is determined by taking the total number of claims and adjustments processed within 30 days and dividing it by the total number of claims and adjustments processed within the reporting period. The measure includes claims and adjustments processed up to and including day 30.</td>
  </tr>
</table>
<br  />
##### Inputs

<table>
  <tr>
    <th rowspan="2">rowspan="2"  Title</th>
    <th rowspan="2">rowspan="2"  Calculated?</th>
    <th rowspan="2">rowspan="2"  Shared with Enhanced IPP Scorecard 2012?</th>
    <th colspan="2">colspan="2"  Interval Length</th>
  </tr>
  <tr>
    <th>Smallest Cumulative</th>
    <th>Frequency</th>
  </tr>
  <tr>
    <td>ITS Claims/Adjustments Proc &lt;= 30 Days (QtD)</td>
    <td>No</td>
    <td>Yes</td>
    <td>Quarter-to-Date</td>
    <td>daily</td>
  </tr>
  <tr>
    <td>NASCO Control Original Claims Proc &lt;= 30 Days (QtD)</td>
    <td>No</td>
    <td>Yes</td>
    <td>Quarter-to-Date</td>
    <td>daily</td>
  </tr>
  <tr>
    <td>NASCO Par Original Claims Proc &lt;= 30 Days (QtD)</td>
    <td>No</td>
    <td>Yes</td>
    <td>Quarter-to-Date</td>
    <td>daily</td>
  </tr>
  <tr>
    <td>ITS Claims/Adjustments Proc Total (QtD)</td>
    <td>No</td>
    <td>Yes</td>
    <td>Quarter-to-Date</td>
    <td>daily</td>
  </tr>
  <tr>
    <td>NASCO Control Original Claims Proc Total (QtD)</td>
    <td>No</td>
    <td>Yes</td>
    <td>Quarter-to-Date</td>
    <td>daily</td>
  </tr>
  <tr>
    <td>NASCO Par Original Claims Proc Total (QtD)</td>
    <td>No</td>
    <td>Yes</td>
    <td>Quarter-to-Date</td>
    <td>daily</td>
  </tr>
</table>
<br  />
====Claims/Adjustments Finalized \<= 60 Days====

##### Field

<table>
  <tr>
    <th>Points Possible</th>
    <td>12.50</td>
  </tr>
  <tr>
    <th>Standard Range</th>
    <td>96.0 - 98.5%</td>
  </tr>
  <tr>
    <th>Points Formula</th>
    <td>Linear (higher is better)</td>
  </tr>
  <tr>
    <th>Score Formula</th>
    <td>Percentage</td>
  </tr>
  <tr>
    <th>Class</th>
    <td>% Processed</td>
  </tr>
  <tr>
    <th>Description</th>
    <td>The percent of all End-to-End Inter-Plan claims finalized in 60 calendar days including all adjustments to the claim based on MTM consistent definition of First Day of Processing (FDP) and Last Day of Processing (LDP).</td>
  </tr>
  <tr>
    <th>Considerations</th>
    <td>Percentage is determined by taking the total number of claims and adjustments processed within 60 days and dividing it by the total number of claims and adjustments processed within the reporting period. The measure includes claims and adjustments processed up to and including day 60.</td>
  </tr>
</table>
<br  />
##### Inputs

<table>
  <tr>
    <th rowspan="2">rowspan="2"  Title</th>
    <th rowspan="2">rowspan="2"  Calculated?</th>
    <th rowspan="2">rowspan="2"  Shared with Enhanced IPP Scorecard 2012?</th>
    <th colspan="2">colspan="2"  Interval Length</th>
  </tr>
  <tr>
    <th>Smallest Cumulative</th>
    <th>Frequency</th>
  </tr>
  <tr>
    <td>ITS Claims/Adjustments Proc &lt;= 60 Days (QtD)</td>
    <td>No</td>
    <td>Yes</td>
    <td>Quarter-to-Date</td>
    <td>daily</td>
  </tr>
  <tr>
    <td>NASCO Control Original Claims Proc &lt;= 60 Days (QtD)</td>
    <td>No</td>
    <td>Yes</td>
    <td>Quarter-to-Date</td>
    <td>daily</td>
  </tr>
  <tr>
    <td>NASCO Par Original Claims Proc &lt;= 60 Days (QtD)</td>
    <td>No</td>
    <td>Yes</td>
    <td>Quarter-to-Date</td>
    <td>daily</td>
  </tr>
  <tr>
    <td>ITS Claims/Adjustments Proc Total (QtD)</td>
    <td>No</td>
    <td>Yes</td>
    <td>Quarter-to-Date</td>
    <td>daily</td>
  </tr>
  <tr>
    <td>NASCO Control Original Claims Proc Total (QtD)</td>
    <td>No</td>
    <td>Yes</td>
    <td>Quarter-to-Date</td>
    <td>daily</td>
  </tr>
  <tr>
    <td>NASCO Par Original Claims Proc Total (QtD)</td>
    <td>No</td>
    <td>Yes</td>
    <td>Quarter-to-Date</td>
    <td>daily</td>
  </tr>
</table>
<br  />
### Licensee Desk-Level Audit

#### Financial Dollar Accuracy

##### Field

<table>
  <tr>
    <th>Points Possible</th>
    <td>10.00</td>
  </tr>
  <tr>
    <th>Standard Range</th>
    <td>97 - 99%</td>
  </tr>
  <tr>
    <th>Points Formula</th>
    <td>Linear (higher is better)</td>
  </tr>
  <tr>
    <th>Score Formula</th>
    <td>Adjusted average</td>
  </tr>
  <tr>
    <th>Class</th>
    <td>Percent</td>
  </tr>
  <tr>
    <th>Description</th>
    <td>The percent of discount dollars passed correctly on claims audited.</td>
  </tr>
  <tr>
    <th>Considerations</th>
    <td>LDLA financial dollar accuracy is calculated based on audit results submitted by each Plan. Margin of error has been removed to more accurately reflect a Plan's actual performance.</td>
  </tr>
</table>
<br  />
##### Inputs

<table>
  <tr>
    <th rowspan="2">rowspan="2"  Title</th>
    <th rowspan="2">rowspan="2"  Calculated?</th>
    <th rowspan="2">rowspan="2"  Shared with Enhanced IPP Scorecard 2012?</th>
    <th colspan="2">colspan="2"  Interval Length</th>
  </tr>
  <tr>
    <th>Smallest Cumulative</th>
    <th>Frequency</th>
  </tr>
  <tr>
    <td>LDLA Financial Dollar Accuracy (quarterly)</td>
    <td>No</td>
    <td>Yes</td>
    <td>Month-to-Date</td>
    <td>quarterly</td>
  </tr>
  <tr>
    <td>LDLA Financial Dollar Accuracy Volume (quarterly)</td>
    <td>No</td>
    <td>Yes</td>
    <td>Month-to-Date</td>
    <td>quarterly</td>
  </tr>
</table>
<br  />
#### Data Accuracy

##### Field

<table>
  <tr>
    <th>Points Possible</th>
    <td>5.00</td>
  </tr>
  <tr>
    <th>Standard Range</th>
    <td>90 - 95%</td>
  </tr>
  <tr>
    <th>Points Formula</th>
    <td>Linear (higher is better)</td>
  </tr>
  <tr>
    <th>Score Formula</th>
    <td>Adjusted average</td>
  </tr>
  <tr>
    <th>Class</th>
    <td>Percent</td>
  </tr>
  <tr>
    <th>Description</th>
    <td>The percent of claims audited correctly.</td>
  </tr>
  <tr>
    <th>Considerations</th>
    <td>LDLA data accuracy is calculated based on audit results submitted by each Plan.</td>
  </tr>
</table>
<br  />
##### Inputs

<table>
  <tr>
    <th rowspan="2">rowspan="2"  Title</th>
    <th rowspan="2">rowspan="2"  Calculated?</th>
    <th rowspan="2">rowspan="2"  Shared with Enhanced IPP Scorecard 2012?</th>
    <th colspan="2">colspan="2"  Interval Length</th>
  </tr>
  <tr>
    <th>Smallest Cumulative</th>
    <th>Frequency</th>
  </tr>
  <tr>
    <td>LDLA Data Accuracy (quarterly)</td>
    <td>No</td>
    <td>Yes</td>
    <td>Month-to-Date</td>
    <td>quarterly</td>
  </tr>
  <tr>
    <td>LDLA Data Accuracy Volume (quarterly)</td>
    <td>No</td>
    <td>Yes</td>
    <td>Month-to-Date</td>
    <td>quarterly</td>
  </tr>
</table>
<br  />
#### Home Financial Dollar Accuracy

##### Field

<table>
  <tr>
    <th>Points Possible</th>
    <td>0.00 (informational)</td>
  </tr>
  <tr>
    <th>Standard Range</th>
    <td>TBD</td>
  </tr>
  <tr>
    <th>Points Formula</th>
    <td>Linear (higher is better)</td>
  </tr>
  <tr>
    <th>Score Formula</th>
    <td>Adjusted average</td>
  </tr>
  <tr>
    <th>Class</th>
    <td>Percent</td>
  </tr>
  <tr>
    <th>Description</th>
    <td>The percent of discount dollars passed correctly on Home claims audited.</td>
  </tr>
  <tr>
    <th>Considerations</th>
    <td>LDLA Home financial dollar accuracy is calculated based on audit results submitted by each Plan. Margin of error has been removed to more accurately reflect a Plan's actual performance.</td>
  </tr>
</table>
<br  />
##### Inputs

<table>
  <tr>
    <th rowspan="2">rowspan="2"  Title</th>
    <th rowspan="2">rowspan="2"  Calculated?</th>
    <th rowspan="2">rowspan="2"  Shared with Enhanced IPP Scorecard 2012?</th>
    <th colspan="2">colspan="2"  Interval Length</th>
  </tr>
  <tr>
    <th>Smallest Cumulative</th>
    <th>Frequency</th>
  </tr>
  <tr>
    <td>LDLA Home Financial Dollar Accuracy (quarterly)</td>
    <td>No</td>
    <td>No</td>
    <td>Month-to-Date</td>
    <td>quarterly</td>
  </tr>
  <tr>
    <td>LDLA Home Financial Dollar Accuracy Volume (quarterly)</td>
    <td>No</td>
    <td>No</td>
    <td>Month-to-Date</td>
    <td>quarterly</td>
  </tr>
</table>
<br  />
