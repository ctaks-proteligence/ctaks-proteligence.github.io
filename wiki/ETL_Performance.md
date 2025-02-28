This page contains benchmarks on the ETL performance in Datanet.

# Serialization Benchmark

### Purpose

The purpose of this benchmark was to compare the different serialization
and deserialization methods in the 2.1.1 version of ETL and ETL 3.2.0
(with Kryo 2.24.0).

### Controls/Conditions

The serialization was performed on Amazon AWS hardware with a m2.4xlarge
(Rotational Hard Disk) instance type without EBS volume. The
serialization times were recorded while writing to disk and to memory.
Also, the deserialization times were recorded when reading from an
'output.txt' file (Disk). Objects initialized of the Entry class with
random fields (shown below) were used for the serialization. For the
tests, 100 different Entry objects were constructed and cycled an
appropriate amount of times to adequately test the speeds of
serialization.

For the disk tests, a DataOutputStream was initialized with a
FileOutputStream writing to 'output.txt'. Many entries were serialized
and timed for several trials with varying sets of parameters shown in
the next section. Under the same parameters, the entries were
deserialized and the time was recorded. For the memory tests, a
DataOutputStream was constructed, which wrote to a
ByteArrayOutputStream, and timed through several cycles of
serialization.

**Parameters**

- Type of serializer (Kryo or ETL 2.1.1)

<!-- -->

- Compression (LZF compression or None)

<!-- -->

- CompatibleFieldSerializer (Kryo only)

<!-- -->

- Class and object serialization (Kryo only)

**Results (Entry)**

The times show that the implementation of Kryo resulted in a significant
performance loss (generally slower serialization and deserialization)
when compared to the ETL 2.1.1 serialization techniques. Compression
also resulted in a large serialization performance loss compared to
uncompressed serializations. The values in the table are the average
results after 5 trials. Each value was measured for the serialization
and deserialization of 500000 Entry objects.

<table>
  <tr align="center">
    <th colspan="5">Parameters</th>
    <th>Memory Times</th>
    <th colspan="2">Disk Times</th>
  </tr>
  <tr align="center">
    <th>Trial</th>
    <th>Serializer Type</th>
    <th>Compressed</th>
    <th>Compatible</th>
    <th>Class and Object</th>
    <th>Serialization (ms)</th>
    <th>Serialization (ms)</th>
    <th>Deserialization (ms)</th>
  </tr>
  <tr align="center">
    <td>1</td>
    <td>ETL 2.1.1</td>
    <td>No</td>
    <td>No</td>
    <td>No</td>
    <td>12488.8</td>
    <td>18768.6</td>
    <td>9452.6</td>
  </tr>
  <tr align="center">
    <td>2</td>
    <td>Kryo 2.24</td>
    <td>No</td>
    <td>No</td>
    <td>No</td>
    <td>31278.4</td>
    <td>38011.6</td>
    <td>29957.4</td>
  </tr>
  <tr align="center">
    <td>3</td>
    <td>Kryo 2.24</td>
    <td>No</td>
    <td>Yes</td>
    <td>No</td>
    <td>31272.0</td>
    <td>37692.0</td>
    <td>29914.2</td>
  </tr>
  <tr align="center">
    <td>4</td>
    <td>Kryo 2.24</td>
    <td>No</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>31583.0</td>
    <td>37663.0</td>
    <td>29960.0</td>
  </tr>
  <tr align="center">
    <td>5</td>
    <td>ETL 2.1.1</td>
    <td>Yes</td>
    <td>No</td>
    <td>No</td>
    <td>87749.2</td>
    <td>92785.6</td>
    <td>15791.8</td>
  </tr>
  <tr align="center">
    <td>6</td>
    <td>Kryo 2.24</td>
    <td>Yes</td>
    <td>No</td>
    <td>No</td>
    <td>106585.0</td>
    <td>111958.2</td>
    <td>36366.4</td>
  </tr>
  <tr align="center">
    <td>7</td>
    <td>Kryo 2.24</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>No</td>
    <td>106652.4</td>
    <td>111723.0</td>
    <td>36399.8</td>
  </tr>
  <tr align="center">
    <td>8</td>
    <td>Kryo 2.24</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>106577.4</td>
    <td>111745.2</td>
    <td>36461.6</td>
  </tr>
</table>
<br  />
**Results (EntryB)**

The times show that the implementation of Kryo resulted in a significant
performance loss (generally slower serialization and deserialization)
when compared to the ETL 2.1.1 serialization techniques. Compression
also resulted in a large serialization performance loss compared to
uncompressed serializations. The values in the table are the average
results after 5 trials. Each value was measured for the serialization
and deserialization of 100000000 EntryB objects.

<table>
  <tr align="center">
    <th colspan="5">Parameters</th>
    <th>Memory Times</th>
    <th colspan="2">Disk Times</th>
  </tr>
  <tr align="center">
    <th>Trial</th>
    <th>Serializer Type</th>
    <th>Compressed</th>
    <th>Compatible</th>
    <th>Class and Object</th>
    <th>Serialization (ms)</th>
    <th>Serialization (ms)</th>
    <th>Deserialization (ms)</th>
  </tr>
  <tr align="center">
    <td>1</td>
    <td>ETL 2.1.1</td>
    <td>No</td>
    <td>No</td>
    <td>No</td>
    <td>-</td>
    <td>64564.2</td>
    <td>65503.0</td>
  </tr>
  <tr align="center">
    <td>2</td>
    <td>Kryo 2.24</td>
    <td>No</td>
    <td>No</td>
    <td>No</td>
    <td>-</td>
    <td>30061.6</td>
    <td>42930.4</td>
  </tr>
  <tr align="center">
    <td>3</td>
    <td>Kryo 2.24</td>
    <td>No</td>
    <td>Yes</td>
    <td>No</td>
    <td>-</td>
    <td>29948.4</td>
    <td>42233.0</td>
  </tr>
  <tr align="center">
    <td>4</td>
    <td>Kryo 2.24</td>
    <td>No</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>-</td>
    <td>31079.4</td>
    <td>43016.0</td>
  </tr>
  <tr align="center">
    <td>5</td>
    <td>ETL 2.1.1</td>
    <td>Yes</td>
    <td>No</td>
    <td>No</td>
    <td>-</td>
    <td>63829.6</td>
    <td>50105.2</td>
  </tr>
  <tr align="center">
    <td>6</td>
    <td>Kryo 2.24</td>
    <td>Yes</td>
    <td>No</td>
    <td>No</td>
    <td>-</td>
    <td>36208.2</td>
    <td>42411.2</td>
  </tr>
  <tr align="center">
    <td>7</td>
    <td>Kryo 2.24</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>No</td>
    <td>-</td>
    <td>36383.4</td>
    <td>42536.8</td>
  </tr>
  <tr align="center">
    <td>8</td>
    <td>Kryo 2.24</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>-</td>
    <td>38064.4</td>
    <td>43166.8</td>
  </tr>
</table>
<br  />
### Entry

**Constructors**

- public Entry() //creates an instance of Entry with null fields

<!-- -->

- public Entry(boolean a) //creates an instance of Entry with all
  non-null randomly generated fields

<!-- -->

- public Entry(String m1, String m2, long m3, int m4, double m5, String
  m6, Date m7) //creates an instance of Entry with specified field
  values

**Fields**

- private String m1 //10000 character, randomly generated string

<!-- -->

- private String m2 //500 characeter, randomly generated string

<!-- -->

- private long m3 //randomly generated long between 0 and Long.MAX_VALUE

<!-- -->

- private int m4 //randomly generated int between 0 and
  Integer.MAX_VALUE

<!-- -->

- private double m5 //randomly generated double

<!-- -->

- private String m6 // 10 character, randomly generated string

<!-- -->

- private Date m7 //default constructed Date object

### EntryB

**Constructors**

- public EntryB() //creates an instance of EntryB with null fields

<!-- -->

- public EntryB(boolean a) //creates an instance of EntryB with all
  non-null randomly generated fields

<!-- -->

- public EntryB(String randString, String short1, String short2, String
  short3, String short4, String short5, String short6) //creates an
  instance of EntryB with specified field values

**Fields**

- private String randString //32 character, randomly generated string

<!-- -->

- private String short1 //a choice of four, 4 character long random
  strings

<!-- -->

- private String short2 //a choice of four, 8 character long random
  strings

<!-- -->

- private String short3 //a choice of four, 2 character long random
  strings

<!-- -->

- private String short4 //a choice of four, 1 character long random
  strings

<!-- -->

- private String short5 //a choice of four, 4 character long random
  strings

<!-- -->

- private String short6 //a choice of four, 4 character long random
  strings

# Compression Benchmark

### Purpose

The purpose of this benchmark was to compare Snappy and LZF compression
libraries for speed.

### Controls/Conditions

The serialization was performed on Amazon AWS hardware with a m2.4xlarge
(Rotational Hard Disk) instance type without EBS volume. The
serialization times were recorded while writing to disk. Also, the
deserialization times were recorded when reading from an 'output.txt'
file. Objects initialized of the Entry class with random fields (shown
below) were used for the serialization. For the tests, 100 different
Entry objects were constructed and cycled an appropriate amount of times
to adequately test the speeds of serialization.

For each test, 5000 cycles (500000 Entries) were serialized and timed
for several trials with varying sets of parameters shown in the next
section. Under the same parameters, the 500000 entries were deserialized
and the time was recorded.

**Parameters**

- Type of serializer (Kryo or ETL 2.1.1)

<!-- -->

- CompatibleFieldSerializer (Kryo only)

<!-- -->

- Class and object serialization (Kryo only)

<!-- -->

- Compression (LZF compression, Snappy, or LZ4)

**Results (Entry)**

The times show that the runtime of LZF compression is significantly
longer than the Snappy compression (After 5 trials of each parameter
set).

<table>
  <tr align="center">
    <th colspan="4">Parameters</th>
    <th colspan="3">LZF</th>
    <th colspan="3">Snappy</th>
    <th colspan="2">LZ4</th>
  </tr>
  <tr align="center">
    <th>Trial</th>
    <th>Serialization Type</th>
    <th>Compatible</th>
    <th>Class and Object</th>
    <th>Serialization (ms)</th>
    <th>Deserialization (ms)</th>
    <th>Size On Disk (kb)</th>
    <th>Serialization (ms)</th>
    <th>Deserialization (ms)</th>
    <th>Size On Disk (kb)</th>
    <th>Serialization (ms)</th>
    <th>Deserialization (ms)</th>
  </tr>
  <tr align="center">
    <td>1</td>
    <td>ETL 2.1.1</td>
    <td>No</td>
    <td>No</td>
    <td>100341.8</td>
    <td>15534.8</td>
    <td>5095129</td>
    <td>20172.6</td>
    <td>9007.8</td>
    <td>5152184</td>
    <td>20693.2</td>
    <td>8768.0</td>
  </tr>
  <tr align="center">
    <td>2</td>
    <td>Kryo 2.24</td>
    <td>No</td>
    <td>No</td>
    <td>111760.2</td>
    <td>36302.0</td>
    <td>5093164</td>
    <td>38987.8</td>
    <td>29481.4</td>
    <td>5150216</td>
    <td>39487.2</td>
    <td>29429.8</td>
  </tr>
  <tr align="center">
    <td>3</td>
    <td>Kryo 2.24</td>
    <td>Yes</td>
    <td>No</td>
    <td>111827.4</td>
    <td>36358.8</td>
    <td>5093164</td>
    <td>38957.6</td>
    <td>29410.2</td>
    <td>5150216</td>
    <td>39624.4</td>
    <td>29561.6</td>
  </tr>
  <tr align="center">
    <td>4</td>
    <td>Kryo 2.24</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>112182.0</td>
    <td>36286.2</td>
    <td>5093676</td>
    <td>38968.8</td>
    <td>29517.2</td>
    <td>5150700</td>
    <td>39369.8</td>
    <td>29494.4</td>
  </tr>
</table>
<br  />
**Results (EntryB)**

Theses are the results after serializing 1000000 cycles (100000000
EntryB objects).

<table>
  <tr align="center">
    <th colspan="4">Parameters</th>
    <th colspan="3">LZF</th>
    <th colspan="3">Snappy</th>
    <th colspan="2">LZ4</th>
  </tr>
  <tr align="center">
    <th>Trial</th>
    <th>Serialization Type</th>
    <th>Compatible</th>
    <th>Class and Object</th>
    <th>Serialization (ms)</th>
    <th>Deserialization (ms)</th>
    <th>Size On Disk (kb)</th>
    <th>Serialization (ms)</th>
    <th>Deserialization (ms)</th>
    <th>Size On Disk (kb)</th>
    <th>Serialization (ms)</th>
    <th>Deserialization (ms)</th>
  </tr>
  <tr align="center">
    <td>1</td>
    <td>ETL 2.1.1</td>
    <td>No</td>
    <td>No</td>
    <td>63551.2</td>
    <td>48978.8</td>
    <td>-</td>
    <td>769.8</td>
    <td>703.6</td>
    <td>-</td>
    <td>54891.8</td>
    <td>52589.2</td>
  </tr>
  <tr align="center">
    <td>2</td>
    <td>Kryo 2.24</td>
    <td>No</td>
    <td>No</td>
    <td>37074.2</td>
    <td>43941.4</td>
    <td>-</td>
    <td>181.6</td>
    <td>208.0</td>
    <td>-</td>
    <td>27525.0</td>
    <td>42946.8</td>
  </tr>
  <tr align="center">
    <td>3</td>
    <td>Kryo 2.24</td>
    <td>Yes</td>
    <td>No</td>
    <td>37134.4</td>
    <td>43920.4</td>
    <td>-</td>
    <td>180.6</td>
    <td>207.6</td>
    <td>-</td>
    <td>27538.8</td>
    <td>42902.6</td>
  </tr>
  <tr align="center">
    <td>4</td>
    <td>Kryo 2.24</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>38161.2</td>
    <td>44831.6</td>
    <td>-</td>
    <td>187.8</td>
    <td>211.0</td>
    <td>-</td>
    <td>28545.8</td>
    <td>43746.4</td>
  </tr>
</table>
<br  />
### Entry

**Constructors**

- public Entry() //creates an instance of Entry with null fields

<!-- -->

- public Entry(boolean a) //creates an instance of Entry with all
  non-null randomly generated fields

<!-- -->

- public Entry(String m1, String m2, long m3, int m4, double m5, String
  m6, Date m7) //creates an instance of Entry with specified field
  values

**Fields**

- private String m1 //10000 character, randomly generated string

<!-- -->

- private String m2 //500 characeter, randomly generated string

<!-- -->

- private long m3 //randomly generated long between 0 and Long.MAX_VALUE

<!-- -->

- private int m4 //randomly generated int between 0 and
  Integer.MAX_VALUE

<!-- -->

- private double m5 //randomly generated double

<!-- -->

- private String m6 // 10 character, randomly generated string

<!-- -->

- private Date m7 //default constructed Date object

### EntryB

**Constructors**

- public EntryB() //creates an instance of EntryB with null fields

<!-- -->

- public EntryB(boolean a) //creates an instance of EntryB with all
  non-null randomly generated fields

<!-- -->

- public EntryB(String randString, String short1, String short2, String
  short3, String short4, String short5, String short6) //creates an
  instance of EntryB with specified field values

**Fields**

- private String randString //32 character, randomly generated string

<!-- -->

- private String short1 //a choice of four, 4 character long random
  strings

<!-- -->

- private String short2 //a choice of four, 8 character long random
  strings

<!-- -->

- private String short3 //a choice of four, 2 character long random
  strings

<!-- -->

- private String short4 //a choice of four, 1 character long random
  strings

<!-- -->

- private String short5 //a choice of four, 4 character long random
  strings

<!-- -->

- private String short6 //a choice of four, 4 character long random
  strings

# Correlated Data Apply Changes Benchmark

### Purpose

The purpose of this benchmark is to test the speed of apply changes
using a constant data set.

### Controls/Conditions

The serialization was performed on Jason's laptop using the 200000000
record init data and the four 200000 record daily correlated data files
generated using the TestDataGenerator and TestCorrelatedDataGenerator.
The CDF was loaded via the TestDataLoaderRunner. Apply changes was
triggered via the ApplyChangesRunner. The data set consisted of a random
selection of four different Entry types described below.

**Parameters**

- Start State - The state the CDF was in before applying changes
  (corresponds to ClaimDataFileState enum)

<!-- -->

- Record Count - The number of records loaded

<!-- -->

- File Count - The number of CDF files corresponding to the record count
  and start state

<!-- -->

- Disk Type

<!-- -->

- Partition Count (claimmanager.partitions.number)

<!-- -->

- CDF Compression (claimmanager.repo.compressed)

<!-- -->

- Temp File Compression (claimmanager.repo.compressed)

<!-- -->

- Concurrency (claimmanager.concurrency.sort and
  claimmanager.concurrency.merge)

<!-- -->

- Buffer Size (claimmanager.filebuffersizek)

<!-- -->

- Sort Threshold (claimmanager.sort.size)

<!-- -->

- Max Merge Files (claimmanager.merge.maxfiles)

**Results**

TODO

<table>
  <tr align="center">
    <th colspan="11">Parameters</th>
    <th colspan="3">Results</th>
  </tr>
  <tr align="center">
    <th>Start State</th>
    <th>Record Count</th>
    <th>File Count</th>
    <th>Disk Type</th>
    <th>Partition Count</th>
    <th>CDF Compression</th>
    <th>Temp File Compression</th>
    <th>Concurrency</th>
    <th>Buffer Size</th>
    <th>Sort Threshold</th>
    <th>Max Merge Files</th>
    <th>Sort Time (ms)</th>
    <th>Merge Rounds</th>
    <th>Merge Time (ms)</th>
  </tr>
  <tr align="center">
    <td>SORT</td>
    <td>200800000</td>
    <td>5</td>
    <td>rotational</td>
    <td>4</td>
    <td>LZF</td>
    <td>LZF</td>
    <td>8</td>
    <td>256</td>
    <td>81920</td>
    <td>128</td>
    <td>2178745</td>
    <td>5</td>
    <td>10275306</td>
  </tr>
  <tr align="center">
    <td>SORT</td>
    <td>200800000</td>
    <td>5</td>
    <td>SSD</td>
    <td>4</td>
    <td>LZF</td>
    <td>LZF</td>
    <td>8</td>
    <td>256</td>
    <td>81920</td>
    <td>128</td>
    <td>853537</td>
    <td>5</td>
    <td>2138207</td>
  </tr>
  <tr align="center">
    <td>MERGE</td>
    <td>~49180000</td>
    <td>150</td>
    <td>rotational</td>
    <td>4</td>
    <td>LZF</td>
    <td>LZF</td>
    <td>8</td>
    <td>256</td>
    <td>N/A</td>
    <td>128</td>
    <td>N/A</td>
    <td>2</td>
    <td>1849704</td>
  </tr>
  <tr align="center">
    <td>MERGE</td>
    <td>~49180000</td>
    <td>150</td>
    <td>SSD</td>
    <td>4</td>
    <td>LZF</td>
    <td>LZF</td>
    <td>8</td>
    <td>256</td>
    <td>N/A</td>
    <td>128</td>
    <td>N/A</td>
    <td>2</td>
    <td>309034</td>
  </tr>
  <tr align="center">
    <td>MERGE</td>
    <td>~49180000</td>
    <td>150</td>
    <td>rotational</td>
    <td>4</td>
    <td>LZF</td>
    <td>LZF</td>
    <td>8</td>
    <td>256</td>
    <td>N/A</td>
    <td>128</td>
    <td>N/A</td>
    <td>2</td>
    <td>1804845</td>
  </tr>
  <tr align="center">
    <td>MERGE</td>
    <td>~49180000</td>
    <td>150</td>
    <td>SSD</td>
    <td>4</td>
    <td>LZF</td>
    <td>LZF</td>
    <td>8</td>
    <td>256</td>
    <td>N/A</td>
    <td>128</td>
    <td>N/A</td>
    <td>2</td>
    <td>358441</td>
  </tr>
  <tr align="center">
    <td>MERGE</td>
    <td>~49180000</td>
    <td>150</td>
    <td>rotational</td>
    <td>4</td>
    <td>LZF</td>
    <td>GZIP</td>
    <td>8</td>
    <td>256</td>
    <td>N/A</td>
    <td>128</td>
    <td>N/A</td>
    <td>2</td>
    <td>1696512</td>
  </tr>
</table>
<br  />
### Entries

The data set contains randomly generated BenchmarkEntry objects. These
objects were designed to approximate IPP data by having a three key
fields that are always fully serialized/deserialized for claim
experience grouping, approximately five scorecard fields that are always
fully serialized/deserialized to represent the fields that would be used
by the scorecard pipe, and approximately thirteen extended fields that
are stored in a map but only serialized/deserialized as bytes unless
specifically requested. Extended field key names were generated by
appending the the first eight characters from the hash of the class name
and field number to the field number. This hashing was done to ensure
field names were constant in a given entry implementation but unique
across implementations. All BenchmarkEntry objects share a common parent
class that contains the three key fields and two of the scorecard
fields. The remaining type specific fields are on the specific
implementations.

#### Common Fields

*Key Fields*

- String key1 //randomly generated lowercase character string with
  length of 4

<!-- -->

- String key2 //string randomly picked from \["1", "2"\]

<!-- -->

- String key3 //randomly generated numeric string with length of 10

*Scorecard Fields*

- String scorecard1 //randomly generated lowercase character string with
  length of 8

<!-- -->

- String scorecard2 //randomly generated numeric string with length of 3

#### BenchmarkEntryA Fields

*Scorecard Fields*

- String abcd //randomly generated lowercase character string with
  length of 4

<!-- -->

- long efgh //randomly generated date between 2000-01-01 and 2015-04-01

*Extended Field Map*

- long 1 //randomly generated date between 2000-01-01 and 2015-04-01

<!-- -->

- int 2 //randomly generated int between 0 and 100

<!-- -->

- String 3 //randomly generated lowercase character string with length
  of 6

<!-- -->

- String 4 //randomly generated lowercase character string with length
  of 4

<!-- -->

- String 5 //string randomly picked from \["Y", "N"\]

<!-- -->

- int 6 //randomly generated int between Integer.MIN_VALUE and
  Integer.MAX_VALUE

<!-- -->

- String 7 //randomly generated numeric string with length of 4

<!-- -->

- long 8 //randomly generated date between 2000-01-01 and 2015-04-01

<!-- -->

- double 9 //randomly generated double between 0.0 and 1.0

<!-- -->

- long 10 //randomly generated date between 2000-01-01 and 2015-04-01

<!-- -->

- String 11 //string randomly picked from \["A", "B", "C", "D", "E"\]

<!-- -->

- int 12 //int randomly picked from \[5, 7, 10\]

<!-- -->

- int 13 //randomly generated int between 0 and 5000

#### BenchmarkEntryB Fields

*Scorecard Fields*

- long ijkl //randomly generated double between 0.0 and 1.0

<!-- -->

- String mnop //string randomly picked from \["W", "N", "P"\]

<!-- -->

- String qrst //randomly generated lowercase character string with
  length of 2

*Extended Field Map*

- String 1 //randomly generated numeric string with length of 6

<!-- -->

- String 2 //string randomly picked from \["Y", "N"\]

<!-- -->

- long 3 //randomly generated date between 2000-01-01 and 2015-04-01

<!-- -->

- String 4 //randomly generated numeric string with length of 6

<!-- -->

- double 5 //randomly generated double between 0.0 and 1.0

<!-- -->

- double 6 //randomly generated double between 0.0 and 1.0

<!-- -->

- String 7 //randomly generated lowercase character string with length
  of 20

<!-- -->

- int 8 //randomly generated int between Integer.MIN_VALUE and
  Integer.MAX_VALUE

<!-- -->

- long 9 //randomly generated date between 2000-01-01 and 2015-04-01

<!-- -->

- int 10 //randomly generated int between Integer.MIN_VALUE and
  Integer.MAX_VALUE

#### BenchmarkEntryC Fields

*Scorecard Fields*

- double uvwx //randomly generated double between 0.0 and 1.0

*Extended Field Map*

- int 1 //randomly generated int between Integer.MIN_VALUE and
  Integer.MAX_VALUE

<!-- -->

- int 2 //randomly generated int between Integer.MIN_VALUE and
  Integer.MAX_VALUE

<!-- -->

- String 3 //randomly generated numeric string with length of 8

<!-- -->

- String 4 //randomly generated lowercase character string with length
  of 3

<!-- -->

- String 5 //string randomly picked from \["Y", "N"\]

<!-- -->

- long 6 //randomly generated date between 2000-01-01 and 2015-04-01

<!-- -->

- double 7 //randomly generated double between 0.0 and 1.0

<!-- -->

- String 8 //string randomly picked from \["R", "Q", "P"\]

<!-- -->

- String 9 //randomly generated lowercase character string with length
  of 4

<!-- -->

- long 10 //randomly generated date between 2000-01-01 and 2015-04-01

<!-- -->

- String 11 //randomly generated numeric string with length of 4

<!-- -->

- int 12 //randomly generated int between 0 and 100

<!-- -->

- int 13 //randomly picked from \[1, 0\]

<!-- -->

- double 14 //randomly generated double between 0.0 and 1.0

<!-- -->

- int 16 //randomly generated int between Integer.MIN_VALUE and
  Integer.MAX_VALUE

#### BenchmarkEntryD Fields

*Scorecard Fields*

- String yzab //randomly generated numeric string with length of 4

<!-- -->

- long cdef //string randomly picked from \["O", "C"\]

*Extended Field Map*

- String 1 //randomly generated lowercase character string with length
  of 4

<!-- -->

- String 2 //string randomly picked from \["S", "R"\]

<!-- -->

- long 3 //randomly generated date between 2000-01-01 and 2015-04-01

<!-- -->

- String 4 //string randomly picked from \["Y", "N"\]

<!-- -->

- double 5 //randomly generated double between 0.0 and 1.0

<!-- -->

- int 6 //randomly generated int between Integer.MIN_VALUE and
  Integer.MAX_VALUE

<!-- -->

- String 7 //randomly generated numeric string with length of 6

<!-- -->

- long 8 //randomly generated date between 2000-01-01 and 2015-04-01

<!-- -->

- String 9 //randomly generated lowercase character string with length
  of 2

<!-- -->

- int 10 //randomly generated int between Integer.MIN_VALUE and
  Integer.MAX_VALUE

<!-- -->

- String 11 //randomly generated numeric string with length of 3

<!-- -->

- double 12 //randomly generated double between 0.0 and 1.0

# AdjustmentResponseApproval Updates Benchmark

### Purpose

The purpose of this benchmark was to benchmark partial updates to
elasticsearch.

### Controls/Conditions

In this case, an elasticsearch index of over 3.6 million BlueSquared
entries was recovered on a 4 node cluster on Amazon as hi1.4xlarge spot
instances (ami-8e682ce6). Running on a m2.4xlarge instance, the boid,
the perspective, and the messageid for each entry was written to memory.
20000000 random sets of these keys were generated and used to perform
random partial updates to elasticsearch (the adjustment response
approval field), simulating the behavior of the
AdjustmentResponseApprovalFilter. The BulkIndexerManager was set to have
a max bulk size of 1000 and queue size of 2.

**Results**

The total run time was just over an hour (3644 sec). Each bulk index of
1000 updates was timed and the results show that of the 20000 bulk
indexes, the majority of the times were between 100 and 300 ms with no
evidence of slow down.
