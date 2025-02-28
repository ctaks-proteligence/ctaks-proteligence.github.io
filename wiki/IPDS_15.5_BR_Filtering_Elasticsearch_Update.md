# Elasticsearch Update Claim Entry Filtering

**Assumptions:**

- The input to the process is a complete collection of claim entries for
  a claim or message experience.
- Duplicate claim entries have been removed from the collection.

**Filtering Steps:**

The list of filtering steps below must be performed in the designated
order.

The order of processing is defined by the following requirements:

First, a 'current' view of the claim experience is achieved by removing
historical, purged, or orphaned entries. Second, fields that are
required for the calculation of later fields, e.g., close date, will be
determined as soon as possible. Third, remaining fields that require
calculation will be handled. Fourth, fields that are 'copied' from one
entry to another will be performed. This includes custom enrichment.
This copying is performed last to keep the entries as small as possible.

1.  Cross Reference Entry Removal
2.  Obsolete Claim Entry Removal
3.  15-Digit Selective (SCCF Base) and Selective (SCCF ID) Purged Entry
    Removal
4.  Orphaned Claim Entry Removal
5.  Submission Close Date Calculation
6.  Disposition Close Date Calculation
7.  ITS Message Cycle Time Filter
8.  SF Claim Closed Cycle Time Filter
9.  B2 Claim SF Cycle Time Filter
10. B2 Escalation Filter
11. B2 Adjustment Message Response Approval Indicator Data Filter
12. B2 Recoupment Report Indicator Filter
13. B2 Partial Response Indicator Filter
14. B2 Response Reason Code Filter
15. DF Submission Data Enrichment Filter
16. RF Disposition Data Enrichment Filter
17. Custom Enrichment Filters
