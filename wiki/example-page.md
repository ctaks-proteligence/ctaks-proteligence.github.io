## Example page

This is an example page. You can edit it or create a [new one](new_page.md)

{| class="wikitable"
! rowspan="2" |
! colspan="2" rowspan="2" | Date Field
! colspan="2" | Order
! rowspan="2" | Record
! colspan="2" | Restriction
|-
! Path A !! Path B !! Field !! Criteria
|-
| rowspan="2" | Start
| rowspan="2" | Date Created
| rowspan="2" | (the later of those records present and considered)
| colspan="2" rowspan="2" align="center" | 1
| rowspan="2" | ADJUST request
| Direction || Received
|-
| Reason || != (209, 220, 222, 273, 274, 275)
|-
| colspan="3" |
|
| align="center" | 2
| C request
| colspan="2" | (none)
|-
| colspan="3" rowspan="2" |
| rowspan="2" |
| rowspan="2" align="center" | 3
| rowspan="2" | C response
| Action || 136 (Approved)
|-
| Date Created || <= Interval end date
|-
| rowspan="3" | Finish
| Date Created
| rowspan="3" | (the earlier of those records present and considered)
| align="center" | 2 (optional)
|
| ADJUST response
| Direction || Sent
|-
| rowspan="2" | Date Posted
| rowspan="2" align="center" | 3
| rowspan="2" |
| rowspan="2" | DF
| Disposition || 4 (Void)
|-
| Streamline? || != Y
|}


<table>
    <tr>
        <td></td>
        <td>colspan=&quot;2&quot;  Date Field</td>
        <td>Order</td>
        <td>Record</td>
        <td>Restriction</td>
    </tr>
    <tr>
        <td>Path A</td>
        <td>Path B</td>
        <td>Field</td>
        <td>Criteria</td>
    </tr>
    <tr>
        <td>Start</td>
        <td>Date Created</td>
        <td>(the later of those records present and considered)</td>
        <td>colspan=&quot;2&quot; rowspan=&quot;2&quot; align=&quot;center&quot;</td>
        <td>1</td>
        <td>ADJUST request</td>
        <td>Direction</td>
        <td>Received</td>
    </tr>
    <tr>
        <td>Reason</td>
        <td></td>
        <td>= (209, 220, 222, 273, 274, 275, 276)</td>
    </tr>
    <tr>
        <td>Date Posted</td>
        <td>colspan=&quot;2&quot; align=&quot;center&quot;</td>
        <td>2 (optional)</td>
        <td>SF (Cross-Reference)</td>
        <td>SCCF ID</td>
        <td>ADJUST request Cross-Reference SCCF ID</td>
    </tr>
    <tr>
        <td></td>
        <td></td>
        <td>align=&quot;center&quot;</td>
        <td>3</td>
        <td>CNCLADJ request</td>
        <td>(none)</td>
    </tr>
    <tr>
        <td>colspan=&quot;3&quot;</td>
        <td></td>
        <td>rowspan=&quot;2&quot; align=&quot;center&quot;</td>
        <td>4</td>
        <td>CNCLADJ response</td>
        <td>Action</td>
        <td>136 (Approved)</td>
    </tr>
    <tr>
        <td>Date Created</td>
        <td>&lt;= Interval end date</td>
    </tr>
    <tr>
        <td>Finish</td>
        <td>Date Created</td>
        <td>(the earlier of those records present and considered)</td>
        <td>align=&quot;center&quot;</td>
        <td>3 (optional)</td>
        <td></td>
        <td>ADJUST response</td>
        <td>Direction</td>
        <td>Sent</td>
    </tr>
    <tr>
        <td>rowspan=&quot;2&quot; align=&quot;center&quot;</td>
        <td>4</td>
        <td></td>
        <td>DF</td>
        <td>Disposition</td>
        <td>4 (Void)</td>
    </tr>
    <tr>
        <td>Streamline?</td>
        <td></td>
        <td>= Y</td>
    </tr>
</table>
