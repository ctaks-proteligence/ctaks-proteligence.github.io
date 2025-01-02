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
