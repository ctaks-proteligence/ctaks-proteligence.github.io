# Testing Strategies

# Testing Strategies

*this section will outline methods, tools and approaches used when
testing and validating different portions of Datanet ETL*

## Scorecard Inputs

*defined by
[National_Programs_Scorecard](National_Programs_Scorecard "wikilink")*

input descriptors are built into the Extract-Datagen utility at
`definitions/scorecard/{INPUT_NAME}/.xml`

### Input Value Comparison

#### expected input values

for each set of input xml descriptors which create extract files which
in turn load through ETL and calculate scorecard inputs, a set of
expected <INPUT /> values must also exist, defined like the actual
results, for use with the `compare-xml` tool to detect differences.

all of the expected values that correlate to xml descriptors for
specific scorecard inputs have not yet been completed

#### actual results retrieval

as a set of attributes

`db2 "select xmlelement(name input, xmlattributes(input_id, valid_date, perf_level_id, datespan_id, calculated_value, entered_value, external_value)) from dn3.dnt_scorecards_input_values";`

*yields*

<INPUT INPUT_ID="100068" VALID_DATE="2013-06-21" PERF_LEVEL_ID="27" DATESPAN_ID="1" CALCULATED_VALUE=".000000"/>

The columns <code>entered_value_ts\</code, `calculated_value_ts`, and
`external_value_ts` can also be exported and used to compare subsequent
updates to the same set of input values.

## Date-based Purge

*defined by [Date-based purge](Date-based_purge "wikilink")*

Using the Extract Datagen utility, generate extract files from the xml descriptor sfi_df_rf.xml

Specify a `--start-date YYYY-MM-DD` and `--end-date YYYY-MM-DD` which
will span the purge retention date logged in dnetl.log

Load files with date based purge disabled; after apply changes has
finished, run a CDR export and Elasticsearch request to to view the
current closed claim experiences stored.

Enable date based purge; after apply changes has finished, run another
CDR export and Elasticsearch request to view the remaining closed claim
experiences, verifying remain claim experiences.

## Flex Reports

General file loading consists of one or more initials, minimum of 3-6
months, to establish a base CDR and then sets of daily files which
contain new claim experiences, updates to existing claim experiences
from the initials and logical purges of existing claim experiences.

Create a set of daily files containing claim experiences that contain
only one of each type; new, update, purge, invalid

## Gap Audit

*defined by [Gap Audit](Gap_Audit "wikilink")*

Using the Extract Datagen utility, generate extract files from the xml descriptors

### SF, RF only Claim Experience


Claim key tested is:
`sccfBase="332201608900000" boid="FACC" perspective="1"`

- Generate the claim experience files using the
  **`test-missing-df-sf_rf.xml`**
- Load the SFI, RF files generated
- After the merge, run gap-audit
- Verify that the sccf base 332201608900000 is **present** for 00-99
  sccf suffix within `FACC`
- Generate the filler files using the **`test-missing-df-filler.xml`**
- Load the generated DF file
- After the merge, run gap-audit
- Verify that the sccf base 332201608900000 is **NOT** present for 00-99
  sccf suffix within `FACC`

### DF only Claim Experience


Defined by [DF only](Gap_Audit#DF_only "wikilink")

Claim key tested is:
`sccfBase="332201608900001" boid="FACC" perspective="1"`

- Generate the claim experience files using the
  **`test-missing-sf-df.xml`**
- Load the DF file generated
- After the merge, run gap-audit
- Verify that the sccf base 332201608900001 is **present** for 00-99
  sccf suffix within `FACC`
- Generate the gap filler files using the
  **`test-missing-sf-filler.xml`**
- Load the generated SFI file
- After the merge, run gap-audit
- Verify that the sccf base 332201608900001 is **NOT** present for 00-99
  sccf suffix within `FACC`

### SF only Claim Experience


Defined by [SF only](Gap_Audit#SF_only "wikilink")

Claim key tested is:
`sccfBase="332201608900002" boid="FACC" perspective="1"`

- Generate the claim experience files using the **`test-nogap-sf.xml`**
- Load the SFI file generated
- After the merge, run gap-audit
- Verify that the sccf base 332201608900002 is **NOT** present for 00-99
  sccf suffix within `FACC`

### SF and DF only Claim Experience


Defined by [SF, DF only](Gap_Audit#SF.2C_DF_only "wikilink")

Claim key tested is:
`sccfBase="332201608900003" boid="FACC" perspective="1"`

- Generate the claim experience files using the
  **`test-nogap-sf-df.xml`**
- Load the SFI, DF files generated
- After the merge, run gap-audit
- Verify that the sccf base 332201608900003 is **NOT** present for 00-99
  sccf suffix within `FACC`

### Multiple Sccf Suffix Claim Experience


Defined by [Multiple SCCF
Suffixes](Gap_Audit#Multiple_SCCF_Suffixes "wikilink")

Claim key tested is:
`sccfBase="332201608900004" boid="FACC" perspective="1"`

- Generate the claim experience files using the
  **`test-multi-suffix-gap-sf_df.xml`**
- Load the SFI and DF files generated
- After the merge, run gap-audit
- Verify that the sccf base 332201608900004 is **present** for 00-99
  sccf suffix within `FACC`
- Generate the gap filler files using the
  **`test-multi-suffix-filler-df_rf.xml`**
- Load the generated DF, RF files
- After the merge, run gap-audit
- Verify that the sccf base 332201608900004 is **NOT** present for 00-99
  sccf suffix within `FACC`

=== Custom Claim Experience (Program Code = 6) ===


Defined by [Custom](Gap_Audit#Custom "wikilink")

Claim key tested is:
`sccfBase="332201608900000" boid="FACC" perspective="1"`

- Custom Claim Experiences are out of scope so we don't do any gap
  processing on them
- Generate the custom claim experience files using the
  **`test-custom-nogap-sf_rf.xml`**
- Load the SFI and RF files generated
- After the merge, run gap-audit
- Verify that the sccf base 332201608900000 is **NOT** present for 00-99
  sccf suffix within `FACC`

### Missing ITS Records Claim Experience


Claim key tested is:
`sccfBase="332201608900006" boid="FACC" perspective="1"`

- Generate the claim experience files using the
  **`test-missing-its-b2.xml`**
- Load the B2 file generated
- After the merge, run gap-audit
- Verify that the sccf base 332201608900006 is **present** for 00-99
  sccf suffix within `FACC`
- Generate the gap filler files using the
  **`test-missing-its-filler.xml`**
- Load the generated SFI, DF, RF files
- After the merge, run gap-audit
- Verify that the sccf base 332201608900006 is **NOT** present for 00-99
  sccf suffix within `FACC`

## Date-based Purge

*defined by [Date-based purge](Date-based_purge "wikilink")*

### Identified Cases

<table>
  <tr align="center">
    <th rowspan="2">rowspan="2"All Closed</th>
    <th colspan="3">colspan="3"for greatest sccf suffix</th>
    <th rowspan="2">rowspan="2"newer slective purge rec exists?</th>
    <th rowspan="2">rowspan="2"should be purged?</th>
  </tr>
  <tr align="center">
    <th>adj resp msg with act_cd 301 or 304 exists?</th>
    <th>newer adj resp msg with act_cd 300 exists?</th>
    <th>void df exists?</th>
  </tr>
  <tr align="center">
    <td>&amp;#10004;</td>
    <td>&amp;#10008;</td>
    <td>-</td>
    <td>-</td>
    <td>&amp;#10008;</td>
    <td>&amp;#10004;</td>
  </tr>
  <tr align="center">
    <td>&amp;#10004;</td>
    <td>&amp;#10004;</td>
    <td>&amp;#10008;</td>
    <td>&amp;#10008;</td>
    <td>&amp;#10008;</td>
    <td>&amp;#10008;</td>
  </tr>
  <tr align="center">
    <td>&amp;#10004;</td>
    <td>&amp;#10004;</td>
    <td>&amp;#10004;</td>
    <td>-</td>
    <td>&amp;#10008;</td>
    <td>&amp;#10004;</td>
  </tr>
  <tr align="center">
    <td>&amp;#10004;</td>
    <td>&amp;#10004;</td>
    <td>&amp;#10008;</td>
    <td>&amp;#10004;</td>
    <td>&amp;#10008;</td>
    <td>&amp;#10004;</td>
  </tr>
  <tr align="center">
    <td>&amp;#10004;</td>
    <td>&amp;#10008;</td>
    <td>-</td>
    <td>-</td>
    <td>&amp;#10004;</td>
    <td>&amp;#10008;</td>
  </tr>
  <tr align="center">
    <td>&amp;#10008;</td>
    <td>&amp;#10008;</td>
    <td>&amp;#10008;</td>
    <td>&amp;#10008;</td>
    <td>&amp;#10008;</td>
    <td>&amp;#10008;</td>
  </tr>
</table>
<br  />
## Anthem Modules

### External Input Dataserver

*defined by
[Externally-generated_inputs_(Anthem)](Externally-generated_inputs_(Anthem) "wikilink")*

#### ASA

#### Inquiries Resolved

### ETL Components

#### LCD

*there currently exists no LCD requirements wiki article*

#### IQD

*defined by
[Inquiry_FlexReports_(Anthem)](Inquiry_FlexReports_(Anthem) "wikilink")*
