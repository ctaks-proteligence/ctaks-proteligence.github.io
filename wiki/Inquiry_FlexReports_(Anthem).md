# Inquiry FlexReports (Anthem)

The purpose of Inquiry FlexReports is to provide Anthem with inquiries
associated with ITS and Blue<sup>2</sup> records in the same user
interface and with the same functionality they are used to for ITS and
Blue<sup>2</sup> records. This includes showing/hiding columns, sorting,
and filtering, as well as exporting.

# Files

The Inquiry detail data is provided to Datanet's ETL process as
fixed-width EBCDIC extract files. Inquiry detail records are stored in
multiple upstream systems; each system produces a separate file. Files
are not concatenated together.

The process that places files in the Datanet ETL stage directory must
not overwrite existing files. The stage directory for Inquiry detail
files may be different from the stage directory for IPDS files.

Each file contains 1 header record, 0 or more detail records, and 1
trailer record. The layout of the header, detail, and trailer records
will match that agreed upon by Anthem and Proteligence. All systems
provide the same layout, though each system may populate a different
subset of optional fields in the detail records.

## Header

The header record corresponds to the Anthem Phase 2 header record
layout, with EXPR-FL-TYP := "IQD".

The header contains a start and end timestamp; for each system, the
start timestamp of the next file must be 1 microsecond later than the
end timestamp of the previous file. There is no distinction made between
the initial file loaded for each system and subsequent files (generally
provided daily). Timestamps between systems as well as file types
(Inquiry detail, IPDS, etc.) are independent.

## Detail

### Restrictions

Each detail record's key consists of BOID, Perspective, Activation Plan
Code, Inquiry \#, Sequence \#, and SCCF Base. Activation Plan Code
and/or Sequence \# may be blank; all other key fields must be populated.
Last Update Timestamp must be populated; all other non-key fields are
optional and therefore may be blank.

All records for the same Activation Plan Code-Inquiry \# combination
must have the same Perspective and BOID.

A file must not contain multiple records with the same key.

Error handling will behave the same as for IPDS files. Any record where
a required field is not populated will error out and not load. Any
optional field that is not populated will load as blank.

### Updates

Each file detail record is a logical record; each logical record is
independent of all other logical records. Therefore, there is no
relationship between records with the same Inquiry \# but other key
fields differing. Though each system may extract all records for an
Inquiry \# when any record for that Inquiry \# changes, this means there
is no ability to change the key for a record (for example, re-numbering
Sequence \#) by doing so.

Each record includes a Last Update Timestamp; a record with the same key
as another record but a newer Last Update Timestamp replaces the other
record. If multiple records exist with the same key and Last Update
Timestamp, ETL will retain only one record.

# Fields

The columns provided on FlexReports will match those agreed upon by
Anthem and Proteligence; they include both fields provided on the
extract files and derived fields.

Inquiry detail records are either open or closed ("processed"): open
records have Date Closed as blank, while closed records have Date Closed
populated.

While the extract file provides SCCF, only SCCF Base is relevant.
Therefore, Inquiry FlexReports should provide a SCCF Base column rather
than a SCCF column (i.e. only the leftmost 15 digits of the SCCF).

# Plan Exit

The systems producing Inquiry detail files are differentiated by Master
Station Code and Process ID, as with other file types; however, ETL must
not translate BOIDs in Inquiry detail files.

# Performance Levels

Due to the key not including all components necessary to assign a
Performance Level, each Inquiry detail record must determine its
associated Performance Levels using the IPDS Claim Experience that
matches its BOID, Perspective, and SCCF Base. Load Inquiry detail
records from a file whether or not a corresponding IPDS Claim Experience
exists at that time.

For data security reasons, an Inquiry detail record must not appear on
FlexReports (nor load to Elasticsearch) unless it has Performance
Level(s) assigned (and therefore has a corresponding IPDS Claim
Experience). Inquiry detail records loaded before a corresponding IPDS
Claim Experience must appear on FlexReports once a corresponding IPDS
Claim Experience loads.

# Purge

Inquiry detail does not have selective purge.

The initial release of Inquiry detail does not include date-based purge.

# ETL Status E-mails

ETL should send event e-mails for Inquiry detail files with the same
behavior and form as for all other files. Summary e-mails should include
a section for Inquiry detail files with the same behavior and form as
the other files' sections.

# Locked Views

Inquiry FlexReports consist of 4 locked views. These views can be
customized and then saved, providing the same functionality as ITS and
Blue<sup>2</sup> FlexReports. The 4 locked views are:

- Home Open Inquiries
- Host Open Inquiries
- Home Processed Inquiries
- Host Processed Inquiries

# Claim Experience pane

The Claim Experience pane will not contain Inquiry detail records.

The Claim Experience pane functions on an Inquiry FlexReport identically
to how it functions on an ITS FlexReport, using the SCCF Base,
Perspective, and BOID as the key.

# Cross-pollination

An Excel-based tool will provide Inquiry \#s and Sequence \#s on
SCCF-based (i.e. ITS and Blue<sup>2</sup>) report exports. To use this
functionality, users will export a SCCF-based report view and an Inquiry
report view to Excel, then run the tool. Cross-pollination of SCCF-based
and Inquiry detail records is not provided in Datanet proper.
