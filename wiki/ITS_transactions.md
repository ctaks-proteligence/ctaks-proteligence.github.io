# ITS transactions

ITS transactions (along with Blue<sup>2</sup> messages) move claim
processing forward. There are currently three types, known as formats.

## Key

All ITS transactions are identified by the SCCF ID, which itself is
comprised of the Base and Suffix. The SCCF Base uniquely identifies a
claim experience at the Host or Home Plan. However, two instances of
each ITS transaction exist, mirroring each other: one at the Host Plan,
the other at the Home Plan. Both instances have the same SCCF ID. Since
Datanet allows loading both instances, BOID is used in addition to SCCF
Base to make the claim experience globally unique.

In addition, DFs and RFs have Non-Void and Void forms, distinguished by
Disposition Code.

## Formats

### SF: Submission Format

The SF is created when the claim is acknowledged by the Plan as
submitted by the provider. This is the starting point of claim
processing. SFs are always the start of a claim experience and always
have SCCF Suffix == 00.

### DF: Disposition Format

The DF denotes the disposition of the claim -- how much of the claim the
Plan will pay. The original DF (SCCF Suffix == 00, Disposition Code
indicating Non-Void) indicates the original amount to pay. Plans choose
to adjust a small percentage of claims after this, at which point they
create a Void DF to indicate the amount specified will be revised.

The Plan considers a claim experience to be worked (complete) once the
last transaction is a Non-Void DF.

### RF: Reconciliation Format

Each DF is usually paired with an RF. (The CFA Code indicates whether
creation of a DF results in an RF.) The RF is focused on the actual
pay-out of the claim -- while other formats are sent and received
between two Plans, the RF is exchanged between the Plan and the Central
Financial Agency (CFA).

### Obsolete Formats

An additional format, NF (Notification Format), is obsolete as of IPP
Release 14.5 -- Blue<sup>2</sup> adjustment messages replace them. The
current releases of Datanet and Datanet Lite have vestiges of support
for NFs, but no further work will occur for them.

## Order of Transactions

<table>
  <tr>
    <th>SCCF Suffix</th>
    <th>Format</th>
    <th>Disposition Code</th>
    <th>Exists Only If</th>
  </tr>
  <tr>
    <td>00</td>
    <td>SF</td>
  </tr>
  <tr>
    <td>00</td>
    <td>DF</td>
    <td>Non-Void</td>
  </tr>
  <tr>
    <td>00</td>
    <td>RF</td>
    <td>Non-Void</td>
    <td>CFA Code != 0</td>
  </tr>
  <tr>
    <td>00</td>
    <td>DF</td>
    <td>Void</td>
  </tr>
  <tr>
    <td>00</td>
    <td>RF</td>
    <td>Void</td>
    <td>CFA Code != 0</td>
  </tr>
  <tr>
    <td>01</td>
    <td>DF</td>
    <td>Non-Void</td>
  </tr>
  <tr>
    <td>01</td>
    <td>RF</td>
    <td>Non-Void</td>
    <td>CFA Code != 0</td>
  </tr>
  <tr>
    <td>01</td>
    <td>DF</td>
    <td>Void</td>
  </tr>
  <tr>
    <td>01</td>
    <td>RF</td>
    <td>Void</td>
    <td>CFA Code != 0</td>
  </tr>
  <tr>
    <td colspan="4">  ...</td>
  </tr>
  <tr>
    <td>99</td>
    <td>DF</td>
    <td>Non-Void</td>
  </tr>
  <tr>
    <td>99</td>
    <td>RF</td>
    <td>Non-Void</td>
    <td>CFA Code != 0</td>
  </tr>
</table>
<br  />
## Closing Transactions

Datanet uses three approaches to close a transaction, in the following
order:

1.  Date Closed from IPOR flat file
2.  Self-closing scenarios
3.  Format-specific scenarios

If none of these three approaches result in Date Closed being set, Date
Closed is left unpopulated.

Open/Closed? and Date Closed must remain consistent -- when Date Closed
is not populated, Open/Closed? must indicate Open; when Date Closed is
populated, Open/Closed? must indicate Closed.

While Date Closed is a field on the IPOR flat files, transactions
created or last updated prior to IPP Release 15.0 do not populate Date
Closed. Further, transactions created as open do not come through on a
later SF or DF file when they close. The OC file covers the latter case
but not the former (therefore, Datanet does not support this file).
Because of this, Datanet uses Date Closed from the file if populated,
and calculates Date Closed otherwise.

The below calculations refer to Date Started; since each transaction has
a mirror transaction at the other Plan, sent transactions use Date
Created as the Date Started, while received transactions use Date Posted
as the Date Started.

### Self-Closing Scenarios

A transaction closes itself when any of the following scenarios apply
using its own fields, setting Date Closed to its own Date Started:

<table>
  <tr>
    <th colspan="3"></th>
    <th>Applies To</th>
    <th rowspan="2"></th>
    <th>Scenario</th>
    <th rowspan="2"></th>
    <th>Notes</th>
  </tr>
  <tr>
    <th>SF</th>
    <th>DF</th>
    <th>RF</th>
  </tr>
  <tr>
    <td>Yes</td>
    <td>Yes</td>
    <td></td>
    <td>Status != 'V'</td>
    <td>Non-Valid transactions close immediately.</td>
  </tr>
  <tr>
    <td>Yes</td>
    <td>Yes</td>
    <td></td>
    <td>Estimate? == 'E' or Estimate? == 'A' (as of 21.5)</td>
    <td>Estimate transactions close immediately. For A as of 21.5, no DF/RF should be created.</td>
  </tr>
  <tr>
    <td>Yes</td>
    <td>Yes</td>
    <td></td>
    <td><b>Out of scope:</b> Program == '6'&lt;br&gt;NOTE: As of the 20.0 ETL update 3.11.1, there will be no PGM_CD=6 considerations</td>
    <td>Custom transactions are Plan-configurable in BCBSA software to either close immediately or remain open. Therefore, Datanet does not implement this logic but simply relies on the file to populate Date Closed if the Plan configured them to close immediately.&lt;br&gt;NOTE: As of the 20.0 ETL update 3.11.1, there will be no PGM_CD=6 considerations</td>
  </tr>
  <tr>
    <td></td>
    <td>Yes</td>
    <td></td>
    <td>CFA == '0'</td>
    <td>An RF will not be created (so the DF must close itself).</td>
  </tr>
  <tr>
    <td></td>
    <td>Yes</td>
    <td></td>
    <td>Claim-level Message Codes list includes '1062'</td>
    <td>Closes out a cross-reference SF and rejects the entire claim. An RF will not be created (so the DF must close itself).</td>
  </tr>
  <tr>
    <td></td>
    <td></td>
    <td>Yes</td>
    <td>(Always)</td>
  </tr>
</table>
<br  />
### Format-Specific Scenarios

If none of the self-closing scenarios apply, evaluate the scenarios
below for the format whose Date Closed is being determined in the order
they appear. The first to match sets Date Closed as indicated.

The following apply in all scenarios below:

- Unless otherwise specified, only transactions and messages within the
  same claim experience are compared (i.e. BOID and SCCF Base match
  across all considered) and, furthermore, the SCCF Suffix must match.
- Only valid transactions (Status == 'V') may close another transaction.
  (Messages do not have Status and are always considered valid.)
- Only non-estimate transactions (Estimate? != 'E') may close another
  transaction. (Messages do not have Estimate? and are always considered
  non-estimate.)
- Only messages with latest Message Status == 'FNAL' (Final) or ==
  'PRSD' (Processed) may close a transaction.

#### SF

If the SF has a Cross-Reference SCCF ID and its Submission Type == '3'
(adjustment SF) or == '4' (adjustment resubmission):

1.  **Adjustment Cancellation Approval** (external to claim experience):
    Adjustment canceled
    (**Out of scope:** The current version of Datanet does not implement
    this scenario due to not having support for Cross-Reference SCCF
    lookup.)

    Does a message matching the following exist? If so, Date Closed :=
    the date portion of that message's earliest PRSD state timestamp.

    - BOID == SF's BOID
    - Cross-Reference SCCF ID == SF's SCCF ID
    - Message Type == 'CNCLADJ' (Adjustment Cancellation)
    - Request/Response? == 'R' (Response)
    - Action == '136'
2.  **Void DF**: Adjustment approved
    Does a DF with Disposition == '4' (Void) exist within the claim
    experience? If so, Date Closed := that DF's Date Started.
3.  **Adjustment Response Denial** (external to claim experience):
    Adjustment denied
    (**Out of scope:** The current version of Datanet does not implement
    this scenario due to not having support for Cross-Reference SCCF
    lookup.)

    Does a message matching the following exist? If so, Date Closed :=
    the date portion of that message's earliest PRSD state timestamp.

    - BOID == SF's BOID
    - Cross-Reference SCCF ID == SF's SCCF ID
    - Message Type == 'ADJUST' (Adjustment)
    - Request/Response? == 'R' (Response)
    - Action \>= '300'

In all cases:

</li>
<li>

**Non-Void DF**

Does a DF with Disposition == '1' (Non-Void) exist? If so, Date Closed
:= that DF's Date Started.

</li>
<li>

**ECRP**

Is ECRP? == 'Y' and an RF with Disposition == '1' (Non-Void) exists? If
so, Date Closed := that RF's Date Started.

(**Out of scope:** The current version of Datanet does not implement
this scenario due to RFs being filtered out of the claim experience if
the corresponding DF is not present.)

</li>
</ol>

#### DF

Does an RF with Disposition == DF's Disposition exist? If so, Date
Closed := that RF's Date Started.

#### RF

An RF always closes itself immediately; therefore, the RF Date Started
is implicitly the Date Closed. No separate Date Closed exists.

## Not Yet Explained

SFs can be either Original or Adjustment. Adjustment SFs entail a
Cross-Reference chain, which results in an end-to-end experience.

## As Implemented in 21.5

Here is a summary of how CLOSE_DT should be defined (note that when
CLOSE_DT is defined, OPEN_CLOSE_IND=2; otherwise, it is 1) for ITS data
as of 21.5:

- Only STS_CD=V and EST_IND\<\>E records can affect another record.
- For SFs with type 1 or 2 or with type 3 or 4 but no XREF_SCCF_ID
  defined, the close date should be set in the following order:
  - PGM_CD=6 - use CLOSE_DT in the file or leave open if not set in the
    file
    NOTE: As of the 20.0 ETL update 3.11.1, there will be no PGM_CD=6
    considerations
  - Self-closing scenarios (Status != 'V' or Estimate? == 'E' or
    Estimate? == 'A') - use CLOSE_DT in the file if set; otherwise, use
    FMT_DB_POST_DT
    NOTE: Estimate? == 'A' self-closing was added in 21.5
  - DF with suffix 00 and DISP_CD=1 exists - use FMT_DB_POST_DT of DF
- For SFs with type 3 or 4 and the XREF_SCCF_ID is populated, the close
  date should be set in the following order:
  - PGM_CD=6 - use CLOSE_DT in the file or leave open if not set in the
    file
    NOTE: As of the 20.0 ETL update 3.11.1, there will be no PGM_CD=6
    considerations
  - Self-closing scenarios (Status != 'V' or Estimate? == 'E') - use
    CLOSE_DT in the file if set; otherwise, use FMT_DB_POST_DT
  - DF with suffix 00 and DISP_CD=4 exists - use FMT_DB_POST_DT of DF
  - DF with suffix 00 and DISP_CD=1 exists - use FMT_DB_POST_DT of DF
  - Self-close - use CLOSE_DT in the file if set; otherwise, use
    FMT_DB_POST_DT
- For DFs, the close date should be set in the following order:
  - PGM_CD=6 - use CLOSE_DT in the file or leave open if not set in the
    file
    NOTE: As of the 20.0 ETL update 3.11.1, there will be no PGM_CD=6
    considerations
  - Self-closing scenarios (Status != 'V' or Estimate? == 'E') - use
    CLOSE_DT in the file if set; otherwise, use FMT_DB_POST_DT
  - CFA == '0' - a RF will not be created - self-close - use CLOSE_DT in
    the file if set; otherwise, use FMT_DB_POST_DT
  - Claim-level Message Codes list includes '1062'- a RF will not be
    created - self-close - use CLOSE_DT in the file if set; otherwise,
    use FMT_DB_POST_DT
  - RF with the same suffix and same DISP_CD exists - use FMT_DB_POST_DT
    of RF
- For RFs, always self-close based on the FMT_DB_POST_DT.
