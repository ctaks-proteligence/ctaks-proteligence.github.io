# Modern National Programs Scorecard (2020)

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

- for the ADJUST message, there exists a matching B2 message as follows:

<!-- -->

    t3.ROOT_MSG_ID = <ROOT_MSG_ID of the ADJUST message> and
    t3.MSG_TP_CD = 'CNCLADJ' and
    t3.PGM_CD <> '6' and
    t3.REQ_RESP_IND = 'Q' and
    t3.MSG_STS_CD in ('PRSD','FNAL')

- that also has a matching B2 message as follows:

<!-- -->

    t4.ROOT_MSG_ID = t3.MSG_ID and
    t4.MSG_TP_CD = 'CNCLADJ' and
    t4.PGM_CD <> '6' and
    t4.REQ_RESP_IND = 'R' and
    t4.MSG_STS_CD in ('PRSD','FNAL') and
    t4.ACT_CD = '136' and
    t4.CRT_DT <= 'MM/DD/YYYY'

## Measures

### Home DFs Processed \> 30 Days

#### Inputs

##### Home DFs Processed Total

Count of DFs as follows:

    df.HOST_HOME_CD = '2' and
    df.PGM_CD <> '6' and
    df.EST_IND <> 'E' and
    df.FMT_DB_POST_DT between 'MM/01/YYYY' and 'MM/DD/YYYY' and
    df.SCCF_SUFX_ID = '00' and
    df.DISP_CD <> '4' and
    df.STS_CD = 'V' and
    df.ADJ_CLOSEOUT_DF_IND <> 'Y'

where the DF has a matching SF as follows:

    sf.SCCF_BASE_ID = df.SCCF_BASE_ID and
    sf.PGM_CD <> '6' and
    sf.EST_IND <> 'E' and
    sf.STS_CD = 'V' and
    sf.HOST_HOME_CD = '2'

##### Home DFs Processed \> 30 Days

Count of above where:

    df.FMT_DB_POST_DT - sf.FMT_DB_POST_DT > 30

#### Calculation

\[Home DFs Processed \> 30 Days\] / \[Home DFs Processed Total\]

#### IDs

- Home DFs Processed Total = 100004
- Home DFs Processed \> 30 Days = 102001

### Host SFs Processed \> 10 Days

#### Inputs

##### Host SF Proc Total

Count of SFs as follows:

    sf.HOST_HOME_CD = '1' and
    sf.PGM_CD <> '6' and
    sf.EST_IND <> 'E' and
    sf.FMT_DB_POST_DT between 'MM/01/YYYY' and 'MM/DD/YYYY' and
    sf.STS_CD = 'V' and
    sf.SUBM_TP_CD in ('1','2')

##### Host SF Proc \> 10 Days

Count of above where:

    sf.FMT_DB_POST_DT - sf.HOST_PLAN_RCPT_DT > 10

#### Calculation

\[Host SF Proc \> 10 Days\] / \[Host SF Proc Total\]

#### IDs

- Host SF Proc Total = 100000
- Host SF Proc \> 10 Days = 102000

===Home Adjustments Processed \<= 14 Days===

#### Inputs

##### Home Adjustment Messages Proc Total

Count of B2 messages as follows:

    t1.MSG_TP_CD = 'ADJUST' and
    t1.HOST_HOME_CD = 2 and
    t1.SEND_RCVE_CD = 'R' and
    t1.PGM_CD <> '6' and
    t1.MSG_STS_CD in ('PRSD','FNAL') and
    t1.REQ_RESP_IND = 'Q' and
    t1.RSN_CD not in ('209','220','222','273','274','275','276','201','242','208','246') and
    No approved cancellation exists as of the report date

where there is an ADJ_RESP_DATE or VOID_DF_DATE as follows:

- ADJ_RESP_DATE = CRT_DT of the corresponding B2 message as follows:

<!-- -->

    t2_.ROOT_MSG_ID = t1.ROOT_MSG_ID and
    t2_.MSG_TP_CD = 'ADJUST' and
    t2_.PGM_CD <> '6' and
    t2_.MSG_STS_CD in ('PRSD','FNAL') and
    t2_.HOST_HOME_CD = 2 and
    t2_.SEND_RCVE_CD = 'S' and
    t2_.REQ_RESP_IND = 'R'

- VOID_DAT_DATE = FMT_DB_POST_DT of the corresponding DF as follows:

<!-- -->

    df2_.SCCF_ID = t1.SCCF_ID and
    df2_.PGM_CD <> '6' and
    df2_.EST_IND <> 'E' and
    df2_.DISP_CD = '4' and
    df2_.STREAM_ADJ_IND <> 'Y' and
    df2_.STS_CD = 'V' and
    df2_.HOST_HOME_CD = '2'

and one of the following conditions is true:

- VOID_DF_DATE is null and ADJ_RESP_DATE is between 'MM/01/MMMM' and
  'MM/DD/MMMM' or
- ADJ_RESP_DATE is null and VOID_DF_DATE is between 'MM/01/MMMM' and
  'MM/DD/MMMM' or
- the earlier of ADJ_RESP_DATE and VOID_DF_DATE is between 'MM/01/MMMM'
  and 'MM/DD/MMMM'

##### Home Adjustment Messages Proc \<= 14 Days

Count of above where:

    DATE2 - DATE1 <= 14

where:

- DATE2 is the earlier of the ADJ_RESP_DATE and VOID_DF_DATE and
- DATE1 is the later of t1.CRT_DT and the optional XREF_SF_DATE

where the XREF_SF_DATE is the FMT_DB_POST_DT of a matching SF as
follows:

    t1.ADJ_SF_IND = 'Y' and
    t1.XREF_SCCF_ID is not null and
    sf2_.SCCF_ID = t1.XREF_SCCF_ID and
    sf2_.PGM_CD <> '6' and
    sf2_.EST_IND <> 'E' and
    sf2_.STS_CD = 'V' and
    sf2_.HOST_HOME_CD = '2'

#### Calculation

(\[Home Adjustment Messages Proc \<= 14 Days\] + \[NASCO Control
Adjustment Messages Proc \<= 14 Days\]) / (\[Home Adjustment Messages
Proc Total\] + \[NASCO Control Adjustment Messages Proc Total\])

#### IDs

- Home Adjustment Messages Proc Total = 100066
- Home Adjustment Messages Proc \<= 14 Days = 100065
- NASCO Control Adjustment Messages Proc Total = 101069 (manual input)
- NASCO Control Adjustment Messages Proc \<= 14 Days = 101070 (manual
  input)

===Host Adjustments Processed \<= 7 Days===

#### Inputs

##### Host Adjustment Messages Proc Total

Count of B2 messages as follows:

    t1.MSG_TP_CD = 'ADJUST' and
    t1.HOST_HOME_CD = 1 and
    t1.SEND_RCVE_CD = 'R' and
    t1.PGM_CD <> '6' and
    t1.MSG_STS_CD in ('PRSD','FNAL') and
    t1.REQ_RESP_IND = 'Q' and
    t1.RSN_CD not in ('209','220','222','273','274','275','201','242') and
    No approved cancellation exists as of the report date

where there is an ADJ_RESP_DATE or VOID_DF_DATE as follows:

- ADJ_RESP_DATE = CRT_DT of the corresponding B2 message as follows

<!-- -->

    t2_.ROOT_MSG_ID = t1.ROOT_MSG_ID and
    t2_.MSG_TP_CD = 'ADJUST' and
    t2_.PGM_CD <> '6' and
    t2_.MSG_STS_CD in ('PRSD','FNAL') and
    t2_.HOST_HOME_CD = 1 and
    t2_.SEND_RCVE_CD = 'S' and
    t2_.REQ_RESP_IND = 'R'

- VOID_DAT_DATE = FMT_DB_POST_DT of the corresponding DF as follows:

<!-- -->

    df2_.SCCF_ID = t1.SCCF_ID and
    df2_.PGM_CD <> '6' and
    df2_.EST_IND <> 'E' and
    df2_.DISP_CD = '4' and
    df2_.STREAM_ADJ_IND <> 'Y' and
    df2_.STS_CD = 'V' and
    df2_.HOST_HOME_CD = '1'

and one of the following conditions is true:

- VOID_DF_DATE is null and ADJ_RESP_DATE is between 'MM/01/MMMM' and
  'MM/DD/MMMM' or
- ADJ_RESP_DATE is null and VOID_DF_DATE is between 'MM/01/MMMM' and
  'MM/DD/MMMM' or
- the earlier of ADJ_RESP_DATE and VOID_DF_DATE is between 'MM/01/MMMM'
  and 'MM/DD/MMMM'

##### Host Adjustment Messages Proc \<= 7 Days

Count of above where:

    DATE2 - t1.CRT_DT <= 7

where DATE2 is the earlier of the ADJ_RESP_DATE and VOID_DF_DATE

#### Calculation

(\[Host Adjustment Messages Proc \<= 7 Days\] + \[NASCO Par Adjustment
Messages Proc \<= 7 Days\]) / (\[Host Adjustment Messages Proc Total\] +
\[NASCO Par Adjustment Messages Proc Total\])

#### IDs

- Host Adjustment Messages Proc Total = 100068
- Host Adjustment Messages Proc \<= 7 Days = 100067
- NASCO Par Adjustment Messages Proc Total = 101072 (manual input)
- NASCO Par Adjustment Messages Proc \<= 7 Days = 101073 (manual input)

===Home MED REC Claims Adjudicated \<= 30 Days===

#### Inputs

##### Home Medical Record Claims Adjudicated Total

Count of DFs and adjustment requests as follows:

- DFs

<!-- -->

    df.FMT_DB_POST_DT between 'MM/01/MMMM' and 'MM/DD/MMMM' and
    df.HOST_HOME_CD = '2' and
    df.PGM_CD <> '6' and
    df.EST_IND <> 'E' and
    df.DISP_CD = '1' and
    df.STS_CD = 'V' and
    df.ADJ_RSN_CD = '  ' or is null or (df.ADJ_RSN_CD = '40' and df.STREAM_ADJ_IND = 'Y')

- Adjustment requests

<!-- -->

    t0.CRT_DT between 'MM/01/MMMM' and 'MM/DD/MMMM' and
    t0.MSG_TP_CD = 'ADJUST' and
    t0.HOST_HOME_CD = 2 and
    t0.SEND_RCVE_CD = 'S' and
    t0.PGM_CD <> '6' and
    t0.MSG_STS_CD in ('PRSD','FNAL') and
    t0.REQ_RESP_IND = 'Q' and
    t0.RSN_CD = '240' and
    No approved cancellation exists as of the report date

where there is at least one matching message as follows:

NOTE: DATE2 refers to the above df.FMT_DB_POST_DT or t0.CRT_DT.

- Get the MEDREC_DATE which is the maximum t2.CRT_DT of the B2 message
  as follows:

<!-- -->

    substr(t1.SCCF_ID,1,15) = <SCCF_BASE_ID of the DF or adjustment request> and
    t1.PGM_CD <> '6' and
    t1.MSG_TP_CD = 'MEDREC' and
    t1.HOST_HOME_CD = 2 and
    t1.SEND_RCVE_CD = 'S' and
    t1.MSG_STS_CD in ('PRSD','FNAL') and
    t1.REQ_RESP_IND = 'Q' and
    t1.OPEN_CLOSE_IND = '2' and
    date(t1.CLOSE_TS) <= DATE2 and

where there is a matching B2 message as follows:

    t2.ROOT_MSG_ID = t1.ROOT_MSG_ID and
    t2.PGM_CD <> '6' and
    t2.MSG_TP_CD = 'MEDREC' and
    t2.HOST_HOME_CD = 2 and
    t2.SEND_RCVE_CD = 'R' and
    t2.MSG_STS_CD in ('PRSD','FNAL') and
    t2.REQ_RESP_IND = 'R'

- Get the INFOMSG_DATE which is the maximum t2.CRT_DT the B2 messages as
  follows:

<!-- -->

    substr(t1.SCCF_ID,1,15) = <SCCF_BASE_ID of the DF or adjustment request> and
    t1.PGM_CD <> '6' and
    t1.MSG_TP_CD = 'MEDREC' and
    t1.HOST_HOME_CD = 2 and
    t1.SEND_RCVE_CD = 'S' and
    t1.MSG_STS_CD in ('PRSD','FNAL') and
    t1.REQ_RESP_IND = 'Q' and
    t1.OPEN_CLOSE_IND = '2' and

where there is a matching B2 message as follows:

    t2.SCCF_ID = t1.SCCF_ID and
    t2.PGM_CD <> '6' and
    t2.MSG_TP_CD = 'INFOMSG' and
    t2.HOST_HOME_CD = 2 and
    t2.SEND_RCVE_CD = 'R' and
    t2.MSG_STS_CD in ('PRSD','FNAL') and
    t2.REQ_RESP_IND = 'O' and
    t2.RSN_CD in ('173','178') and
    t2.CRT_DT >= t1.CRT_DT and
    t2.CRT_DT <= DATE2)

and one of the following conditions is true:

- MEDREC_DATE \<= DATE2 or
- INFOMSG_DATE is not null

##### Home Medical Record Claims Adjudicated \<= 30 Days

Count of above where:

DATE2 - DATE1 \<= 30

where DATE1 is defined as follows:

- when MEDREC_DATE is null or MEDREC_DATE \> DATE2 then DATE1 =
  INFOMSG_DATE
- when MEDREC_DATE \<= DATE2 and INFOMSG_DATE is null then DATE1 =
  MEDREC_DATE
- when MEDREC_DATE \<= DATE2 and INFOMSG_DATE not null then DATE1 = the
  later of MEDREC_DATE and INFOMSG_DATE

#### Calculation

\[Home Medical Record Claims Adjudicated \<= 30 Days\] / \[Home Medical
Record Claims Adjudicated Total\]

#### IDs

- Home Medical Record Claims Adjudicated Total = 100024
- Home Medical Record Claims Adjudicated \<= 30 Days = 100023

===Host Misrouted Messages Processed \<= 10 Days===

#### Inputs

##### Host Misrouted Messages Processed Total

Count of messages as follows:

    t1.MSG_TP_CD = 'MISROUTE' and
    t1.HOST_HOME_CD = 1 and
    t1.SEND_RCVE_CD = 'S' and
    t1.MSG_STS_CD in ('PRSD','FNAL') and
    t1.REQ_RESP_IND = 'R' and
    t1.PGM_CD <> '6' and
    t1.CRT_DT between 'MM/01/YYYY' and 'MM/DD/YYYY'

where there is a matching message as follows:

    t2.ROOT_MSG_ID = t1.ROOT_MSG_ID and
    t2.PGM_CD <> '6' and
    t2.REQ_RESP_IND = 'Q' and
    t2.HOST_HOME_CD = 1 and
    t2.SEND_RCVE_CD = 'R'
    t2.OPEN_CLOSE_IND = '2'

##### Host Misrouted Messages Processed \<= 10 Days

Count of above where:

    t1.CRT_DT - t2.CRT_DT <= 10

#### Calculation

\[Host Misrouted Messages Processed \<= 10 Days\] / \[Host Misrouted
Messages Processed Total\]

#### IDs

- Host Misrouted Messages Processed Total = 100028
- Host Misrouted Messages Processed \<= 10 Days = 100027

===Claim Appeals Resolved \<= 20 Days===

#### Inputs

##### Home Claim Appeals Resolved Total

Count of B2 messages as follows:

    t1.MSG_TP_CD = 'CLMAPPL' and
    t1.PGM_CD <> '6' and
    t1.OPEN_CLOSE_IND = '2' and
    t1.REQ_RESP_IND = 'Q' and
    t1.HOST_HOME_CD = 2 and
    t1.SEND_RCVE_CD = 'R'

where there is a matching SF as follows:

    sf.SCCF_BASE_ID = substr(t1.SCCF_ID,1,15) and
    sf.HOST_HOME_CD = '2' and
    sf.PGM_CD <> '6' and
    sf.EST_IND <> 'E' and
    sf.STS_CD = 'V' and
    sf.PGM_CD <> '9' or sf.CLASS_PROV_CD not in ('1','3','9','E','G')

where there is a CLM_RESP_DATE or VOID_DF_DATE as follows:

- CLM_RESP_DATE = CRT_DT of the corresponding B2 message as follows:

<!-- -->

    t2_.ROOT_MSG_ID = t1.ROOT_MSG_ID and
    t2_.HOST_HOME_CD = 2 and
    t2_.SEND_RCVE_CD = 'S' and
    t2_.PGM_CD <> '6' and
    t2_.MSG_STS_CD in ('PRSD','FNAL') and
    t2_.REQ_RESP_IND = 'R'

- VOID_DF_DATE = minimum FMT_DB_POST_DT of the corresponding DFs as
  follows:

<!-- -->

    df2_.SCCF_BASE_ID = substr(t1.SCCF_ID,1,15) and
    df2_.PGM_CD <> '6' and
    df2_.EST_IND <> 'E' and
    df2_.DISP_CD = '4' and
    df2_.HOST_HOME_CD = '2' and
    df2_.STS_CD = 'V' and
    df2_.FMT_DB_POST_DT >= t1.CRT_DT

and one of the following conditions is true:

- VOID_DF_DATE is null and CLM_RESP_DATE is between 'MM/01/MMMM' and
  'MM/DD/MMMM' and or
- CLM_RESP_DATE is null and VOID_DF_DATE is between 'MM/01/MMMM' and
  'MM/DD/MMMM' and or
- the earlier of CLM_RESP_DATE and VOID_DF_DATE is between 'MM/01/MMMM'
  and 'MM/DD/MMMM' and

##### Home Claim Appeals Resolved \<= 20 Days

Count of above where:

    DATE2 - t1.CRT_DT <= 20

where DATE2 is the earlier of the CLM_RESP_DATE and VOID_DF_DATE

##### Host Claim Appeals Resolved Total

Count of B2 messages as follows:

    t1.MSG_TP_CD = 'CLMAPPL' and
    t1.PGM_CD <> '6' and
    t1.OPEN_CLOSE_IND = '2' and
    t1.REQ_RESP_IND = 'Q' and
    t1.HOST_HOME_CD = 1 and
    t1.SEND_RCVE_CD = 'R'

where there is a matching SF as follows:

    sf.SCCF_BASE_ID = substr(t1.SCCF_ID,1,15) and
    sf.HOST_HOME_CD = '1' and
    sf.PGM_CD <> '6' and
    sf.EST_IND <> 'E' and
    sf.STS_CD = 'V' and
    sf.PGM_CD <> '9' or sf.CLASS_PROV_CD not in ('1','3','9','E','G')

where there is a CLM_RESP_DATE or VOID_DF_DATE as follows:

- CLM_RESP_DATE = CRT_DT of the corresponding B2 message as follows:

<!-- -->

    t2_.ROOT_MSG_ID = t1.ROOT_MSG_ID and
    t2_.HOST_HOME_CD = 1 and
    t2_.SEND_RCVE_CD = 'S' and
    t2_.PGM_CD <> '6' and
    t2_.MSG_STS_CD in ('PRSD','FNAL') and
    t2_.REQ_RESP_IND = 'R'

- VOID_DF_DATE = minimum FMT_DB_POST_DT of the corresponding DFs as
  follows:

<!-- -->

    df2_.SCCF_BASE_ID = substr(t1.SCCF_ID,1,15) and
    df2_.PGM_CD <> '6' and
    df2_.EST_IND <> 'E' and
    df2_.DISP_CD = '4' and
    df2_.HOST_HOME_CD = '1' and
    df2_.STS_CD = 'V' and
    df2_.FMT_DB_POST_DT >= t1.CRT_DT

and one of the following conditions is true:

- VOID_DF_DATE is null and CLM_RESP_DATE is between 'MM/01/MMMM' and
  'MM/DD/MMMM' or
- CLM_RESP_DATE is null and VOID_DF_DATE is between 'MM/01/MMMM' and
  'MM/DD/MMMM' or
- the earlier of CLM_RESP_DATE and VOID_DF_DATE is between 'MM/01/MMMM'
  and 'MM/DD/MMMM'

##### Host Claim Appeals Resolved \<= 20 Days

Count of above where:

    DATE2 - t1.CRT_DT <= 20

where DATE2 is the earlier of the CLM_RESP_DATE and VOID_DF_DATE

#### Calculation

(\[Home Claim Appeals Resolved \<= 20 Days\] + \[Host Claim Appeals
Resolved \<= 20 Days\]) / (\[Home Claim Appeals Resolved Total\] +
\[Host Claim Appeals Resolved Total\])

#### IDs

- Home Claim Appeals Resolved Total = 102003
- Home Claim Appeals Resolved \<= 20 Days = 102004
- Host Claim Appeals Resolved Total = 102009
- Host Claim Appeals Resolved \<= 20 Days = 102010

===Home GEN INQ Finalized \<= 14 Days===

#### Inputs

##### Home General Inquiries Finalized Total

Count of messages as follows:

    t1.MSG_TP_CD = 'GENINQ' and
    t1.HOST_HOME_CD = 2 and
    t1.SEND_RCVE_CD = 'S' and
    t1.PGM_CD <> '6' and
    t1.RSN_CD <> '168' and
    t1.CRT_DT between 'MM/01/MMMM' and 'MM/DD/MMMM' and
    t1.MSG_STS_CD in ('PRSD','FNAL') and
    t1.REQ_RESP_IND = 'R'

where there is a matching message as follows:

    t2.ROOT_MSG_ID = t1.ROOT_MSG_ID and
    t2.PGM_CD <> '6' and
    t2.RSN_CD <> '168' and
    t2.OPEN_CLOSE_IND = '2' and
    t2.REQ_RESP_IND = 'Q' and
    t2.HOST_HOME_CD = 2 and
    t2.SEND_RCVE_CD = 'R' and
    t2.SCCF_ID <> ''

##### Home General Inquiries Finalized \<= 14 Days

Count of above where:

    t1.CRT_DT - t2.CRT_DT <= 14

#### Calculation

\[Home General Inquiries Finalized \<= 14 Days\] / \[Home General
Inquiries Finalized Total\]

#### IDs

- Home General Inquiries Finalized Total = 100015
- Home General Inquiries Finalized \<= 14 Days = 102011

===Host GEN INQ Finalized \<= 14 Days===

#### Inputs

##### Host General Inquiries Finalized Total

Count of messages as follows:

    t1.MSG_TP_CD = 'GENINQ' and
    t1.HOST_HOME_CD = 1 and
    t1.SEND_RCVE_CD = 'S' and
    t1.PGM_CD <> '6' and
    t1.CRT_DT between 'MM/01/MMMM' and 'MM/DD/MMMM' and
    t1.MSG_STS_CD in ('PRSD','FNAL') and
    t1.REQ_RESP_IND = 'R'

where there is a matching message as follows:

    t2.ROOT_MSG_ID = t1.ROOT_MSG_ID and
    t2.PGM_CD <> '6' and
    t2.OPEN_CLOSE_IND = '2' and
    t2.REQ_RESP_IND = 'Q' and
    t2.HOST_HOME_CD = 1 and
    t2.SEND_RCVE_CD = 'R' and
    t2.SCCF_ID <> ''

##### Host General Inquiries Finalized \<= 14 Days

Count of above where:

    t1.CRT_DT - t2.CRT_DT <= 14

#### Calculation

\[Host General Inquiries Finalized \<= 14 Days\] / \[Host General
Inquiries Finalized Total\]

#### IDs

- Host General Inquiries Finalized Total = 100018
- Host General Inquiries Finalized \<= 14 Days = 102015
