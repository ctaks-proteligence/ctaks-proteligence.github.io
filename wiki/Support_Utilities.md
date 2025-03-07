# Support Utilities

## Current Utilities

Landing place for current and future projects in the Support repo

### Datagen

#### Extract-Datagen

Creates a set of IPDS files containing correlated claim experiences or
XML templates, sourced xml Claim Experience Definition files

Latest Info:

- Check out the project from SVN repo
  [`https://svn.proteligence.com/repos/support/extract-datagen/trunk`](https://svn.proteligence.com/repos/support/extract-datagen/trunk)
- Build the jar file using maven
- This utility now requires a boid, master station code and process id
  to be supplied on the command line.
- Contains xml descriptor templates for gap audit as described in
  [Testing_Strategies](Testing_Strategies "wikilink")
- Contains xml descriptor templates for scorecard inputs - INPROGRESS
- ~~This utility requires db2 driver to connect to the database and get
  Performance Level related info~~
- ~~This utility also requires PlanExit.xml and dn3.properties on the
  class path~~

Usage:

- `java -jar extract-datagen-0.0.1-SNAPSHOT.jar -b {boid} -msc {masterstationcode} -p {processid} -d {xmltemplate} -xml|-dat`
- ~~`java -cp path-to-extract-datagen-jar;path-to-db2-driver;path-to-dn3-properties-planexit com.proteligence.datanet.tools.test.data.main.ExtractGenerator <parameters>`~~

Required Parameters

1.  -b/--boid \<arg\>:
    - the boid to be used with the records to be generated. The boid can
      also be specified in the xml definition, if specified the boid in
      the xml will be used.
2.  -msc/--master-station-code \<arg\>:
    - the master station code to be used with the records to be
      generated. This is currently not output on the XML descriptor.
3.  -pid/--process-id \<arg\>:
    - the process id to be used with the records generated. This is
      currently not output on the XML descriptor.
4.  -dat/--export-extract:
    - tells the utility to generate binary extract files
5.  -xml/--export-xml:
    - tells the utility to export the records as xml. This is a good way
      to generate custom xml templates as well as to verify the
      correctness of the utility
6.  -d/--definitions <path/to/xml>:
    - used to provide the claim experience definition xml file, if the
      value provided is an absolute path it will be used as is, all
      relative paths are resolved as classpath resources

Optional Parameters

1.  -v/--version \<arg\>:
    - the IPP Release version '15.0' - '16.0.1', defaults to the latest
      IPP Release
2.  -o/--output \<arg\>:
    - the output directory to which the generated files will be written
3.  -c/--count \<arg\>
    - specify the total number of claim experiences to be generated
4.  -sd/--start-date \<YYYY-MM-DD\>:
    - specify the start date which claim experiences are generated with,
      1 claim experience per day up to today or \<end-date\>
5.  -ed/--end-date \<YYYY-MM-DD\>:
    - specify the end date which claim experiences are generated up to,
      from \<start-date\> or today.
6.  -z/--compress
    - indicates whether the output should be compressed, defaults to
      false - only applies to flat file outputs

------------------------------------------------------------------------

#### Anthem-Datagen

Creates a set of LCD or IQD files based upon a specified IPDS CDR repo
and user \# of record limit.

#### IPDS-File-Multiplier

Replicates IPDS records sourced from an existing IPDS flat file out to 1
or more new files with manufactured SCCF's and/or replicated across
BOID/MSC.

### XML

#### XML-Formatter

Takes a 3.2.5 or later form of the CDR export XML and makes it 1 record
per line, allows for additional format type filtering and for the
inclusion/exclusion of individual tags. Using *-c* skips lines that
contain no xml events and adds a new line to the end of each record to
make the export easier to read. Can take in CDR export information from
System.in or from an already existing XML file.

#### Compare-XML

Accepts a more recent CDR export file `'left'` as `Standard.INPUT` and
an older CDR export file `'right'` as a command line option `-f` then
prints the differences between the two XML data structures to
`Standard.OUTPUT`

#### Check-XML-Sort

Verifies that a CDR XML output file was written out in the correct
sorted order by Claim Key - TODO define this specifically, it's a
comparator in ETL...

### additional

#### CDESU-Deserializer

Allows for the reading of a CDESU update file captured or from the error
directory, as .esu files are kryo serialized and compressed.

~~====CDR-Import-Utility====~~ ~~Creates a usable CDR from an XML input.
This input is not the output from CDR export.~~

## Utilities in Development

#### IPDS-File-Reader

A remake of an old utility that told you file header information and
such in a pretty print way.

#### External-Input-Datagen

Creates CSV files along with expected calcs for testing ASA & Inquiry
Resolved.
