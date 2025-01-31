## Example page

This is an example page. You can edit it or create a [new one](new_page.md)

#### table test 1
<table style="border-collapse: collapse; border: 1px solid black;">
  <thead>
    <tr>
      <th rowspan="2"></th>
      <th colspan="2" rowspan="2">Date Field</th>
      <th colspan="2">Order</th>
      <th rowspan="2">Record</th>
      <th colspan="2">Restriction</th>
    </tr>
    <tr>
      <th>Path A</th>
      <th>Path B</th>
      <th>Field</th>
      <th>Criteria</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="2">Start</td>
      <td rowspan="2">Date Created</td>
      <td rowspan="2">(the later of those records present and considered)</td>
      <td colspan="2" rowspan="2" align="center">1</td>
      <td rowspan="2">ADJUST request</td>
      <td>Direction</td>
      <td>Received</td>
    </tr>
    <tr>
      <td>Reason</td>
      <td>!= (209, 220, 222, 273, 274, 275)</td>
    </tr>
    <tr>
      <td colspan="3"></td>
      <td></td>
      <td align="center">2</td>
      <td>C request</td>
      <td colspan="2">(none)</td>
    </tr>
    <tr>
      <td colspan="3" rowspan="2"></td>
      <td rowspan="2"></td>
      <td rowspan="2" align="center">3</td>
      <td rowspan="2">C response</td>
      <td>Action</td>
      <td>136 (Approved)</td>
    </tr>
    <tr>
      <td>Date Created</td>
      <td>&lt;= Interval end date</td>
    </tr>
    <tr>
      <td rowspan="3">Finish</td>
      <td>Date Created</td>
      <td rowspan="3">(the earlier of those records present and considered)</td>
      <td align="center">2 (optional)</td>
      <td></td>
      <td>ADJUST response</td>
      <td>Direction</td>
      <td>Sent</td>
    </tr>
    <tr>
      <td rowspan="2">Date Posted</td>
      <td rowspan="2" align="center">3</td>
      <td rowspan="2"></td>
      <td rowspan="2">DF</td>
      <td>Disposition</td>
      <td>4 (Void)</td>
    </tr>
    <tr>
      <td>Streamline?</td>
      <td>!= Y</td>
    </tr>
  </tbody>
</table>

#### table test 2
<table style="border-collapse: collapse; border: 1px solid black;">
  <thead>
    <tr>
      <th rowspan="2" style="border: 1px solid black;"></th>
      <th colspan="2" rowspan="2" style="border: 1px solid black;">Date Field</th>
      <th colspan="2" style="border: 1px solid black;">Order</th>
      <th rowspan="2" style="border: 1px solid black;">Record</th>
      <th colspan="2" style="border: 1px solid black;">Restriction</th>
    </tr>
    <tr>
      <th style="border: 1px solid black;">Path A</th>
      <th style="border: 1px solid black;">Path B</th>
      <th style="border: 1px solid black;">Field</th>
      <th style="border: 1px solid black;">Criteria</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="2" style="border: 1px solid black;">Start</td>
      <td rowspan="2" style="border: 1px solid black;">Date Created</td>
      <td rowspan="2" style="border: 1px solid black;">(the later of those records present and considered)</td>
      <td colspan="2" rowspan="2" align="center" style="border: 1px solid black;">1</td>
      <td rowspan="2" style="border: 1px solid black;">ADJUST request</td>
      <td style="border: 1px solid black;">Direction</td>
      <td style="border: 1px solid black;">Received</td>
    </tr>
    <tr>
      <td style="border: 1px solid black;">Reason</td>
      <td style="border: 1px solid black;">!= (209, 220, 222, 273, 274, 275)</td>
    </tr>
    <tr>
      <td colspan="3" style="border: 1px solid black;"></td>
      <td style="border: 1px solid black;"></td>
      <td align="center" style="border: 1px solid black;">2</td>
      <td style="border: 1px solid black;">C request</td>
      <td colspan="2" style="border: 1px solid black;">(none)</td>
    </tr>
    <!-- Continue similarly for the rest of the table -->
  </tbody>
</table>

#### table test 3 (ai output)

<table class="wikitable">
  <tr>
    <th rowspan="2"></th>
    <th colspan="2" rowspan="2">D F</th>
    <th colspan="2">Order</th>
    <th rowspan="2">Record</th>
    <th colspan="2">Restriction</th>
  </tr>
  <tr>
    <th>P A</th>
    <th>P B</th>
    <th>F</th>
    <th>C</th>
  </tr>
  <tr>
    <td rowspan="2">Start</td>
    <td rowspan="2">Date Created</td>
    <td rowspan="2">(t)</td>
    <td colspan="2" rowspan="2" align="center">1</td>
    <td rowspan="2">A r</td>
    <td>Direction</td>
    <td>R</td>
  </tr>
  <tr>
    <td>Reason</td>
    <td>!= (2)</td>
  </tr>
  <tr>
    <td colspan="3"></td>
    <td align="center">2</td>
    <td>Ct</td>
    <td colspan="2">(none)</td>
  </tr>
  <tr>
    <td colspan="3" rowspan="2"></td>
    <td rowspan="2"></td>
    <td rowspan="2" align="center">3</td>
    <td rowspan="2">C r</td>
    <td>Action</td>
    <td>136 (Approved)</td>
  </tr>
  <tr>
    <td>Date C</td>
    <td>&lt;= I end date</td>
  </tr>
  <tr>
    <td rowspan="3">Finish</td>
    <td>D C</td>
    <td rowspan="3">(t)</td>
    <td align="center">2 (optional)</td>
    <td></td>
    <td>A r</td>
    <td>D</td>
    <td>Sent</td>
  </tr>
  <tr>
    <td rowspan="2">Date Posted</td>
    <td rowspan="2" align="center">3</td>
    <td rowspan="2"></td>
    <td rowspan="2">DF</td>
    <td>Disposition</td>
    <td>4 (Void)</td>
  </tr>
  <tr>
    <td>S?</td>
    <td>!= Y</td>
  </tr>
</table>

#### table test 4 (ai java code output)
<table border="1">
  <tr>
    <th rowspan="2"></th>
    <th rowspan="2" colspan="2">D F</th>
    <th colspan="2">Order</th>
    <th rowspan="2">Record</th>
    <th colspan="2">Restriction</th>
  <tr>
  <tr>
    <th>P A !! P B !! F !! C</th>
  <tr>
  <tr>
    <td rowspan="2">Start</td>
  <tr>
    <td rowspan="2">Date Created</td>
  <tr>
    <td rowspan="2">(t)</td>
  <tr>
    <td rowspan="2" colspan="2" align="center">1</td>
  <tr>
    <td rowspan="2">A r</td>
  <tr>
    <td>R</td>
  <tr>
  <tr>
    <td>!= (2)</td>
  <tr>
  <tr>
    <td colspan="3"></td>
  <tr>
    <td align="center">2</td>
  <tr>
    <td>Ct</td>
  <tr>
    <td colspan="2">(none)</td>
  <tr>
  <tr>
    <td rowspan="2" colspan="3"></td>
  <tr>
    <td rowspan="2"></td>
  <tr>
    <td rowspan="2" align="center">3</td>
  <tr>
    <td rowspan="2">C r</td>
  <tr>
    <td>136 (Approved)</td>
  <tr>
  <tr>
    <td><= I end date</td>
  <tr>
  <tr>
    <td rowspan="3">Finish</td>
  <tr>
    <td>D C</td>
  <tr>
    <td rowspan="3">(t)</td>
  <tr>
    <td align="center">2 (optional)</td>
  <tr>
    <td></td>
  <tr>
    <td>A r</td>
  <tr>
    <td>Sent</td>
  <tr>
  <tr>
    <td rowspan="2">Date Posted</td>
  <tr>
    <td rowspan="2" align="center">3</td>
  <tr>
    <td rowspan="2"></td>
  <tr>
    <td rowspan="2">DF</td>
  <tr>
    <td>4 (Void)</td>
  <tr>
  <tr>
    <td>!= Y</td>
</table>
