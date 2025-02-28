## DN3 Properties

Below is an outline of the current dn3.properties which can be
configured for a running ETL installation. Additional comments not found
in the default template are italicized.

#### DN3 Data Configuration

`jdbc.dn3.driverClassName=com.ibm.db2.jcc.DB2Driver`

`jdbc.dn3.url=jdbc:db2://localhost:50000/DATANET`

`jdbc.dn3.username=user`

`jdbc.dn3.password=pass`

`jdbc.dn3.defaultSchema=DATANET`

#### IPDS Data Configuration

Time (in milliseconds) between stage directory checks (i.e., how often
the server should look for newly added files)

`extract.ipds.dir.stage.watcher.sleep=5000`

Time (in milliseconds) between file delta checks before submitting for
processing (i.e., file size must not change for this amount of time
before file will be submitted for processing)

`extract.ipds.dir.stage.watcher.processWait=60000`

This is the directory where IPOR flat files are dropped for automatic
loading

`extract.ipds.dir.stage=/opt/dnetl/data/source_files_ipds/stage`

This is the directory where IPOR flat files are moved when successfully
processed

`extract.ipds.dir.success=/opt/dnetl/data/source_files_ipds/success`

This is the directory where IPOR flat files are moved when there are
errors during processing

`extract.ipds.dir.failure=/opt/dnetl/data/source_files_ipds/failure`

This is the invalid record count that once reached in an extract file
section will cause server to stop processing the file and consider it to
have failed

`extract.failure.invalidThreshold=0`

#### Claim Manager Configuration

`claimmanager.ipds.repo.location=/opt/dnetl/data/cdb_ipds`

`claimmanager.repo.compressed=true`

`claimmanager.filebuffersizek=256`

`claimmanager.partitions.number=2`

`claimmanager.concurrency.sort=2`

`claimmanager.concurrency.merge=2`

#### CDESU Configuration

`cdesu.ipds.repo.location=/opt/dnetl/data/cdesu_ipds`

#### Index Configuration

`index.ipds.es.clusterName=elasticsearch`

`index.ipds.es.hosts=localhost`

`index.ipds.es.port=9300`

This is the minimum available space in GB needed on each es node's data
volume to prevent file loading from pausing (recommend 10% of data
volume)

`index.ipds.es.minspace=5`

`index.ipds.manager.queue.factor=2`

`index.ipds.manager.bulk.size=1000`

#### Date-based Purge Configuration

`purge.datebased.enabled=false`

The schedule uses a crontab pattern and is a list of six single
space-separated fields representing second, minute, hour, day, month,
and weekday.

*We currently use a quartz crontab pattern parser, which has slightly
different restrictions than traditional cron syntax. See google. (find
link TODO).*

`purge.datebased.schedule=0 0 2 * * *`

#### Scorecard Configuration

`scorecard.autocalc.enabled=true`

*This date does not have to be set for ETL to start and/or run, which
differs from releases prior to 3.2*

`scorecard.min.date=MM-DD-YYYY`

How often auto scorecard calculation is attempted

`scorecard.autocalc.frequency.min=15`

#### Email Transport Settings

The SMTP server name

`email.smtp.serverName=your.mailhostname.com`

The FROM email address for emails sent by the system

`email.from=noreply@yourcompanyname.com`

The email server username and password for authenticated SMTP -
uncomment if needed

`#email.smtp.username=`

`#email.smtp.password=`

Use SMTPS instead of SMTP - uncomment if needed

`#email.smtp.secure=true`

#### Export File Status Reporting

Enable summary reporting (sends emails periodically about the status of
export file processing)

`report.exportfilestatus.summary.enabled=true`

Destination email address for summary reports - can be multiple
addresses separated by commas

`report.exportfilestatus.summary.emailto=etlsummarydistribution@yourcompany.com`

Generation schedule for summary reports in a crontab pattern
representing second, minute, hour, day, month, and weekday

`report.exportfilestatus.summary.schedule=0 0 8 * * *`

Lookback period for summary report in hours

`report.exportfilestatus.summary.lookback.hours=24`

Threshold age in hours for the most recent data end timestamp for each
Master Station Code/Process ID/Record Type combination before warning
about outdated data

`report.exportfilestatus.summary.outdated.threshold.hours=24`

Enable event based reporting

`report.exportfilestatus.event.enabled=true`

If true, only include failure events in report. If false, include all
events.

`report.exportfilestatus.event.failureOnly=true`

Destination email address for event reports - can be multiple addresses
separated by commas

`report.exportfilestatus.event.emailto=etleventdistribution@yourcompany.com`

#### General Settings

Default number of threads that the process uses to do compute intensive
tasks; generally, this should be the number of physical cores - 2

`etl.concurrency=2`

`etl.log.dir=/path/to/logging/dir`

`etl.log.retention.days=30`

`etl.tmpfile.location=/tmp`

#### Gap Audit

`gapaudit.ipds.targetDirectory`

## Misc Properties

jvm -Xmx setting for server process and data intensive utilities -
should be set to half physical memory

*Can't say for certain if this property is honored, or if it's that the
way the install file is configured now makes it not used.*

`JVM_MAX_MEM=2G`

*Useful for plans who do not want to have certain properties, such as
passwords, within the main dn3.properties. I believe the dnetl user
still would need read access to the alt config file*

`ETL_ALT_CONFIG_FILE=/path/to/alt/config/file`

## Internal Properties

*These properties live in internal.properties within the ETL codebase
and are not exposed to the plans in any template. They are reserved for
fine tuning or other unforeseen cases.*

do not comment this out. just leave it blank

`loader.names=`

to disable the validation of the logs being in sync between the
CDB,ES,DB

~~`fileprocessor.validate.db=true`~~

This property is now defined as the below property.

*I'm not certain if the camel case name is required, also I believe it
breaks our current naming conventions. bug reference 1522.*

TODO *this property should now be considered a seriously hidden property
as the internal.properties file does not contain the updated property,
yet still contains the old property as of 9/28.*

`extract.validate.persistentStores:true`

to disable Q file ordering based on header timestamps set this to false

`fileprocessor.enforcing=true`

elasticsearch request timeouts, hidden by default

`index.es.bulkRequestWaitMins=5`

`index.es.timeout=30000`

`index.ipds.es.bulkRequestWaitMins=5`

`index.ipds.es.timeout=30000`

throw error when index structure check in the MainBootStrap detects a
difference, hidden by default

`index.es.errorOnIndexStructureDifference=true`

max number of files for the ClaimDataManager to merge in a single round

*Check IPDSConfig, the default listed there is 500. I believe this
internal property of 250 will override the code-based default.*

`claimmanager.merge.maxfiles=250`

## Seriously Hidden Properties

*These properties are not listed within any properties file or template.
They may include past ipor-based properties (TODO).*

*Number of records sorted in a single round of sorting (?)*

*Set heuristically based on configured HEAP size and an estimated record
size which is possibly being adjusted, see bug 1514*

`claimmanager.sort.size=0`

*This property is basically a replacement for the ipor-based
retention.days property.*

`purge.datebased.retention.quarters:2`

*This property defines the size of the index.log which contains version
conflict exceptions and other cdesu errors that would otherwise balloon
the main log. In megabytes.*

`etl.log.index.size=333`

*This property was introduced to allow for fine tuning of when the
scorecard calculation schedule can run. This is a quartz cron syntax
property and if set will override the scorecard.autocalc.frequency
property even if it's set*

`scorecard.autocalc.schedule:@null`

*Introduced to limit the number of days back we calculated the
scorecard*

`scorecard.autocalc.maxLookbackDays:-1`

## Anthem LCD Properties

`extract.anthem.lcd.dir.success=/opt/dnetl/current/data/source_files_lcd/success`

`extract.anthem.lcd.dir.stage=/opt/dnetl/current/data/source_files_lcd/stage`

`extract.anthem.lcd.dir.failure=/opt/dnetl/current/data/source_files_lcd/failure`

`claimmanager.anthem.lcd.repo.location=/opt/dnetl/current/data/cdb_lcd`

## Anthem Inquiry Properties

`extract.anthem.inquiry.dir.stage.watcher.sleep=5000`

`extract.anthem.inquiry.dir.stage.watcher.processWait=60000`

`extract.anthem.inquiry.dir.stage=/opt/dnetl/data/source_files_iqd/stage`

`extract.anthem.inquiry.dir.success=/opt/dnetl/data/source_files_iqd/success`

`extract.anthem.inquiry.dir.failure=/opt/dnetl/data/source_files_iqd/failure`

`claimmanager.anthem.inquiry.repo.location=/opt/dnetl/data/cdb_iqd`

`cdesu.anthem.inquiry.repo.location=/opt/dnetl/data/cdesu_iqd`

*inquiry detail merge timer*

`cdesu.anthem.inquiry.autorun.frequency:15`
