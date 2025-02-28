Protected Health Information (PHI) must be treated carefully and
securely. HIPAA requires certain standards be met. To meet these
standards, Datanet should not load any field that is considered PHI. In
addition, data received from Plans or the BCBSA for test purposes must
be de-identified.

Out of an abundance of caution, certain fields not strictly PHI are
nevertheless listed here; for example, free-form text fields may in
certain instances contain PHI.

The fields listed below are current through IPP Release 15.5.1.

# Loading

Datanet should not load any Protected Health Information. Because the
payload is the entire Blue^2 message, Datanet should not load the
payload itself, but only those values from the payload necessary to
fully provide existing functionality. The following fields are
considered PHI:

<table>
  <tr>
    <th>Title</th>
    <th>Available Since IPP Release</th>
  </tr>
  <tr>
    <td>Ambulance Drop off Location Address</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Ambulance Drop off Location Name</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Ambulance Drop-off City</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Ambulance Drop-off State</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Ambulance Drop-off Zip Code</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Ambulance Pick-up Address/Location</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Ambulance Pick-up Location City</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Ambulance Pick-up Location State</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Ambulance Pick-up Location Zip</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Bank Account Number</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Check/Voucher Number</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Corrected Priority Payer Name</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Narrative</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Other Carrier City</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Other Insured Last Name</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Other Payer Name</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Other Subscriber First Name</td>
    <td>15.5.1</td>
  </tr>
  <tr>
    <td>Other Subscriber Last Name</td>
    <td>15.5.1</td>
  </tr>
  <tr>
    <td>Other Subscriber Middle Name</td>
    <td>15.5.1</td>
  </tr>
  <tr>
    <td>Patient Address Line 1</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Patient Address Line 2</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Patient Birth Date</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Patient City</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Patient Control Number</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Patient First Name</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Patient Last Name</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Patient Marital Status Code</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Patient Middle Initial</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Patient Relationship to Subscriber - Standardized</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Patient Sex</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Patient State</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Patient Zip Code</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Performing Provider Office Zip Code - Claim</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Performing Provider Office Zip Code – Line</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Provider Address Line 1</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Provider Address Line 2</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Provider Country Code</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Provider Fax Number</td>
    <td>15.5.1</td>
  </tr>
  <tr>
    <td>Provider Office City</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Provider Office State</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Provider Office Zip Code</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Provider Telephone Number</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Representative Payee City</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Representative Payee Name</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Representative Payee State</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Representative Payee Street Address</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Representative Payee Zip Code</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Service Facility Location State - Claim</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Service Facility Location State - Line</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Service Facility Location Zip - Claim</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Service Facility Location Zip - Line</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Special Notations Data</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Subscriber Address Line 1</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Subscriber Address Line 2</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Subscriber Birth Date</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Subscriber City</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Subscriber First Name</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Subscriber Last Name</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Subscriber Middle Initial</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Subscriber Sex</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Subscriber State</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Subscriber Zip Code</td>
    <td>15.5.0</td>
  </tr>
</table>
<br  />
# De-identifying

De-identify all files received from Plans or the BCBSA before storing
them. This consists of clearing all fields listed above under Loading
and reducing the Blue^2 message payload to the minimum necessary to
fully provide existing functionality. In addition, for further safety,
also clear the following fields:

<table>
  <tr>
    <th>Title</th>
    <th>Available Since IPP Release</th>
  </tr>
  <tr>
    <td>BCBS Provider Number</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Benefit Management Treatment Authorization Number</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Billing Provider First Name</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Billing Provider Last Name</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Billing Provider Middle Initial</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Claim Submitter Organization Name</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Control Plan Authorization Number</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Control Plan Referral Number</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Federal Tax ID Number</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Federal Tax Sub ID</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Healthcare Policy Identification</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Local Plan Claim Reference Number</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Local Plan Control Number</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Local Plan Referral Number</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Medical Record Number</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Medicare Claim Number</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Medicare Provider Number</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Medicare Provider Number</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Other Insured Identification Number</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Other Subscriber Identification Number</td>
    <td>15.5.1</td>
  </tr>
  <tr>
    <td>Payer Plan Name</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Performing Provider First Name</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Performing Provider Last Name</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Performing Provider Middle Initial</td>
    <td>15.5.1</td>
  </tr>
  <tr>
    <td>Performing Provider Name - Claim</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Performing Provider NPI - Claim</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Performing Provider NPI - Line</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Performing Provider Number - Claim</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Performing Provider Number – Line</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Physician First Name</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Physician Identifier</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Physician Last Name</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Physician Middle Initial</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Predetermination of Benefits Identifier - Line</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Processing Site Control Number</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Provider Name</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Provider NPI</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Referring Provider First Name</td>
    <td>15.5.1</td>
  </tr>
  <tr>
    <td>Referring Provider Last Name</td>
    <td>15.5.1</td>
  </tr>
  <tr>
    <td>Referring Provider Middle Name</td>
    <td>15.5.1</td>
  </tr>
  <tr>
    <td>Referring Provider Name</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Referring Provider Number</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Service Facility Location Number - Claim</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Service Facility Location Number - Line</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Service Facility Name</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Service Facility Number</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Special Notations Data</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Subscriber Group Number</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Subscriber Identification Number - Alpha Prefix</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Subscriber Identification Number - Suffix</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Subscriber Identification Number Actual – Alpha Prefix</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Subscriber Identification Number Actual - Suffix</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Subscriber Identification Number Input – Alpha Prefix</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Subscriber Identification Number Input - Suffix</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Subscriber Middle Initial</td>
    <td>15.5.0</td>
  </tr>
  <tr>
    <td>Subscriber Sex</td>
    <td>15.5.0</td>
  </tr>
</table>
<br  />
These additional fields, while not PHI themselves, may provide the means
to statistically narrow down the possible candidates or, in the case of
identifiers, allow joining with another data set for identification.
