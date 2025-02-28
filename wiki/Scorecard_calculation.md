# Scorecard calculation

## Actual Date

Scorecard results are calculated for an Actual Date using a date range
with that actual date as the finish. The start date for the range is
relative to the actual date and is based on the cumulative interval
length for the scorecard results. For example, 1/20/2016 Month-to-Date
uses records from 1/1/2016 - 1/20/2016 and stores the results for
1/20/2016.

### Range Determination

If ETL has loaded files since the last scorecard calculation, ETL
calculates scorecard results for all Actual Dates from the earliest Date
Posted of the fact records loaded from those files through the latest
Date Posted of those fact records as well as all fact records from
previously-loaded files, inclusive.

In addition, ETL calculates scorecard results for any Actual Date ranges
manually submitted.

### Restriction

The above ranges are restricted as follows:

The latest date ETL may calculate scorecard for is the current date, as
indicated by the ETL server's system time in UTC; this is not changeable
by any settings. The earliest Actual Date ETL may calculate scorecard
for is restricted by the following settings; each, if set, restricts the
allowed range further than those above it and does not widen it. The
settings affect the Actual Dates allowed, not the earliest date of data
to use in the calculations.

1.  [Date-based purge retention
    period](Date-based_purge#Retention_period "wikilink") (required)
2.  Minimum-date (optional)
3.  File-driven (optional)

#### Minimum-date

If set, this absolute minimum-date setting defines the earliest Actual
Date that ETL may calculate. If this date is before the earliest date
within the date-based purge retention period, it has no effect.

#### File-driven

This setting affects only Actual Dates submitted for scorecard
calculation by loading files. If set, this relative day-count setting
defines how many days earlier than the current date the earliest Actual
Date to calculate is allowed to be. 0 refers to the current date.

## Manual Submission

The ETL command-line utility supports manually submitting scorecard
calculation requests. This takes two forms:

1.  Absolute
2.  Relative

Both forms allow submitting a contiguous inclusive range of Actual Dates
to calculate the next time scorecard calculation runs. Manual submission
does not provide the ability to override the scorecard calculation
scheduling, only the ability to specify what to calculate.

Since Plans may automate manual submission, the utility should return a
code indicating success or failure of the submission. (Indication of
actual calculation of the ranges submitted should be obtained by other
means.) The Datanet ETL log should also indicate the submission was
received.

### Absolute

Parameters:

<table>
  <tr>
    <th>Name</th>
    <th>Required?</th>
    <th>Restriction</th>
  </tr>
  <tr>
    <td>Actual Date Start</td>
    <td>Yes</td>
    <td>Must be &lt;= current date.</td>
  </tr>
  <tr>
    <td>Actual Date Finish</td>
    <td>Yes</td>
    <td>Must be &gt;= Actual Date Start.</td>
  </tr>
</table>
<br  />
This form allows a user or automated process to target a specific Actual
Date range identified as needing calculation. For example, a Plan
identifies a system error that is then corrected with a selective purge
but occurred during dates which now fall outside the File-driven
setting's window.

### Relative

Parameters:

<table>
  <tr>
    <th>Name</th>
    <th>Required?</th>
    <th>Restriction</th>
    <th>Default</th>
  </tr>
  <tr>
    <td>Start Offset</td>
    <td>Yes</td>
    <td>Must be &gt;= 0.</td>
  </tr>
  <tr>
    <td>Start Interval Length</td>
    <td>No</td>
    <td>Must be days, months, or quarters.</td>
    <td>Same as Finish Interval Length.</td>
  </tr>
  <tr>
    <td>Finish Offset</td>
    <td>Yes</td>
    <td>Must be &gt;= 0.</td>
  </tr>
  <tr>
    <td>Finish Interval Length</td>
    <td>Yes</td>
    <td>Must be days, months, or quarters.</td>
  </tr>
</table>
<br  />
The parameters are converted to an absolute Actual Date range as
follows:

1.  **Determine Actual Date Finish:** Define an initial interval with
    the current date as its finish; its start is the current date
    aligned earlier to Finish Interval Length. Move earlier from this
    initial interval the number of Finish Interval Lengths specified by
    Finish Offset, with 0 indicating the initial interval. The Actual
    Date Finish is the resultant interval's finish date.
2.  **Determine Actual Date Start:** Define an initial interval with the
    Actual Date Finish as its finish; its start is the Actual Date
    Finish aligned earlier to Start Interval Length. Move earlier from
    this initial interval the number of Start Interval Lengths specified
    by Start Offset, with 0 indicating the initial interval. The Actual
    Date Start is the resultant interval's start date.

The relative form is useful for Plans that want to reduce the time
scorecard calculation takes after files load each day; they may set the
file-driven setting to a low day count, then periodically recalculate
scorecard for the range desired using a cron job. One example of this is
calculating the official dates (month- and quarter-ends) some weeks
after a quarter ends, at which point no further data changes within that
quarter are expected (due to selective purges, for example).

## Formulas

### Low-Volume Logic

Scorecard measures may have no or low volume at the beginning of an
interval length (for example, the first few days of a month for a
measure that calculates Month-to-Date). Additionally, some Plans are
small enough that certain measures may never have volume.

Because of this, per the BCBSA, a scorecard measure should gain all
points regardless of its score in the following cases:

1.  For SF Inventory \> 30: All its volume inputs in the numerator of
    the score formula are populated and sum to 0 or 1.
2.  For all other measures in the Transactional category, excluding
    BlueExchange Index: All its volume inputs in the denominator of the
    score formula are populated and sum to 0.

If an input is defaultable and has no stored value, the default value is
used and the input is considered populated. The score and points
possible do not have special no-volume logic. If the scorecard results
are for a specific perspective, this logic applies only to measures for
that perspective.

(Aside from SF Inventory \> 30, this logic exists only in Datanet; the
BCBSA chose not to add this logic to the other measures in Datanet
Lite.)

## Future Improvements

1.  To reduce the number of Actual Dates ETL calculates the scorecard
    for due to loading files, restrict the search for the earliest Date
    Posted of data loaded to those records that either have not
    previously loaded (based on the key) or have a Last Update Timestamp
    later than the record previously loaded. This is primarily
    beneficial for Blue<sup>2</sup> messages; when the IPOR flat file
    generation process extracts a Blue<sup>2</sup> message, it also
    extracts all earlier messages in that message's message experience.
2.  ETL must also handle ITS Archive Search records specially; to be
    consistent, determining the earliest Date Posted of data loaded
    should consider the Date Posted of all records purged or restored by
    the Archive Search record rather than a date or timestamp on the
    Archive Search record itself. That is, an Archive Search record
    purges or restores fact records across all history.
