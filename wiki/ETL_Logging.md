## ETL Logging

TODO - document current and future WARN,ERROR,INFO,DEBUG,TRACE logger
lines. Expected use cases and rationale.

## USAGE

### DNETL Log

The general purpose log which contains enough information to debug
issues which arise throwing developer handled Exceptions.

Rolls daily at 1am, to a tgz date-stamped, retention days is a
configured property, includes PropertiesLogger at beginning of new log.

### DIAG Log

Low level camel log and any unhandled non-com.proteligence.\*
exceptions.

Rolls daily at 1am, to a tgz date-stamped, retention days is hardcoded?

### INDEX Log

Elasticsearch error log for index rejections and version conflicts (of
which are managed externally by the development team).

File rolls at a configured max file size, retains up to 3 last tgz.

#### TODO/FIXME

##### dnetl.log

Currently 01/16/2016 - Elasticsearch CDESU file information is not
logged via the dnetl.log, as was with 3.2.x (known-issue)

##### diag.log

##### index.log

#### Wishlist

##### DEBUG

Change in ClaimDataFileState logging at the debug level; including size
in MB of (at least) the individual .cdf directories

Elasticsearch CDESU logging of current DNDW mapping counts before CDESU,
expected outgoing updates & delete counts of the specific CDESU file,
logging of DNDW mapping counts after CDESU - allowing for an expected
vs. actual comparison via the dnetl log at the debug level.