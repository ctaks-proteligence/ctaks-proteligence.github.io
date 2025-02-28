# IPDS 15.5 BR Filtering Scorecard

# Scorecard Claim Entry Filtering

**Assumptions:**

- The input to the process is a complete collection of claim entries for
  a claim or message experience.
- Duplicate claim entries have been removed from the collection.

**Filtering Steps:**

The list of filtering steps below must be performed in the designated
order.

The order of processing is defined by the following requirements:

First, an interval-dependent collection of claim entries is achieved by
removing all records that should not exist for the purposes of the
calculation, i.e., are after the end of the interval. Second, a
'current' view of the claim experience is achieved by removing
historical, purged, or orphaned entries.

1.  After Interval Entry Removal
2.  Obsolete Claim Entry Removal
3.  15-Digit Selective (SCCF Base) and Selective (SCCF ID) Purged Record
    Removal
4.  Invalid Status Claim Entry Removal
5.  Orphaned Claim Entry Removal

There is also the potential to filter claim experiences by relevance,
i.e., are any post dates in the interval range or is there an open SF.
Currently, the scorecard pipe does a similar check to prevent the
unnecessary processing of old claim experiences.
