Note: IPDS 15.0 files cannot be used in production Datanet. The IPOR
splitter process filters out a huge chunk of records that we want. It
will be addressed in 15.5, but presumably that means we can't use 15.0
files -- because the splitter is what's responsible for creating files
that only contain a certain Process Number. However, 15.0 processing
will need to be functional for Datanet 3.4 alpha release (5/1/2015).

IPDS Flow

**Step 1: File Trait Discovery**

May be a combination of sources:

- route
- header read

Note: record data type could also come from route if files are sent to
different directories. However, this structure would have to be
maintained in success/failure for recovery

At a minimum, we must know:

- the file type (IPDS)
- the record data type (e.g., B2, SFI, etc.)
- the version (e.g., 15.0, 15.5)

Note: in the sample IPDS files received, the version is garbage.
Confirmation required that the IPDS version will be in this field.
Without it, we cannot maintain historical parsing.

**Step 2: Binary Record Extraction**

Based on the file traits, each record can be extracted from the file.
There are two potential extraction techniques:

- fixed-width
- variable-length

Note: variable-length may or may not be required. Need confirmation

The extraction technique will be defined in the metadata for each set of
file traits along with the data necessary to implement:

- for fixed-width, the record length
- for variable-length, the prefix length

Note: variable-length prefixes may require parsing to return an int

Note: location in the file/stream may need to be returned along with the
record

**Step 3: Binary Record Parsing**

Based on the file traits, a parser will interpret the record based on a
sequential list of field definitions. A field definition for binary
records will require:

- the field name
- the field length
- the field alignment (left vs right)
- the type it should be converted to

Note: whether or not a field is validated at this point is under
discussion

Some fields contain values that determine the field definitions that
should also be parsed. These fields will implement a special interface
that will handle that determination and return a sequential list of
field definitions. This list can either be parsed immediately or
deferred until the original field definition list is completely parsed.

Note: some records contain duplicate fields. The parser's behavior in
duplicate data situations should be documented for use in diagnosing
"bad data" problems (i.e., the duplicate fields have data that doesn't
match)

**Step 4: B2 Record Aggregation and 2nd Pass XML Parsing**

B2 payload records will need to be collected, and the xml payload will
need to be concatenated in the order of the sequence numbers on the
records. Once the XML payload is complete, it will need to be sent to an
XML parser to extract the record.

**WARNING**: XML payloads contain PHI. This data cannot be stored in
ETL, and the security of any process that touches XML payload data needs
to be evaluated.

**Step 5: Batch Data Enrichment**

After a record has been parsed, it may be enriched with batch data.
Batch data that is stored in ETL (e.g., file identifier) should be
stored as record data. Batch data that is used for transform only (e.g.,
master station code, process id, etc.) can be stored externally and
passed along the route.

**Step 6: Transformation**

The following transformations are normally run:

- plan exit (applicable only to records that contain BOID)
- performance level (not applicable to all records. needs further
  discussion)
- cycle time calculations
- sccf id to sccf base/sffx and vice versa
- other calculations (will be specified in more detail later on a type
  by type basis)