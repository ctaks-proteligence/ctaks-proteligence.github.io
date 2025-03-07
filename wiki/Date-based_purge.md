# Date-based purge

Datanet stores both open and closed records; therefore, it requires a
data retention policy to prevent indefinitely growing the storage
required. Date-based purge removes old, closed records to satisfy this
requirement.

# Versions

Datanet 3.3 (ETL 3.1) and earlier differ in behavior from the described.
Datanet 15.5 (3.4/ETL 3.2) does not include date-based purge.

# Retention period

Date-based purge operates with a rolling retention period. Because
accurate scorecard calculation requires all records for the first day of
a quarter through the date in that quarter being calculated, the
retention period is defined as a count of full quarters of data. The
number of quarters is configurable; the minimum is 1 and the default is
2. This setting also restricts which quarters scorecard calculations
will occur for.

In order to even out the number of records removed each time date-based
purge runs (for user understanding and acceptance as well as performance
reasons), date-based purge should occur daily despite the retention
period being defined as full quarters. This has the effect of retaining
a rolling quarter of data in addition to the configured number. That
rolling quarter consists of a shrinking portion before the full quarters
(due to date-based purge) and a growing portion after the full quarters
(due to file loading).

# Record removal

Date-based purge operates at the claim experience level; it must not
remove only some records comprising a claim experience. A claim
experience is removed only if all the following are true of the records
present:

A.  **Non-purged fact records:**
    1.  All in the claim experience are closed.
    2.  The latest Date Closed (explicit or implicit) in the claim
        experience is earlier than the earliest date to retain.
    3.  For the greatest SCCF suffix existing, any of the following are
        true:
        - No adjustment response message exists with an Action Code of
          301 or 304; or
        - An adjustment response message exists with an Action Code of
          300 and a Date Created later than or equal to the most-recent
          Date Created of all adjustment response messages with an
          Action Code of 301 or 304; or
        - A Void DF exists.
    4.  No records in the claim experience have STS_CD that is not V or
        R ().
B.  **Selective-purge records:** The latest Last Update Timestamp in the
    claim experience is earlier than the earliest date to retain.

Selectively-purged fact records are not considered; they may have been
purged while open, and will never close due to being purged.

Selective-purge records are considered, because the records purged may
be restored at a later date. This allows a restore to occur within the
retention period without requiring the claim experience to be re-loaded
using the Gap Audit process. (Without this, the first date-based purge
to occur after loading the purge record could purge its claim
experience.)

# Potential future changes

1.  Consider a setting to store less in Elasticsearch than in the CDR.
