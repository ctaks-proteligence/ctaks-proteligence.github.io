## Overview

This page provides the basic requirements for each calculated Scorecard
input for the listed measure.

## Common References

### Date References

Each Scorecard input is calculated for a report date based on a date
range between MM/01/YYYY and MM/DD/YYYY, inclusive, as follows:

- MM/DD/YYYY is the specific report date for which the Scorecard is
  being calculated and
- MM/01/YYYY is the first day of the report date's month.

### Approved Cancellation References

An adjustment can be cancelled by a corresponding approved cancellation
request as follows:

- for the ADJUST message, there exists a matching message as follows:

<!-- -->

    t3.ROOT_MSG_ID = <ROOT_MSG_ID of the ADJUST message> and
    t3.MSG_TP_CD = 'CNCLADJ' and
    t3.PGM_CD <> '6' and
    t3.REQ_RESP_IND = 'Q' and
    t3.MSG_STS_CD in ('PRSD','FNAL')

- that has a matching message as follows:

<!-- -->

    t4.ROOT_MSG_ID = t3.MSG_ID and
    t4.MSG_TP_CD = 'CNCLADJ' and
    t4.PGM_CD <> '6' and
    t4.REQ_RESP_IND = 'R' and
    t4.MSG_STS_CD in ('PRSD','FNAL') and
    t4.ACT_CD = '136' and
    t4.CRT_DT <= 'MM/DD/YYYY'

## Measures

### Home DFs to Process \> 30 Days

#### Inputs

##### Home DFs to Process Total

Count of SFs as follows:

    sf.PGM_CD <> '6' and
    sf.EST_IND <> 'E' and
    sf.STS_CD = 'V' and
    sf.HOST_HOME_CD = '2' and
    sf.FMT_DB_POST_DT <= 'MM/DD/YYYY'

where there is no matching DF as follows:

    df.SCCF_BASE_ID = sf.SCCF_BASE_ID and
    df.SCCF_SUFX_ID = '00' and
    df.HOST_HOME_CD = '2' and
    df.PGM_CD <> '6' and
    df.EST_IND <> 'E' and
    df.DISP_CD <> 4 and
    df.STS_CD = 'V' and
    df.FMT_DB_POST_DT <= 'MM/DD/YYYY'

NOTE: To be completely accurate, also account for ways a SF can be
closed without a DF (out of scope for now).

##### Home DFs to Process \> 30 Days

Count of above where:

    MM/DD/YYYY - sf.FMT_DB_POST_DT > 30

#### Calculation

\[Home DFs to Process \> 30 Days\] / \[Home DFs to Process Total\]

#### IDs

- Home DFs to Process Total = ?
- Home DFs to Process \> 30 Days = ?

===Home Adjustments to Process \<= 14 Days===

#### Inputs

##### Home Adjustment Messages to Process Total

Count of messages as follows:

    t1.MSG_TP_CD = 'ADJUST' and
    t1.HOST_HOME_CD = 2 and
    t1.SEND_RCVE_CD = 'R' and
    t1.PGM_CD <> '6' and
    t1.MSG_STS_CD in ('PRSD','FNAL') and
    t1.REQ_RESP_IND = 'Q' and
    t1.RSN_CD not in ('209','220','222','273','274','275','276','201','242','208','246') and
    t1.CRT_DT <= 'MM/DD/YYYY' and
    No approved cancellation exists as of the report date

where there is no matching message or Void DF as follows:

- Message:

<!-- -->

    t2_.ROOT_MSG_ID = t1.ROOT_MSG_ID and
    t2_.PGM_CD <> '6' and
    t2_.MSG_STS_CD in ('PRSD','FNAL') and
    t2_.HOST_HOME_CD = 2 and
    t2_.SEND_RCVE_CD = 'S' and
    t2_.REQ_RESP_IND = 'R' and
    t2_.CRT_DT <= 'MM/DD/YYYY'

- Void DF:

<!-- -->

    df2_.SCCF_ID = t1.SCCF_ID and
    df2_.PGM_CD <> '6' and
    df2_.EST_IND <> 'E' and
    df2_.DISP_CD = '4' and
    df2_.STREAM_ADJ_IND <> 'Y' and
    df2_.STS_CD = 'V' and
    df2_.HOST_HOME_CD = '2' and
    df2_.FMT_DB_POST_DT <= 'MM/DD/YYYY'

=====Home Adjustment Messages to Process \<= 14 Days=====

Count of above where:

    MM/DD/YYYY - START_DATE <= 14

where START_DATE is the later of t1.CRT_DT and the FMT_DB_POST_DT of the
optional matching XREF SF as follows:

    t1.ADJ_SF_IND = 'Y' and
    sf2_.SCCF_ID = t1.XREF_SCCF_ID and
    sf2_.PGM_CD <> '6' and
    sf2_.EST_IND <> 'E' and
    sf2_.STS_CD = 'V' and
    sf2_.HOST_HOME_CD = '2' and
    sf2_.FMT_DB_POST_DT <= 'MM/DD/YYYY'

#### Calculation

\[Home Adjustment Messages to Process \<= 14 Days\] / \[Home Adjustment
Messages to Process Total\]

#### IDs

- Home Adjustment Messages to Process Total = ?
- Home Adjustment Messages to Process \<= 14 Days = ?

===Host Adjustments to Process \<= 7 Days===

#### Inputs

##### Host Adjustment Messages to Process Total

Count of messages as follows:

    t1.MSG_TP_CD = 'ADJUST' and
    t1.HOST_HOME_CD = 1 and
    t1.SEND_RCVE_CD = 'R' and
    t1.PGM_CD <> '6' and
    t1.MSG_STS_CD in ('PRSD','FNAL') and
    t1.REQ_RESP_IND = 'Q' and
    t1.RSN_CD not in ('209','220','222','273','274','275','201','242') and
    t1.CRT_DT <= 'MM/DD/YYYY' and
    No approved cancellation exists as of the report date

where there is no matching message or Void DF as follows:

- Message:

<!-- -->

    t2_.ROOT_MSG_ID = t1.ROOT_MSG_ID and
    t2_.PGM_CD <> '6' and
    t2_.MSG_STS_CD in ('PRSD','FNAL') and
    t2_.HOST_HOME_CD = 1 and
    t2_.SEND_RCVE_CD = 'S' and
    t2_.REQ_RESP_IND = 'R' and
    t2_.CRT_DT <= 'MM/DD/YYYY'

- Void DF:

<!-- -->

    df2_.SCCF_ID = t1.SCCF_ID and
    df2_.PGM_CD <> '6' and
    df2_.EST_IND <> 'E' and
    df2_.DISP_CD = '4' and
    df2_.STREAM_ADJ_IND <> 'Y' and
    df2_.STS_CD = 'V' and
    df2_.HOST_HOME_CD = '1' and
    df2_.FMT_DB_POST_DT <= 'MM/DD/YYYY'

=====Host Adjustment Messages to Process \<= 7 Days=====

Count of above where:

    MM/DD/YYYY - t1.CRT_DT <= 7

#### Calculation

\[Host Adjustment Messages to Process \<= 7 Days\] / \[Host Adjustment
Messages to Process Total\]

#### IDs

- Host Adjustment Messages to Process Total = ?
- Host Adjustment Messages to Process \<= 7 Days = ?

===Home MED REC Claims to Adjudicate \<= 30 Days===

#### Inputs

##### Home Medical Record Claims to Adjudicate Total

Count of SCCF_BASE_IDs with a matching message as follows:

    t1.PGM_CD <> '6' and
    t1.MSG_TP_CD = 'MEDREC' and
    t1.HOST_HOME_CD = 2 and
    t1.SEND_RCVE_CD = 'S' and
    t1.MSG_STS_CD in ('PRSD','FNAL') and
    t1.REQ_RESP_IND = 'Q'

where at least one of the following is true:

- Either t1.OPEN_CLOSE_IND = '2' and date(t1.CLOSE_TS) \<= 'MM/DD/YYYY'
  and there is at least one matching MEDREC/R message as follows:

<!-- -->

    t2.ROOT_MSG_ID = t1.ROOT_MSG_ID and
    t2.PGM_CD <> '6' and
    t2.HOST_HOME_CD = 2 and
    t2.SEND_RCVE_CD = 'R' and
    t2.MSG_STS_CD in ('PRSD','FNAL') and
    t2.REQ_RESP_IND = 'R' and
    t2.CRT_DT <= 'MM/DD/YYYY'

- or there is at least one matching message INFOMSG as follows:

<!-- -->

    t2.SCCF_ID = t1.SCCF_ID and
    t2.PGM_CD <> '6' and
    t2.MSG_TP_CD = 'INFOMSG' and
    t2.HOST_HOME_CD = 2 and
    t2.SEND_RCVE_CD = 'R' and
    t2.MSG_STS_CD in ('PRSD','FNAL') and
    t2.REQ_RESP_IND = 'O' and
    t2.RSN_CD in ('173','178') and
    t2.CRT_DT >= t1.CRT_DT and
    t2.CRT_DT <= 'MM/DD/YYYY'

and where neither of the following is true:

- there is a matching DF as follows:

<!-- -->

    df.SCCF_BASE_ID = t1.SCCF_BASE_ID and
    df.FMT_DB_POST_DT <= 'MM/DD/YYYY' and
    df.HOST_HOME_CD = '2' and
    df.PGM_CD <> '6' and
    df.EST_IND <> 'E' and
    df.DISP_CD = '1' and
    df.STS_CD = 'V' and
    (df.ADJ_RSN_CD = '  ' or is null or (df.ADJ_RSN_CD = '40' and df.STREAM_ADJ_IND = 'Y'))

- there is a matching message as follows:

<!-- -->

    t0.SCCF_BASE_ID = t1.SCCF_BASE_ID and
    t0.CRT_DT <= 'MM/DD/YYYY' and
    t0.MSG_TP_CD = 'ADJUST' and
    t0.HOST_HOME_CD = 2 and
    t0.SEND_RCVE_CD = 'S' and
    t0.PGM_CD <> '6' and
    t0.MSG_STS_CD in ('PRSD','FNAL') and
    t0.REQ_RESP_IND = 'Q' and
    t0.RSN_CD = '240' and
    No approved cancellation exists as of the report date

=====Home Medical Record Claims to Adjudicate \<= 30 Days=====

Count of above where:

MM/DD/YYYY - START_DATE \<= 30

where START_DATE is the later of the max MEDREC/R CRT_DT or the max
INFOMSG CRT_DT

#### Calculation

\[Home Medical Record Claims to Adjudicate \<= 30 Days\] / \[Home
Medical Record Claims to Adjudicate Total\]

#### IDs

- Home Medical Record Claims to Adjudicate Total = ?
- Home Medical Record Claims to Adjudicate \<= 30 Days = ?

===Host Misroute Messages to Process \<= 10 Days===

#### Inputs

##### Host Misroute Messages to Process Total

Count of messages as follows:

    t2.MSG_TP_CD = 'MISROUTE' and
    t2.PGM_CD <> '6' and
    t2.REQ_RESP_IND = 'Q' and
    t2.HOST_HOME_CD = 1 and
    t2.SEND_RCVE_CD = 'R' and
    t2.CRT_DT <= 'MM/DD/YYYY'

where there is no matching message as follows:

    t1.ROOT_MSG_ID = t2.ROOT_MSG_ID and
    t1.HOST_HOME_CD = 1 and
    t1.SEND_RCVE_CD = 'S' and
    t1.MST_STS_CD in ('PRSD','FNAL') and
    t1.REQ_RESP_IND = 'R' and
    t1.PGM_CD <> '6' and
    t1.CRT_DT <= 'MM/DD/YYYY'

=====Host Misroute Messages to Process \<= 10 Days=====

Count of above where:

    MM/DD/YYYY - t2.CRT_DT <= 10

#### Calculation

\[Host Misroute Messages to Process \<= 10 Days\] / \[Host Misroute
Messages to Process Total\]

#### IDs

- Host Misroute Messages to Process Total = ?
- Host Misroute Messages to Process \<= 10 Days = ?

===Claim Appeals to Resolve \<= 20 Days===

#### Inputs

##### Home Claim Appeals to Resolve Total

Count of messages as follows:

    t1.MSG_TP_CD = 'CLMAPPL' and
    t1.PGM_CD <> '6' and
    t1.REQ_RESP_IND = 'Q' and
    t1.HOST_HOME_CD = 2 and
    t1.SEND_RCVE_CD = 'R' and
    t1.CRT_DT <= 'MM/DD/YYYY'

where there is a matching SF as follows:

    sf.SCCF_BASE_ID = substring(t1.SCCF_ID,1,15) and
    sf.HOST_HOME_CD = '2' and
    sf.PGM_CD <> '6' and
    sf.EST_IND <> 'E' and
    sf.STS_CD = 'V' and
    sf.PGM_CD <> '9' or sf.CLASS_PROV_CD not in ('1','3','9','E','G')

and where there is no matching message or Void DF as follows:

- Message:

<!-- -->

    t2_.ROOT_MSG_ID = t1.ROOT_MSG_ID and
    t2_.HOST_HOME_CD = 2 and
    t2_.SEND_RCVE_CD = 'S' and
    t2_.PGM_CD <> '6' and
    t2_.MSG_STS_CD in ('PRSD','FNAL') and
    t2_.REQ_RESP_IND = 'R' and
    t2_.CRT_DT <= 'MM/DD/YYYY'

- Void DF:

<!-- -->

    df2_.SCCF_BASE_ID = substring(t1.SCCF_ID,1,15) and
    df2_.PGM_CD <> '6' and
    df2_.EST_IND <> 'E' and
    df2_.DISP_CD = '4' and
    df2_.STS_CD = 'V' and
    df2_.HOST_HOME_CD = '2' and
    df2_.FMT_DB_POST_DT <= 'MM/DD/YYYY'

=====Home Claim Appeals to Resolve \<= 20 Days=====

Count of above where:

    MM/DD/YYYY - t1.CRT_DT <= 20

##### Host Claim Appeals to Resolve Total

Count of messages as follows:

    t1.MSG_TP_CD = 'CLMAPPL' and
    t1.PGM_CD <> '6' and
    t1.REQ_RESP_IND = 'Q' and
    t1.HOST_HOME_CD = 1 and
    t1.SEND_RCVE_CD = 'R' and
    t1.CRT_DT <= 'MM/DD/YYYY'

where there is a matching SF as follows:

    sf.SCCF_BASE_ID = substring(t1.SCCF_ID,1,15) and
    sf.HOST_HOME_CD = '1' and
    sf.PGM_CD <> '6' and
    sf.EST_IND <> 'E' and
    sf.STS_CD = 'V' and
    sf.PGM_CD <> '9' or sf.CLASS_PROV_CD not in ('1','3','9','E','G')

and where there is no matching message or Void DF as follows:

- Message:

<!-- -->

    t2_.ROOT_MSG_ID = t1.ROOT_MSG_ID and
    t2_.HOST_HOME_CD = 2 and
    t2_.SEND_RCVE_CD = 'S' and
    t2_.PGM_CD <> '6' and
    t2_.MSG_STS_CD in ('PRSD','FNAL') and
    t2_.REQ_RESP_IND = 'R' and
    t2_.CRT_DT <= 'MM/DD/YYYY'

- Void DF:

<!-- -->

    df2_.SCCF_BASE_ID = substring(t1.SCCF_ID,1,15) and
    df2_.PGM_CD <> '6' and
    df2_.EST_IND <> 'E' and
    df2_.DISP_CD = '4' and
    df2_.STS_CD = 'V' and
    df2_.HOST_HOME_CD = '2' and
    df2_.FMT_DB_POST_DT <= 'MM/DD/YYYY'

=====Host Claim Appeals to Resolve \<= 20 Days=====

Count of above where:

    MM/DD/YYYY - t1.CRT_DT <= 20

#### Calculation

(\[Home Claim Appeals to Resolve \<= 20 Days\] + \[Host Claim Appeals to
Resolve \<= 20 Days\]) / (\[Home Claim Appeals to Resolve Total\] +
\[Host Claim Appeals to Resolve Total\])

#### IDs

- Home Claim Appeals to Resolve Total = ?
- Home Claim Appeals to Resolve \<= 20 Days = ?
- Host Claim Appeals to Resolve Total = ?
- Host Claim Appeals to Resolve \<= 20 Days = ?

===Home GEN INQ to Finalize \<= 14 Days===

#### Inputs

##### Home General Inquiries to Finalize Total

Count of messages as follows:

    t2.MSG_TP_CD = 'GENINQ' and
    t2.PGM_CD <> '6' and
    t2.RSN_CD <> '168' and
    t2.REQ_RESP_IND = 'Q' and
    t2.HOST_HOME_CD = 2 and
    t2.SEND_RCVE_CD = 'R' and
    t2.SCCF_ID <> '' and
    t2.CRT_DT <= 'MM/DD/YYYY';

where there is no matching message as follows:

    t1.ROOT_MSG_ID = t2.ROOT_MSG_ID and
    t1.HOST_HOME_CD = 2 and
    t1.SEND_RCVE_CD = 'S' and
    t1.PGM_CD <> '6' and
    t1.RSN_CD <> '168' and
    t1.MSG_STS_CD in ('PRSD','FNAL') and
    t1.REQ_RESP_IND = 'R' and
    t1.CRT_DT <= 'MM/DD/YYYY';

=====Home General Inquiries to Finalize \<= 14 Days=====

Count of above where:

    MM/DD/YYYY - t2.CRT_DT <= 14

#### Calculation

\[Home General Inquiries to Finalize \<= 14 Days\] / \[Home General
Inquiries to Finalize Total\]

#### IDs

- Home General Inquiries to Finalize Total = ?
- Home General Inquiries to Finalize \<= 14 Days = ?

===Host GEN INQ to Finalize \<= 14 Days===

#### Inputs

##### Host General Inquiries to Finalize Total

Count of messages as follows:

    t2.MSG_TP_CD = 'GENINQ' and
    t2.PGM_CD <> '6' and
    t2.OPEN_CLOSE_IND = '1' and
    t2.REQ_RESP_IND = 'Q' and
    t2.HOST_HOME_CD = 1 and
    t2.SEND_RCVE_CD = 'R' and
    t2.SCCF_ID <> '' and
    t2.CRT_DT <= 'MM/DD/YYYY';

where there is no matching message as follows:

    t1.ROOT_MSG_ID = t2.ROOT_MSG_ID and
    t1.HOST_HOME_CD = 1 and
    t1.SEND_RCVE_CD = 'S' and
    t1.PGM_CD <> '6' and
    t1.MSG_STS_CD in ('PRSD','FNAL') and
    t1.REQ_RESP_IND = 'R' and
    t1.CRT_DT <= 'MM/DD/YYYY';

=====Host General Inquiries to Finalize \<= 14 Days=====

Count of above where:

    MM/DD/YYYY - t2.CRT_DT <= 14

#### Calculation

\[Host General Inquiries to Finalize \<= 14 Days\] / \[Host General
Inquiries to Finalize Total\]

#### IDs

- Host General Inquiries to Finalize Total = ?
- Host General Inquiries to Finalize \<= 14 Days = ?
