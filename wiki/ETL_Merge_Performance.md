# ETL Merge Performance

This page contains benchmark information related to ETL merge
performance

# Comparison between 3.2.5, 3.3.3 with and without LCD and with Date-based Purge enabled

The AR initials and a set of AR Incremental were loaded into an IPDS
CDR.

LCD data was generated for the complete dataset. One record per clmexp
in the AR dataset. Other than the key fields the data was random data.
It was loaded and had apply changes run on it b/4 any other merge
testing or loading.

This was a special testing rig and only loaded the files, applied the
changes to the CDR and wrote the CDESU files. Everything else was
stripped out of the executable.

AWS c3.8xl - Intel(R) Xeon(R) CPU E5-2680 v2 @ 2.80GHz x 32
hyperthreads - specint 48.9

Total record count was ~1.1 million.

<table>
  <tr align="center">
    <th>Num Partitions</th>
    <th>3.2.5 IPDS  Init</th>
    <th>3.3.0 IPDS  Init</th>
    <th>3.3.0 IPDS  Init w/ Date Based Purge</th>
    <th>3.3.0 IPDS w/ LCD  Init</th>
    <th>3.2.5 IPDS Incr</th>
    <th>3.3.0 IPDS Incr</th>
    <th>3.3.0 IPDS Incr w/ Date Based Purge</th>
    <th>3.3.0 IPDS w/ LCD Incr</th>
  </tr>
  <tr align="center">
    <td>1</td>
    <td>361</td>
    <td>378</td>
    <td>391</td>
    <td>388</td>
    <td>213</td>
    <td>182</td>
    <td>205</td>
    <td>181</td>
  </tr>
  <tr align="center">
    <td>2</td>
    <td>190</td>
    <td>161</td>
    <td>164</td>
    <td>166</td>
    <td>108</td>
    <td>36</td>
    <td>49</td>
    <td>37</td>
  </tr>
  <tr align="center">
    <td>4</td>
    <td>100</td>
    <td>96</td>
    <td>99</td>
    <td>102</td>
    <td>55</td>
    <td>20</td>
    <td>23</td>
    <td>19</td>
  </tr>
  <tr align="center">
    <td>8</td>
    <td>60</td>
    <td>88</td>
    <td>89</td>
    <td>89</td>
    <td>28</td>
    <td>13</td>
    <td>15</td>
    <td>17</td>
  </tr>
  <tr align="center">
    <td>12</td>
    <td>45</td>
    <td>89</td>
    <td>90</td>
    <td>90</td>
    <td>20</td>
    <td>16</td>
    <td>15</td>
    <td>19</td>
  </tr>
  <tr align="center">
    <td>16</td>
    <td>39</td>
    <td>89</td>
    <td>89</td>
    <td>90</td>
    <td>16</td>
    <td>16</td>
    <td>18</td>
    <td>22</td>
  </tr>
</table>
<br  />
# Comparison between AWS and Softlayer(bare metal)

The AR initials and a set of AR Incremental were loaded into an IPDS
CDR. This was a special testing rig and only loaded the files, applied
the changes to the CDR and wrote the CDESU files. Everything else was
stripped out of the executable.

AWS c3.8xl - Intel(R) Xeon(R) CPU E5-2680 v2 @ 2.80GHz x 32
hyperthreads - specint 48.9

SL - 2 x 2.6GHz Intel Xeon-Haswell (E5-2690-V3-DodecaCore) - specint
63.5

Total record count was ~1.1 million.

<table>
  <tr align="center">
    <th>Num Partitions</th>
    <th>AWS Init Load Time</th>
    <th>SL Init Load Time</th>
    <th>AWS Init Sort Time</th>
    <th>SL Init Sort Time</th>
    <th>AWS Init Merge Time</th>
    <th>SL Init Merge Time</th>
    <th>AWS Incr Load Time</th>
    <th>SL Incr Load Time</th>
    <th>AWS Incr Sort Time</th>
    <th>SL Incr Sort Time</th>
    <th>AWS Incr Merge Time</th>
    <th>SL Incr Merge Time</th>
  </tr>
  <tr align="center">
    <td>1</td>
    <td>774</td>
    <td>568</td>
    <td>55</td>
    <td>51</td>
    <td>516</td>
    <td>320</td>
    <td>16</td>
    <td>13</td>
    <td>1</td>
    <td>&lt;1</td>
    <td>284</td>
    <td>198</td>
  </tr>
  <tr align="center">
    <td>2</td>
    <td>684</td>
    <td>570</td>
    <td>28</td>
    <td>27</td>
    <td>157</td>
    <td>112</td>
    <td>23</td>
    <td>14</td>
    <td>3</td>
    <td>&lt;1</td>
    <td>36</td>
    <td>35</td>
  </tr>
  <tr align="center">
    <td>4</td>
    <td>604</td>
    <td>582</td>
    <td>15</td>
    <td>14</td>
    <td>80</td>
    <td>64</td>
    <td>14</td>
    <td>14</td>
    <td>&lt;1</td>
    <td>&lt;1</td>
    <td>19</td>
    <td>16</td>
  </tr>
  <tr align="center">
    <td>8</td>
    <td>607</td>
    <td>577</td>
    <td>12</td>
    <td>9</td>
    <td>77</td>
    <td>60</td>
    <td>14</td>
    <td>14</td>
    <td>&lt;1</td>
    <td>&lt;1</td>
    <td>13</td>
    <td>9</td>
  </tr>
  <tr align="center">
    <td>12</td>
    <td>612</td>
    <td>572</td>
    <td>9</td>
    <td>8</td>
    <td>79</td>
    <td>68</td>
    <td>14</td>
    <td>14</td>
    <td>&lt;1</td>
    <td>&lt;1</td>
    <td>16</td>
    <td>9</td>
  </tr>
  <tr align="center">
    <td>16</td>
    <td>607</td>
    <td>572</td>
    <td>9</td>
    <td>7</td>
    <td>79</td>
    <td>74</td>
    <td>14</td>
    <td>14</td>
    <td>&lt;1</td>
    <td>&lt;1</td>
    <td>18</td>
    <td>9</td>
  </tr>
</table>
<br  />
### Conclusions

Given the difference in specint rates it can be concluded that there is
little difference between AWS(virtualized) and SL(bare metal)
