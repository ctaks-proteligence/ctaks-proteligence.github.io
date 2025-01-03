<h2 id="overview">Overview</h2>
<p>The National Programs Scorecard is available for Month-to-Date,
Quarter-to-Date, and Half-to-Date (2 intervals per year, the first being
quarters 1 and 2 and the second being quarters 3 and 4). However, not
all inputs are available Month-to-Date. The list of inputs below
indicates each input's smallest available interval length. All inputs
are calculated/entered for the smallest available interval length and
then rolled up as necessary to the scorecard result's interval
length.</p>
<p>Each calculated input considers certain records (those ITS
transactions and/or Blue<sup>2</sup> messages listed below in the
timeline for that input) in each claim experience to determine whether
those records should count towards the input value. Unless otherwise
stated, a claim experience may contain records such that it matches the
input multiple times and therefore counts multiple times.</p>
<p>The minimum allowable value for all fields is 0, inclusive. The
maximum allowable value for percentage fields is 100% and for all other
fields is not defined.</p>
<h3 id="date_and_age">Date and Age</h3>
<p>Each input uses a Start Date and a Finish Date. The Finish Date must
be within the inclusive bounds of an interval to count towards the input
for that interval.</p>
<p>Currently, all calculated inputs come in pairs; each pair consists of
a Total and an age-restricted subset. The logic for the subset input
exactly matches that of the Total input except for the addition of the
age restriction indicated in the input's title. The age is calculated as
Finish Date - Start Date.</p>
<h3 id="joining">Joining</h3>
<p>With the exception of Adjustment SFs (used in the 2 Adjustment
Processed measures), all inputs join only transactions and messages
within the same claim experience. Therefore, all records possible for
joining except Adjustment SFs share the same SCCF Base.</p>
<p>For the purpose of tying records together within a claim experience
to form an input's timeline, use the first rule that applies:</p>
<ol>
<li>Join Blue<sup>2</sup> request and response messages of the same
message type using request Message ID and response Root Message ID
(unless otherwise mentioned, a request can only have one response).</li>
<li>Join Blue<sup>2</sup> adjustment request and adjustment cancellation
request using the previous rule, treating the adjustment request as the
request and the adjustment cancellation request as the response.</li>
<li>Join an ITS DF or RF transaction and Blue<sup>2</sup> adjustment or
adjustment cancellation message by SCCF ID.</li>
<li>Join ITS (except SF) transactions by SCCF ID.</li>
</ol>
<p>(Blue<sup>2</sup> messages not mentioned above and SFs always have
SCCF Suffix 00 and therefore do not have any joining logic beyond
sharing the same SCCF Base.)</p>
<p>Data issues can arise, such that the rule in combination with the
input timeline's additional restrictions are not sufficient to determine
a single record to use for each step in the input's timeline. If this
occurs, exclude those records from consideration for the input being
calculated; log that the data issue exists, including the relevant
information (BOID, SCCF Base, input ID, and input title).</p>
<h3 id="claim_experience_wide_restrictions">Claim Experience-wide
restrictions</h3>
<p>Regardless of input, only consider a transaction or message if it
meets the following criteria:</p>
<table>
<thead>
<tr class="header">
<th><p>Record</p></th>
<th><p>Field</p></th>
<th><p>Operator</p></th>
<th><p>Value</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>ITS and Blue<sup>2</sup></p></td>
<td><p>Perspective</p></td>
<td><p>==</p></td>
<td><p>(Measure's perspective)</p></td>
</tr>
<tr class="even">
<td><p>ITS and Blue<sup>2</sup></p></td>
<td><p>Program</p></td>
<td><p>!=</p></td>
<td><p>6</p></td>
</tr>
<tr class="odd">
<td><p>ITS (only SF, DF)</p></td>
<td><p>Estimate?</p></td>
<td><p>!=</p></td>
<td><p>E</p></td>
</tr>
<tr class="even">
<td><p>ITS</p></td>
<td><p>Status</p></td>
<td><p>==</p></td>
<td><p>V</p></td>
</tr>
</tbody>
</table>
<p>Additionally, an input for a processed measure considers a
Blue<sup>2</sup> message only if its latest Message Status is PRSD or
FNAL; and unless otherwise noted it considers a request-response pair
only if the request is closed (Open/Closed? == 2). Currently, all
calculated inputs belong to processed measures. (Blue<sup>2</sup>
messages move to FNAL only from PRSD; therefore, the PRSD or FNAL
restriction simply ensures the Blue<sup>2</sup> message has been
processed [i.e. PRSD exists in its Message State history].)</p>
<h2 id="categories_fields_and_inputs">Categories, Fields, and
Inputs</h2>
<h3 id="transactional">Transactional</h3>
<h4 id="home_dfs_processed_30_days">Home DFs Processed &gt; 30 Days</h4>
<h5 id="field">Field</h5>
<table>
<thead>
<tr class="header">
<th><p>Points Possible</p></th>
<th><p>5.00</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Standard Range</p></td>
<td><p>0.5 - 3.0%</p></td>
</tr>
<tr class="even">
<td><p>Points Formula</p></td>
<td><p>Linear (lower is better); includes no-volume logic</p></td>
</tr>
<tr class="odd">
<td><p>Score Formula</p></td>
<td><p>Percentage</p></td>
</tr>
<tr class="even">
<td><p>Class</p></td>
<td><p>% Processed</p></td>
</tr>
<tr class="odd">
<td><p>Description</p></td>
<td><p>The percentage of valid DFs created more than 30 calendar days
from receipt of SFs at the Home Plan.<br />
<br />
Calculation of days is based on Formats Database Home SF Receipt Post
Date to the Post Date of the valid DF.</p></td>
</tr>
<tr class="even">
<td><p>Considerations</p></td>
<td><p>Reporting Date is based on the Home DF Formats Post Date and Home
SF Post Date.</p></td>
</tr>
</tbody>
</table>
<h5 id="inputs">Inputs</h5>
<table>
<thead>
<tr class="header">
<th><p>Title</p></th>
<th><p>Calculated?</p></th>
<th><p>Shared with Enhanced IPP Scorecard 2012?</p></th>
<th><p>Interval Length</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Smallest Cumulative</p></td>
<td><p>Frequency</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>Home DF Proc &gt; 30 Days</p></td>
<td><p>Yes</p></td>
<td><p>No</p></td>
<td><p>Month-to-Date</p></td>
</tr>
<tr class="odd">
<td><p>Home DF Proc Total</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
<td><p>Month-to-Date</p></td>
</tr>
</tbody>
</table>
<h5 id="timeline_and_restrictions">Timeline and Restrictions</h5>
<p>Even though the SF is used only for calculating the Age for the
subset input, the SF must be present for the DF to count towards either
input.</p>
<p>Each claim experience can have at most one (valid) original DF, which
this measure targets; therefore, each claim experience can count at most
once towards each input for this measure.</p>
<table>
<thead>
<tr class="header">
<th></th>
<th><p>Date Field</p></th>
<th><p>Order</p></th>
<th><p>Record</p></th>
<th><p>Restriction</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Field</p></td>
<td><p>Criteria</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>Start</p></td>
<td><p>Date Posted</p></td>
<td style="text-align: center;"><p>1</p></td>
<td><p>SF</p></td>
<td><p>(none)</p></td>
</tr>
<tr class="odd">
<td rowspan="3"><p>Finish</p></td>
<td rowspan="3"><p>Date Created</p></td>
<td rowspan="3" style="text-align: center;"><p>2</p></td>
<td rowspan="3"><p>DF</p></td>
<td><p>SCCF Suffix</p></td>
</tr>
<tr class="even">
<td><p>Disposition</p></td>
</tr>
<tr class="odd">
<td><p>Adj Closeout DF?</p></td>
</tr>
</tbody>
</table>
<h4 id="host_sfs_processed_10_days">Host SFs Processed &gt; 10 Days</h4>
<h5 id="field_1">Field</h5>
<table>
<thead>
<tr class="header">
<th><p>Points Possible</p></th>
<th><p>5.00</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Standard Range</p></td>
<td><p>0.75 - 3.50%</p></td>
</tr>
<tr class="even">
<td><p>Points Formula</p></td>
<td><p>Linear (lower is better); includes no-volume logic</p></td>
</tr>
<tr class="odd">
<td><p>Score Formula</p></td>
<td><p>Percentage</p></td>
</tr>
<tr class="even">
<td><p>Class</p></td>
<td><p>% Processed</p></td>
</tr>
<tr class="odd">
<td><p>Description</p></td>
<td><p>The percentage of valid SFs processed aged greater than 10
calendar days from Host Claim Receipt Date to Host SF Post Date.<br />
<br />
Calculation of days is based on the number of calendar days from Host
Plan Claim Receipt Date to the date the corresponding valid SF is posted
to the Host Plan's Formats Database.</p></td>
</tr>
<tr class="even">
<td><p>Considerations</p></td>
<td><p>Reporting Date is based on the Host SF Post Date and Original
Claim Receipt Date.</p></td>
</tr>
</tbody>
</table>
<h5 id="inputs_1">Inputs</h5>
<table>
<thead>
<tr class="header">
<th><p>Title</p></th>
<th><p>Calculated?</p></th>
<th><p>Shared with Enhanced IPP Scorecard 2012?</p></th>
<th><p>Interval Length</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Smallest Cumulative</p></td>
<td><p>Frequency</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>Host SF Proc &gt; 10 Days</p></td>
<td><p>Yes</p></td>
<td><p>No</p></td>
<td><p>Month-to-Date</p></td>
</tr>
<tr class="odd">
<td><p>Host SF Proc Total</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
<td><p>Month-to-Date</p></td>
</tr>
</tbody>
</table>
<h5 id="timeline_and_restrictions_1">Timeline and Restrictions</h5>
<table>
<thead>
<tr class="header">
<th></th>
<th><p>Date Field</p></th>
<th><p>Order</p></th>
<th><p>Record</p></th>
<th><p>Restriction</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Field</p></td>
<td><p>Criteria</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>Start</p></td>
<td><p>Date Host Rcpt</p></td>
<td rowspan="2" style="text-align: center;"><p>1</p></td>
<td rowspan="2"><p>SF</p></td>
<td rowspan="2"><p>Submission Type</p></td>
</tr>
<tr class="odd">
<td><p>Finish</p></td>
<td><p>Date Created</p></td>
</tr>
</tbody>
</table>
<p>====Home Adjustments Processed &lt;= 14 Days====</p>
<h5 id="field_2">Field</h5>
<table>
<thead>
<tr class="header">
<th><p>Points Possible</p></th>
<th><p>5.00</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Standard Range</p></td>
<td><p>95.0 - 99.5%</p></td>
</tr>
<tr class="even">
<td><p>Points Formula</p></td>
<td><p>Linear (higher is better); includes no-volume logic</p></td>
</tr>
<tr class="odd">
<td><p>Score Formula</p></td>
<td><p>Percentage</p></td>
</tr>
<tr class="even">
<td><p>Class</p></td>
<td><p>% Processed</p></td>
</tr>
<tr class="odd">
<td><p>Description</p></td>
<td><p>As a Home Plan as a receiver, the total non-cancelled Blue²
Adjustment requests processed within 14 calendar days of receipt divided
by the total non-cancelled Blue² Adjustment requests processed.<br />
<br />
Calculation of cycle time (days) is based on either:<br />
- Home Plan Adjustment request received message Create Date; or<br />
- If the adjustment cross reference indicator on the adjustment is set
to Y, then the later of the Home Plan Adjustment request received
message Create Date or the Home Plan Formats Post Date of the Adjustment
SF<br />
<br />
To either (in this hierarchy):<br />
- Adjustment Approval response sent message Create Date;<br />
- Home Void DF Post Date; or<br />
- Adjustment Denial response sent message Create Date.</p></td>
</tr>
<tr class="even">
<td><p>Considerations</p></td>
<td><p>Cancelled Approved Adjustment requests are excluded. High Volume
Adjustments are excluded. Streamline Adjustments are excluded.
NASCO-to-NASCO adjustments are incorporated into this measure based on a
total sum of total NASCO-to-NASCO Control and Blue² Home Adjustments
Processed within 14 calendar days and a total sum of total
NASCO-to-NASCO Control and Blue² Home Adjustments Processed.</p></td>
</tr>
</tbody>
</table>
<h5 id="inputs_2">Inputs</h5>
<table>
<thead>
<tr class="header">
<th><p>Title</p></th>
<th><p>Calculated?</p></th>
<th><p>Shared with Enhanced IPP Scorecard 2012?</p></th>
<th><p>Interval Length</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Smallest Cumulative</p></td>
<td><p>Frequency</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>Home Adjustment Messages Proc &lt;= 14 Days - as
receiver</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
<td><p>Month-to-Date</p></td>
</tr>
<tr class="odd">
<td><p>NASCO Control Adjustment Messages Proc &lt;= 14 Days - as
receiver</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
<td><p>Month-to-Date</p></td>
</tr>
<tr class="even">
<td><p>Home Adjustment Messages Proc Total - as receiver</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
<td><p>Month-to-Date</p></td>
</tr>
<tr class="odd">
<td><p>NASCO Control Adjustment Messages Proc Total - as
receiver</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
<td><p>Month-to-Date</p></td>
</tr>
</tbody>
</table>
<h5 id="timeline_and_restrictions_2">Timeline and Restrictions</h5>
<p>Only Path A counts towards this input. If the adjustment cancellation
request and response are present (Path B), the input excludes the
corresponding adjustment response from consideration.</p>
<p>This considers request-response pairs whether or not the request is
closed.</p>
<p>The Cross-Reference SF is considered only if the adjustment request
has Adj SF? == Y.</p>
<p>Note: The only differences from Host Adjustments Processed &lt;= 7
Days are the perspective, Reason Codes, and age cut-off, as well as the
optional presence of the Cross-Reference SF.</p>
<table>
<thead>
<tr class="header">
<th></th>
<th colspan="2"><p>Date Field</p></th>
<th colspan="2"><p>Order</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Path A</p></td>
<td><p>Path B</p></td>
<td><p>Field</p></td>
<td><p>Criteria</p></td>
<td></td>
</tr>
<tr class="even">
<td rowspan="3"><p>Start</p></td>
<td rowspan="2"><p>Date Created</p></td>
<td rowspan="3"><p>(the later of those records present and
considered)</p></td>
<td colspan="2" rowspan="2" style="text-align: center;"><p>1</p></td>
</tr>
<tr class="odd">
</tr>
<tr class="even">
<td><p>Date Posted</p></td>
<td colspan="2" style="text-align: center;"><p>2 (optional)</p></td>
</tr>
<tr class="odd">
<td colspan="3"></td>
<td></td>
<td style="text-align: center;"><p>3</p></td>
</tr>
<tr class="even">
<td colspan="3" rowspan="2"></td>
<td rowspan="2"></td>
<td rowspan="2" style="text-align: center;"><p>4</p></td>
</tr>
<tr class="odd">
</tr>
<tr class="even">
<td rowspan="3"><p>Finish</p></td>
<td rowspan="3"><p>Date Created</p></td>
<td rowspan="3"><p>(the earlier of those records present and
considered)</p></td>
<td style="text-align: center;"><p>3 (optional)</p></td>
<td></td>
</tr>
<tr class="odd">
<td rowspan="2" style="text-align: center;"><p>4</p></td>
<td rowspan="2"></td>
</tr>
<tr class="even">
</tr>
</tbody>
</table>
<p>====Host Adjustments Processed &lt;= 7 Days====</p>
<h5 id="field_3">Field</h5>
<table>
<thead>
<tr class="header">
<th><p>Points Possible</p></th>
<th><p>5.00</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Standard Range</p></td>
<td><p>95.0 - 99.5%</p></td>
</tr>
<tr class="even">
<td><p>Points Formula</p></td>
<td><p>Linear (higher is better); includes no-volume logic</p></td>
</tr>
<tr class="odd">
<td><p>Score Formula</p></td>
<td><p>Percentage</p></td>
</tr>
<tr class="even">
<td><p>Class</p></td>
<td><p>% Processed</p></td>
</tr>
<tr class="odd">
<td><p>Description</p></td>
<td><p>As a Host Plan as a receiver, the total non-cancelled Blue²
Adjustment requests processed within 7 calendar days of receipt divided
by the total non-cancelled Blue² Adjustments Processed.<br />
Calculation of cycle time (days) is based on Host Plan Adjustment
request received message Create Date to either (in this
hierarchy):<br />
- Adjustment Approval or Denial response sent message Create Date<br />
- Host Void DF Post Date</p></td>
</tr>
<tr class="even">
<td><p>Considerations</p></td>
<td><p>Cancelled Approved Adjustment requests are excluded. High Volume
Adjustments are excluded. Streamlined Adjustments are excluded.
NASCO-to-NASCO adjustments are incorporated into this measure based on a
total sum of total NASCO-to-NASCO Par and Blue²/ITS Host Adjustments
Processed with 7 calendar days and a total sum of total NASCO-to-NASCO
Par and Blue² Host Adjustments Processed.</p></td>
</tr>
</tbody>
</table>
<h5 id="inputs_3">Inputs</h5>
<table>
<thead>
<tr class="header">
<th><p>Title</p></th>
<th><p>Calculated?</p></th>
<th><p>Shared with Enhanced IPP Scorecard 2012?</p></th>
<th><p>Interval Length</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Smallest Cumulative</p></td>
<td><p>Frequency</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>Host Adjustment Messages Proc &lt;= 7 Days - as receiver</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
<td><p>Month-to-Date</p></td>
</tr>
<tr class="odd">
<td><p>NASCO Par Adjustment Messages Proc &lt;= 7 Days - as
receiver</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
<td><p>Month-to-Date</p></td>
</tr>
<tr class="even">
<td><p>Host Adjustment Messages Proc Total - as receiver</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
<td><p>Month-to-Date</p></td>
</tr>
<tr class="odd">
<td><p>NASCO Par Adjustment Messages Proc Total - as receiver</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
<td><p>Month-to-Date</p></td>
</tr>
</tbody>
</table>
<h5 id="timeline_and_restrictions_3">Timeline and Restrictions</h5>
<p>Only Path A counts towards this input. If the adjustment cancellation
request and response are present (Path B), the input excludes the
corresponding adjustment response from consideration.</p>
<p>This considers request-response pairs whether or not the request is
closed.</p>
<p>Note: The only differences from Home Adjustments Processed &lt;= 14
Days are the perspective, Reason Codes, and age cut-off, as well as no
consideration of a Cross-Reference SF.</p>
<table>
<thead>
<tr class="header">
<th></th>
<th colspan="2"><p>Date Field</p></th>
<th colspan="2"><p>Order</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Path A</p></td>
<td><p>Path B</p></td>
<td><p>Field</p></td>
<td><p>Criteria</p></td>
<td></td>
</tr>
<tr class="even">
<td rowspan="2"><p>Start</p></td>
<td rowspan="2"><p>Date Created</p></td>
<td rowspan="2"><p>(the later of those records present and
considered)</p></td>
<td colspan="2" rowspan="2" style="text-align: center;"><p>1</p></td>
</tr>
<tr class="odd">
</tr>
<tr class="even">
<td colspan="3"></td>
<td></td>
<td style="text-align: center;"><p>2</p></td>
</tr>
<tr class="odd">
<td colspan="3" rowspan="2"></td>
<td rowspan="2"></td>
<td rowspan="2" style="text-align: center;"><p>3</p></td>
</tr>
<tr class="even">
</tr>
<tr class="odd">
<td rowspan="3"><p>Finish</p></td>
<td><p>Date Created</p></td>
<td rowspan="3"><p>(the earlier of those records present and
considered)</p></td>
<td style="text-align: center;"><p>2 (optional)</p></td>
<td></td>
</tr>
<tr class="even">
<td rowspan="2"><p>Date Posted</p></td>
<td rowspan="2" style="text-align: center;"><p>3</p></td>
<td rowspan="2"></td>
</tr>
<tr class="odd">
</tr>
</tbody>
</table>
<p>====Home MED REC Claims Adjudicated &lt;= 30 Days====</p>
<h5 id="field_4">Field</h5>
<table>
<thead>
<tr class="header">
<th><p>Points Possible</p></th>
<th><p>5.00</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Standard Range</p></td>
<td><p>85 - 99%</p></td>
</tr>
<tr class="even">
<td><p>Points Formula</p></td>
<td><p>Linear (higher is better); includes no-volume logic</p></td>
</tr>
<tr class="odd">
<td><p>Score Formula</p></td>
<td><p>Percentage</p></td>
</tr>
<tr class="even">
<td><p>Class</p></td>
<td><p>% Processed</p></td>
</tr>
<tr class="odd">
<td><p>Description</p></td>
<td><p>The percent of claims, for which medical records were requested
and received, that are adjudicated within 30 calendar days of receipt of
the medical records via Blue².</p></td>
</tr>
<tr class="even">
<td><p>Considerations</p></td>
<td><p>Start Time - Home Plan Receipt of Medical Records is determined
by one of the following ways, the latter of:<br />
- The last Medical Record response from the Host Plan closing the parent
Medical Record request; or<br />
- The response from the Host Plan comes in the form of an Info message
with message code 173 or 178<br />
<br />
End Time/Reporting Date - Home Plan Claim Processing Post Date is
determined by one of the following ways:<br />
- The claim is processed by the Original DF Home Plan Post Date, upon
the receipt of medical records; or<br />
- The claim is processed by creating an adjusted DF. The adjustment for
the DF may come in the form of a Streamline Adjustment or an Adjustment
request and both must include the adjustment Reason Code 240<br />
<br />
One claim can have multiple medical record events where each event is
captured as a separate transaction. Exclusions:<br />
- Claims without a Medical Record request (either never present or
cancelled), regardless of the presence of Info message with a Reason
Code of 173 or 178<br />
- Claims adjusted with Reason Code 240, but without a valid Medical
Record request 165 or 171 present on Formats Database/Blue²<br />
- Adjusted DFs using any other Reason Code than 240</p></td>
</tr>
</tbody>
</table>
<h5 id="inputs_4">Inputs</h5>
<table>
<thead>
<tr class="header">
<th><p>Title</p></th>
<th><p>Calculated?</p></th>
<th><p>Shared with Enhanced IPP Scorecard 2012?</p></th>
<th><p>Interval Length</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Smallest Cumulative</p></td>
<td><p>Frequency</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>Home MED REC Claims Adjudicated &lt;= 30 Days</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
<td><p>Month-to-Date</p></td>
</tr>
<tr class="odd">
<td><p>Home MED REC Claims Adjudicated Total</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
<td><p>Month-to-Date</p></td>
</tr>
</tbody>
</table>
<h5 id="timeline_and_restrictions_4">Timeline and Restrictions</h5>
<p>Both Path A and Path B count towards this input; a Plan can respond
to the MEDREC request with either a MEDREC response or an INFO
one-way.</p>
<table>
<thead>
<tr class="header">
<th></th>
<th><p>Date Field</p></th>
<th colspan="2"><p>Order</p></th>
<th><p>Record</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Path A</p></td>
<td><p>Path B</p></td>
<td><p>Field</p></td>
<td><p>Criteria</p></td>
<td></td>
</tr>
<tr class="even">
<td rowspan="2"></td>
<td rowspan="2"></td>
<td colspan="2" rowspan="2" style="text-align: center;"><p>1</p></td>
<td rowspan="2"><p>MEDREC Request</p></td>
</tr>
<tr class="odd">
</tr>
<tr class="even">
<td rowspan="7"><p>Start</p></td>
<td rowspan="7"><p>Date Created (the later from those records present
and considered)</p></td>
<td rowspan="3" style="text-align: center;"><p>2</p></td>
<td rowspan="3"></td>
<td rowspan="3"><p>MEDREC Response</p></td>
</tr>
<tr class="odd">
</tr>
<tr class="even">
</tr>
<tr class="odd">
<td rowspan="4"></td>
<td rowspan="4" style="text-align: center;"><p>2</p></td>
<td rowspan="4"><p>INFO One-way</p></td>
</tr>
<tr class="even">
</tr>
<tr class="odd">
</tr>
<tr class="even">
</tr>
<tr class="odd">
<td rowspan="2"><p>Finish</p></td>
<td rowspan="2"><p>Date Created</p></td>
<td colspan="2" rowspan="2" style="text-align: center;"><p>3</p></td>
<td rowspan="2"><p>DF</p></td>
</tr>
<tr class="even">
</tr>
</tbody>
</table>
<p>====Host Misroutes Processed &lt;= 10 Days====</p>
<h5 id="field_5">Field</h5>
<table>
<thead>
<tr class="header">
<th><p>Points Possible</p></th>
<th><p>5.00</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Standard Range</p></td>
<td><p>90 - 98%</p></td>
</tr>
<tr class="even">
<td><p>Points Formula</p></td>
<td><p>Linear (higher is better); includes no-volume logic</p></td>
</tr>
<tr class="odd">
<td><p>Score Formula</p></td>
<td><p>Percentage</p></td>
</tr>
<tr class="even">
<td><p>Class</p></td>
<td><p>% Processed</p></td>
</tr>
<tr class="odd">
<td><p>Description</p></td>
<td><p>As a Host Plan as a receiver, the total Blue² misroute messages
processed within 10 calendar days of receipt from the partner Home Plan
divided by the total Blue² misroute messages processed.<br />
<br />
Processed is defined as a misroute message response that has been sent
to the partner Home Plan.</p></td>
</tr>
<tr class="even">
<td><p>Considerations</p></td>
<td><p>Start Time - Host Plan Receipt message Create Date of a Misrouted
Claim message from Home Plan in Blue². End Time - Host Plan response
message Create Date to the Home Plan Misrouted Claim message in
Blue².</p></td>
</tr>
</tbody>
</table>
<h5 id="inputs_5">Inputs</h5>
<table>
<thead>
<tr class="header">
<th><p>Title</p></th>
<th><p>Calculated?</p></th>
<th><p>Shared with Enhanced IPP Scorecard 2012?</p></th>
<th><p>Interval Length</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Smallest Cumulative</p></td>
<td><p>Frequency</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>Host Misroutes Processed &lt;= 10 Days</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
<td><p>Month-to-Date</p></td>
</tr>
<tr class="odd">
<td><p>Host Misroutes Processed Total</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
<td><p>Month-to-Date</p></td>
</tr>
</tbody>
</table>
<h5 id="timeline_and_restrictions_5">Timeline and Restrictions</h5>
<table>
<thead>
<tr class="header">
<th></th>
<th><p>Date Field</p></th>
<th><p>Order</p></th>
<th><p>Record</p></th>
<th><p>Restriction</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Field</p></td>
<td><p>Criteria</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>Start</p></td>
<td><p>Date Created</p></td>
<td style="text-align: center;"><p>1</p></td>
<td><p>MISROUTE Request</p></td>
<td><p>Direction</p></td>
</tr>
<tr class="odd">
<td><p>Finish</p></td>
<td><p>Date Created</p></td>
<td style="text-align: center;"><p>2</p></td>
<td><p>MISROUTE Response</p></td>
<td><p>Direction</p></td>
</tr>
</tbody>
</table>
<p>====Home Claim Appeals Resolved &lt;= 20====</p>
<h5 id="field_6">Field</h5>
<table>
<thead>
<tr class="header">
<th><p>Points Possible</p></th>
<th><p>5.00</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Standard Range</p></td>
<td><p>85.0 - 97.0%</p></td>
</tr>
<tr class="even">
<td><p>Points Formula</p></td>
<td><p>Linear (higher is better); includes no-volume logic</p></td>
</tr>
<tr class="odd">
<td><p>Score Formula</p></td>
<td><p>Percentage</p></td>
</tr>
<tr class="even">
<td><p>Class</p></td>
<td><p>% Processed</p></td>
</tr>
<tr class="odd">
<td><p>Description</p></td>
<td><p>As a Home Plan as a receiver, the total Blue² Claim Appeals
processed within 20 calendar days divided by the total Home Blue² Claim
Appeals processed.</p></td>
</tr>
<tr class="even">
<td><p>Considerations</p></td>
<td><p>The reporting date is based off the Home Plan Claim Appeals
response message Create Date. Only request messages that are 'Closed'
messages (Open/Closed? = 2) are included. Claim Appeals in a processed
state are included. Claim Appeals in a retry state are excluded. The
Host Plan is the initiator and sends a request to the Home Plan. The
Start Time (trigger) is the Home Plan Receipt Date of the Claim Appeal
request. The Stop Time is the Home Plan Response Date to the open
request.</p></td>
</tr>
</tbody>
</table>
<h5 id="inputs_6">Inputs</h5>
<table>
<thead>
<tr class="header">
<th><p>Title</p></th>
<th><p>Calculated?</p></th>
<th><p>Shared with Enhanced IPP Scorecard 2012?</p></th>
<th><p>Interval Length</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Smallest Cumulative</p></td>
<td><p>Frequency</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>Home Claim Appeals Resolved &lt;= 20 Days</p></td>
<td><p>Yes</p></td>
<td><p>No</p></td>
<td><p>Month-to-Date</p></td>
</tr>
<tr class="odd">
<td><p>Home Claim Appeals Resolved Total</p></td>
<td><p>Yes</p></td>
<td><p>No</p></td>
<td><p>Month-to-Date</p></td>
</tr>
</tbody>
</table>
<h5 id="timeline_and_restrictions_6">Timeline and Restrictions</h5>
<table>
<thead>
<tr class="header">
<th></th>
<th><p>Date Field</p></th>
<th><p>Order</p></th>
<th><p>Record</p></th>
<th><p>Restriction</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Field</p></td>
<td><p>Criteria</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>Start</p></td>
<td><p>Date Created</p></td>
<td style="text-align: center;"><p>1</p></td>
<td><p>CLMAPPL Request</p></td>
<td><p>Direction</p></td>
</tr>
<tr class="odd">
<td><p>Finish</p></td>
<td><p>Date Created</p></td>
<td style="text-align: center;"><p>2</p></td>
<td><p>CLMAPPL Response</p></td>
<td><p>Direction</p></td>
</tr>
</tbody>
</table>
<h3 id="plan_to_plan_inquiry">Plan-to-Plan Inquiry</h3>
<p>====Home GEN INQ Finalized &lt;= 10 Days====</p>
<h5 id="field_7">Field</h5>
<table>
<thead>
<tr class="header">
<th><p>Points Possible</p></th>
<th><p>5.50</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Standard Range</p></td>
<td><p>90.0 - 97.0%</p></td>
</tr>
<tr class="even">
<td><p>Points Formula</p></td>
<td><p>Linear (higher is better); includes no-volume logic</p></td>
</tr>
<tr class="odd">
<td><p>Score Formula</p></td>
<td><p>Percentage</p></td>
</tr>
<tr class="even">
<td><p>Class</p></td>
<td><p>% Processed</p></td>
</tr>
<tr class="odd">
<td><p>Description</p></td>
<td><p>As a Home Plan as a receiver, the total Blue² General Inquiries
processed within 10 calendar days divided by the total Home Blue²
General Inquiries processed.</p></td>
</tr>
<tr class="even">
<td><p>Considerations</p></td>
<td><p>The reporting date is based off the Home Plan General Inquiry
response message Create Date. Only request messages that are 'Closed'
messages (Open/Closed? = 2) are included. General Inquiries in a
processed state are included. General Inquiries in a retry state are
excluded. General Inquiries with Reason Code 168 are excluded. The Host
Plan is the initiator and sends a request to the Home Plan. The Start
Time (trigger) is the Home Plan Receipt Date of the General Inquiry
request. The Stop Time is the Home Plan Response Date to the open
request.</p></td>
</tr>
</tbody>
</table>
<h5 id="inputs_7">Inputs</h5>
<table>
<thead>
<tr class="header">
<th><p>Title</p></th>
<th><p>Calculated?</p></th>
<th><p>Shared with Enhanced IPP Scorecard 2012?</p></th>
<th><p>Interval Length</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Smallest Cumulative</p></td>
<td><p>Frequency</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>Home General Inquiries Proc &lt;= 10 Days</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
<td><p>Month-to-Date</p></td>
</tr>
<tr class="odd">
<td><p>Home General Inquiries Proc Total</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
<td><p>Month-to-Date</p></td>
</tr>
</tbody>
</table>
<h5 id="timeline_and_restrictions_7">Timeline and Restrictions</h5>
<table>
<thead>
<tr class="header">
<th></th>
<th><p>Date Field</p></th>
<th><p>Order</p></th>
<th><p>Record</p></th>
<th><p>Restriction</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Field</p></td>
<td><p>Criteria</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td rowspan="2"><p>Start</p></td>
<td rowspan="2"><p>Date Created</p></td>
<td rowspan="2" style="text-align: center;"><p>1</p></td>
<td rowspan="2"><p>GENINQ Request</p></td>
<td><p>Direction</p></td>
</tr>
<tr class="odd">
<td><p>Reason</p></td>
</tr>
<tr class="even">
<td rowspan="2"><p>Finish</p></td>
<td rowspan="2"><p>Date Created</p></td>
<td rowspan="2" style="text-align: center;"><p>2</p></td>
<td rowspan="2"><p>GENINQ Response</p></td>
<td><p>Direction</p></td>
</tr>
<tr class="odd">
<td><p>Reason</p></td>
</tr>
</tbody>
</table>
<p>====Host GEN INQ Finalized &lt;= 10 Days====</p>
<h5 id="field_8">Field</h5>
<table>
<thead>
<tr class="header">
<th><p>Points Possible</p></th>
<th><p>5.50</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Standard Range</p></td>
<td><p>90.0 - 97.0%</p></td>
</tr>
<tr class="even">
<td><p>Points Formula</p></td>
<td><p>Linear (higher is better); includes no-volume logic</p></td>
</tr>
<tr class="odd">
<td><p>Score Formula</p></td>
<td><p>Percentage</p></td>
</tr>
<tr class="even">
<td><p>Class</p></td>
<td><p>% Processed</p></td>
</tr>
<tr class="odd">
<td><p>Description</p></td>
<td><p>As a Host Plan as a receiver, the total Blue² General Inquiries
processed within 10 calendar days divided by the total Host Blue²
General Inquiries processed.</p></td>
</tr>
<tr class="even">
<td><p>Considerations</p></td>
<td><p>The reporting date is based off the Host Plan General Inquiry
response message Create Date. Only request messages that are 'Closed'
messages (Open/Closed? = 2) are included. General Inquiries in a
processed state are included. General Inquiries in a retry state are
excluded. General Inquiries with Reason Code 168 are excluded. The Home
Plan is the initiator and sends a request to the Host Plan. The Start
Time (trigger) is the Host Plan Receipt Date of the General Inquiry
request. The Stop Time is the Host Plan Response Date to the open
request.</p></td>
</tr>
</tbody>
</table>
<h5 id="inputs_8">Inputs</h5>
<table>
<thead>
<tr class="header">
<th><p>Title</p></th>
<th><p>Calculated?</p></th>
<th><p>Shared with Enhanced IPP Scorecard 2012?</p></th>
<th><p>Interval Length</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Smallest Cumulative</p></td>
<td><p>Frequency</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>Host General Inquiries Proc &lt;= 10 Days</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
<td><p>Month-to-Date</p></td>
</tr>
<tr class="odd">
<td><p>Host General Inquiries Proc Total</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
<td><p>Month-to-Date</p></td>
</tr>
</tbody>
</table>
<h5 id="timeline_and_restrictions_8">Timeline and Restrictions</h5>
<table>
<thead>
<tr class="header">
<th></th>
<th><p>Date Field</p></th>
<th><p>Order</p></th>
<th><p>Record</p></th>
<th><p>Restriction</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Field</p></td>
<td><p>Criteria</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>Start</p></td>
<td><p>Date Created</p></td>
<td style="text-align: center;"><p>1</p></td>
<td><p>GENINQ Request</p></td>
<td><p>Direction</p></td>
</tr>
<tr class="odd">
<td><p>Finish</p></td>
<td><p>Date Created</p></td>
<td style="text-align: center;"><p>2</p></td>
<td><p>GENINQ Response</p></td>
<td><p>Direction</p></td>
</tr>
</tbody>
</table>
<h4 id="average_speed_of_answer">Average Speed of Answer</h4>
<h5 id="field_9">Field</h5>
<table>
<thead>
<tr class="header">
<th><p>Points Possible</p></th>
<th><p>5.0</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Standard Range</p></td>
<td><p>45 - 90 sec.</p></td>
</tr>
<tr class="even">
<td><p>Points Formula</p></td>
<td><p>Linear (lower is better)</p></td>
</tr>
<tr class="odd">
<td><p>Score Formula</p></td>
<td><p>Adjusted average</p></td>
</tr>
<tr class="even">
<td><p>Class</p></td>
<td><p>N/A</p></td>
</tr>
<tr class="odd">
<td><p>Description</p></td>
<td><p>The average time (in seconds) that a caller from another Plan
must wait to connect to an agent. The clock starts when the caller
enters the system queue after answering the last automatic telephone
prompt.</p></td>
</tr>
<tr class="even">
<td><p>Considerations</p></td>
<td><p>N/A</p></td>
</tr>
</tbody>
</table>
<h5 id="inputs_9">Inputs</h5>
<table>
<thead>
<tr class="header">
<th><p>Title</p></th>
<th><p>Calculated?</p></th>
<th><p>Shared with Enhanced IPP Scorecard 2012?</p></th>
<th><p>Interval Length</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Smallest Cumulative</p></td>
<td><p>Frequency</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>Average Speed of Answer (seconds)</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
<td><p>Month-to-Date</p></td>
</tr>
<tr class="odd">
<td><p>Average Speed of Answer Volume</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
<td><p>Month-to-Date</p></td>
</tr>
</tbody>
</table>
<p>====Inquiries Resolved &lt;= 14 Days====</p>
<h5 id="field_10">Field</h5>
<table>
<thead>
<tr class="header">
<th><p>Points Possible</p></th>
<th><p>9.0</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Standard Range</p></td>
<td><p>90 - 98%</p></td>
</tr>
<tr class="even">
<td><p>Points Formula</p></td>
<td><p>Linear (higher is better)</p></td>
</tr>
<tr class="odd">
<td><p>Score Formula</p></td>
<td><p>Percentage</p></td>
</tr>
<tr class="even">
<td><p>Class</p></td>
<td><p>% Processed</p></td>
</tr>
<tr class="odd">
<td><p>Description</p></td>
<td><p>The percent of total Plan-to-Plan inquiries resolved in 14
calendar days or less.</p></td>
</tr>
<tr class="even">
<td><p>Considerations</p></td>
<td><p>Percentage is determined by taking the total number of inquiries
resolved within 14 calendar days and dividing it by the total number of
inquiries resolved within the reporting period. The measure includes
calls resolved up to and including day 14.</p></td>
</tr>
</tbody>
</table>
<h5 id="inputs_10">Inputs</h5>
<table>
<thead>
<tr class="header">
<th><p>Title</p></th>
<th><p>Calculated?</p></th>
<th><p>Shared with Enhanced IPP Scorecard 2012?</p></th>
<th><p>Interval Length</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Smallest Cumulative</p></td>
<td><p>Frequency</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>Home/Control Inquiries Resolved &lt;= 14 Days</p></td>
<td><p>No</p></td>
<td><p>No</p></td>
<td><p>Month-to-Date</p></td>
</tr>
<tr class="odd">
<td><p>Host/Par Inquiries Resolved &lt;= 14 Days</p></td>
<td><p>No</p></td>
<td><p>No</p></td>
<td><p>Month-to-Date</p></td>
</tr>
<tr class="even">
<td><p>Home/Control Inquiries Resolved Total</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
<td><p>Month-to-Date</p></td>
</tr>
<tr class="odd">
<td><p>Host/Par Inquiries Resolved Total</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
<td><p>Month-to-Date</p></td>
</tr>
</tbody>
</table>
<h3 id="end_to_end_claim_experience">End-to-End Claim Experience</h3>
<p>====Claims/Adjustments Finalized &lt;= 30 Days====</p>
<h5 id="field_11">Field</h5>
<table>
<thead>
<tr class="header">
<th><p>Points Possible</p></th>
<th><p>12.50</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Standard Range</p></td>
<td><p>90 - 96%</p></td>
</tr>
<tr class="even">
<td><p>Points Formula</p></td>
<td><p>Linear (higher is better)</p></td>
</tr>
<tr class="odd">
<td><p>Score Formula</p></td>
<td><p>Percentage</p></td>
</tr>
<tr class="even">
<td><p>Class</p></td>
<td><p>% Processed</p></td>
</tr>
<tr class="odd">
<td><p>Description</p></td>
<td><p>The percent of all End-to-End Inter-Plan claims finalized in 30
calendar days including all adjustments to the claim based on MTM
consistent definition of First Day of Processing (FDP) and Last Day of
Processing (LDP).</p></td>
</tr>
<tr class="even">
<td><p>Considerations</p></td>
<td><p>Percentage is determined by taking the total number of claims and
adjustments processed within 30 days and dividing it by the total number
of claims and adjustments processed within the reporting period. The
measure includes claims and adjustments processed up to and including
day 30.</p></td>
</tr>
</tbody>
</table>
<h5 id="inputs_11">Inputs</h5>
<table>
<thead>
<tr class="header">
<th><p>Title</p></th>
<th><p>Calculated?</p></th>
<th><p>Shared with Enhanced IPP Scorecard 2012?</p></th>
<th><p>Interval Length</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Smallest Cumulative</p></td>
<td><p>Frequency</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>ITS Claims/Adjustments Proc &lt;= 30 Days (QtD)</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
<td><p>Quarter-to-Date</p></td>
</tr>
<tr class="odd">
<td><p>NASCO Control Original Claims Proc &lt;= 30 Days (QtD)</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
<td><p>Quarter-to-Date</p></td>
</tr>
<tr class="even">
<td><p>NASCO Par Original Claims Proc &lt;= 30 Days (QtD)</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
<td><p>Quarter-to-Date</p></td>
</tr>
<tr class="odd">
<td><p>ITS Claims/Adjustments Proc Total (QtD)</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
<td><p>Quarter-to-Date</p></td>
</tr>
<tr class="even">
<td><p>NASCO Control Original Claims Proc Total (QtD)</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
<td><p>Quarter-to-Date</p></td>
</tr>
<tr class="odd">
<td><p>NASCO Par Original Claims Proc Total (QtD)</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
<td><p>Quarter-to-Date</p></td>
</tr>
</tbody>
</table>
<p>====Claims/Adjustments Finalized &lt;= 60 Days====</p>
<h5 id="field_12">Field</h5>
<table>
<thead>
<tr class="header">
<th><p>Points Possible</p></th>
<th><p>12.50</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Standard Range</p></td>
<td><p>96.0 - 98.5%</p></td>
</tr>
<tr class="even">
<td><p>Points Formula</p></td>
<td><p>Linear (higher is better)</p></td>
</tr>
<tr class="odd">
<td><p>Score Formula</p></td>
<td><p>Percentage</p></td>
</tr>
<tr class="even">
<td><p>Class</p></td>
<td><p>% Processed</p></td>
</tr>
<tr class="odd">
<td><p>Description</p></td>
<td><p>The percent of all End-to-End Inter-Plan claims finalized in 60
calendar days including all adjustments to the claim based on MTM
consistent definition of First Day of Processing (FDP) and Last Day of
Processing (LDP).</p></td>
</tr>
<tr class="even">
<td><p>Considerations</p></td>
<td><p>Percentage is determined by taking the total number of claims and
adjustments processed within 60 days and dividing it by the total number
of claims and adjustments processed within the reporting period. The
measure includes claims and adjustments processed up to and including
day 60.</p></td>
</tr>
</tbody>
</table>
<h5 id="inputs_12">Inputs</h5>
<table>
<thead>
<tr class="header">
<th><p>Title</p></th>
<th><p>Calculated?</p></th>
<th><p>Shared with Enhanced IPP Scorecard 2012?</p></th>
<th><p>Interval Length</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Smallest Cumulative</p></td>
<td><p>Frequency</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>ITS Claims/Adjustments Proc &lt;= 60 Days (QtD)</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
<td><p>Quarter-to-Date</p></td>
</tr>
<tr class="odd">
<td><p>NASCO Control Original Claims Proc &lt;= 60 Days (QtD)</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
<td><p>Quarter-to-Date</p></td>
</tr>
<tr class="even">
<td><p>NASCO Par Original Claims Proc &lt;= 60 Days (QtD)</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
<td><p>Quarter-to-Date</p></td>
</tr>
<tr class="odd">
<td><p>ITS Claims/Adjustments Proc Total (QtD)</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
<td><p>Quarter-to-Date</p></td>
</tr>
<tr class="even">
<td><p>NASCO Control Original Claims Proc Total (QtD)</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
<td><p>Quarter-to-Date</p></td>
</tr>
<tr class="odd">
<td><p>NASCO Par Original Claims Proc Total (QtD)</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
<td><p>Quarter-to-Date</p></td>
</tr>
</tbody>
</table>
<h3 id="licensee_desk_level_audit">Licensee Desk-Level Audit</h3>
<h4 id="financial_dollar_accuracy">Financial Dollar Accuracy</h4>
<h5 id="field_13">Field</h5>
<table>
<thead>
<tr class="header">
<th><p>Points Possible</p></th>
<th><p>10.00</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Standard Range</p></td>
<td><p>97 - 99%</p></td>
</tr>
<tr class="even">
<td><p>Points Formula</p></td>
<td><p>Linear (higher is better)</p></td>
</tr>
<tr class="odd">
<td><p>Score Formula</p></td>
<td><p>Adjusted average</p></td>
</tr>
<tr class="even">
<td><p>Class</p></td>
<td><p>Percent</p></td>
</tr>
<tr class="odd">
<td><p>Description</p></td>
<td><p>The percent of discount dollars passed correctly on claims
audited.</p></td>
</tr>
<tr class="even">
<td><p>Considerations</p></td>
<td><p>LDLA financial dollar accuracy is calculated based on audit
results submitted by each Plan. Margin of error has been removed to more
accurately reflect a Plan's actual performance.</p></td>
</tr>
</tbody>
</table>
<h5 id="inputs_13">Inputs</h5>
<table>
<thead>
<tr class="header">
<th><p>Title</p></th>
<th><p>Calculated?</p></th>
<th><p>Shared with Enhanced IPP Scorecard 2012?</p></th>
<th><p>Interval Length</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Smallest Cumulative</p></td>
<td><p>Frequency</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>LDLA Financial Dollar Accuracy (quarterly)</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
<td><p>Month-to-Date</p></td>
</tr>
<tr class="odd">
<td><p>LDLA Financial Dollar Accuracy Volume (quarterly)</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
<td><p>Month-to-Date</p></td>
</tr>
</tbody>
</table>
<h4 id="data_accuracy">Data Accuracy</h4>
<h5 id="field_14">Field</h5>
<table>
<thead>
<tr class="header">
<th><p>Points Possible</p></th>
<th><p>5.00</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Standard Range</p></td>
<td><p>90 - 95%</p></td>
</tr>
<tr class="even">
<td><p>Points Formula</p></td>
<td><p>Linear (higher is better)</p></td>
</tr>
<tr class="odd">
<td><p>Score Formula</p></td>
<td><p>Adjusted average</p></td>
</tr>
<tr class="even">
<td><p>Class</p></td>
<td><p>Percent</p></td>
</tr>
<tr class="odd">
<td><p>Description</p></td>
<td><p>The percent of claims audited correctly.</p></td>
</tr>
<tr class="even">
<td><p>Considerations</p></td>
<td><p>LDLA data accuracy is calculated based on audit results submitted
by each Plan.</p></td>
</tr>
</tbody>
</table>
<h5 id="inputs_14">Inputs</h5>
<table>
<thead>
<tr class="header">
<th><p>Title</p></th>
<th><p>Calculated?</p></th>
<th><p>Shared with Enhanced IPP Scorecard 2012?</p></th>
<th><p>Interval Length</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Smallest Cumulative</p></td>
<td><p>Frequency</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>LDLA Data Accuracy (quarterly)</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
<td><p>Month-to-Date</p></td>
</tr>
<tr class="odd">
<td><p>LDLA Data Accuracy Volume (quarterly)</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
<td><p>Month-to-Date</p></td>
</tr>
</tbody>
</table>
<h4 id="home_financial_dollar_accuracy">Home Financial Dollar
Accuracy</h4>
<h5 id="field_15">Field</h5>
<table>
<thead>
<tr class="header">
<th><p>Points Possible</p></th>
<th><p>0.00 (informational)</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Standard Range</p></td>
<td><p>TBD</p></td>
</tr>
<tr class="even">
<td><p>Points Formula</p></td>
<td><p>Linear (higher is better)</p></td>
</tr>
<tr class="odd">
<td><p>Score Formula</p></td>
<td><p>Adjusted average</p></td>
</tr>
<tr class="even">
<td><p>Class</p></td>
<td><p>Percent</p></td>
</tr>
<tr class="odd">
<td><p>Description</p></td>
<td><p>The percent of discount dollars passed correctly on Home claims
audited.</p></td>
</tr>
<tr class="even">
<td><p>Considerations</p></td>
<td><p>LDLA Home financial dollar accuracy is calculated based on audit
results submitted by each Plan. Margin of error has been removed to more
accurately reflect a Plan's actual performance.</p></td>
</tr>
</tbody>
</table>
<h5 id="inputs_15">Inputs</h5>
<table>
<thead>
<tr class="header">
<th><p>Title</p></th>
<th><p>Calculated?</p></th>
<th><p>Shared with Enhanced IPP Scorecard 2012?</p></th>
<th><p>Interval Length</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Smallest Cumulative</p></td>
<td><p>Frequency</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>LDLA Home Financial Dollar Accuracy (quarterly)</p></td>
<td><p>No</p></td>
<td><p>No</p></td>
<td><p>Month-to-Date</p></td>
</tr>
<tr class="odd">
<td><p>LDLA Home Financial Dollar Accuracy Volume (quarterly)</p></td>
<td><p>No</p></td>
<td><p>No</p></td>
<td><p>Month-to-Date</p></td>
</tr>
</tbody>
</table>