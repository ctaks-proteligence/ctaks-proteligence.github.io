## Filtering

If inclusive, filters for the same column are Ored. If exclusive,
filters for the same column are Anded. If a column has both inclusive
and exclusive filters, the inclusive filter set and exclusive filter set
are Anded. Filter sets across columns are also Anded. This logic applies
regardless of filter type.

All filters are case-insensitive; text is lower-cased when a filter is
added.

## Multi-Value Columns

Certain columns are defined as multi-value textual columns.

### Display (UI Grid and Export)

All values for a multi-value column display on one line in the cell; the
values appear left to right sorted in ascending order for consistency.
Values are separated by two spaces.

Multi-value columns are auto-sized. (Current multi-value columns and
those expected in the future have limited number and length.)

### Selecting

The user may select any part of the text in the cell. If the selection
includes part of a value, the selection should expand to encompass that
entire value. Note that it is possible for the selection to include part
of a value at the beginning and part of a value at the end, in which
case the selection should expand in both directions to encompass both
entire values. If the selection starts or ends with any of the
separation space characters, the selection should contract to exclude
those space character(s).

### Sorting

Because a record can have multiple values that would sort in different
positions in the results, multi-value columns are not available for
sorting.

### Filtering

Multi-value columns have two filter types available, titled "Multiple"
and "Other".

#### Multiple

This filter type provides four options:

1.  Include if has any
2.  Exclude if has any
3.  Include if has all
4.  Exclude if has all

Each line in the multi-line text box is considered a value. Wildcards
are allowed. The suggested values list contains individual values, not
multiple-value combinations.

If the user adds a "has all" filter with multiple values, only one
filter is added, using all values.

If the user adds a "has any" filter with multiple values, each value
becomes a separate filter. This matches the behavior of the List filter
type for single-value columns, making it easier to remove a subset of
values later if desired.

An inclusive applied filter displays as "has {comma-separated value
list}" (e.g. "has value1" or "has value1, value2"). An exclusive applied
filter displays as "does not have {comma-separated value list}" (e.g.
"does not have value1" or "does not have value1, value2").

<figure>
<img src="Mockup_-_Filters_window_-_filter_type_Multiple.png"
title="File:Mockup_-_Filters_window_-_filter_type_Multiple.png" />
<figcaption><a
href="File:Mockup_-_Filters_window_-_filter_type_Multiple.png">File:Mockup_-_Filters_window_-_filter_type_Multiple.png</a></figcaption>
</figure>

#### Other

This filter type provides two options:

1.  Include if has value(s)
2.  Exclude if has value(s)

The applied filter displays as, respectively:

1.  "has value(s)"
2.  "does not have value(s)"