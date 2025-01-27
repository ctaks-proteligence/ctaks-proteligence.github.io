# Google Docs Test Page

## this is the google doc example page exported into markdown
**just seeing what it looks like**

The purpose of this document is to show how the proteligence wiki can be shown in google docs.

Steps that were taken to format this document:

- Font: Merriweather   
- Pageless format  
  - Format \-\> switch to pageless  
- “Full” textwidth, which makes the margins the width of the screen  
  - View \-\> text width \-\> Full  
- Change hyperlinks to move within the document  
  - Right click \-\> edit link \-\> Headings and Bookmarks (at the bottom) \-\> click the corresponding one  
- Tables are simply copy pasted in and edited from there  
  - The table pastes with invisible borders.  
  - Click on any cell in the table and then click “table options” on the top right of the screen  
  - At the bottom, do Color \-\> Table border \-\> 1pt  
  - Highlight the entire table and unselect the “minimum row height” checkbox  
  - Per column, adjust the column width to a desired state. Sadly the table does not like to format width based on longest text in a column like it does on mediawiki so to make it look nice you have to do it manually. It looks like it does to an extent, actually, but it stops formatting itself after it goes beyond the default/regular page size. But we’re trying to make it as wide as our screen, so yeah. I consider this a dumb oversight/bug on the google doc devs part.

More tweaking can obviously be done, such as a smaller font size (mediawiki looks to have a bit smaller of a font), among other things.

Google docs has a sidebar on the left that works as an automated table of contents.

Below is just copy-pasted examples from certain pages from mediawiki

# **Usage**

## **Common**

These are fields or sets of fields referenced by the base message classes or by specific messages.

## **Message Classes**

These are the base message formats upon which all specific messages are based.

## **Specific Messages**

These are messages of a specific type (e.g., ADJUST) and subtype (e.g., Q). For each specific message, the type value and base class for the message's Message and messageDetail definitions are shown. The Message extended class shows the basic class used for the specific message while the messageDetail extended class shows the base messageDetail contents for the specific message. If additional tags are also listed, those fields are in addition to the base message detail fields.

# **Common**

## **address** {#address}

| Tag 1 | Data Type | Length | minOccurs | maxOccurs | Comments |
| ----- | ----- | ----- | ----- | ----- | ----- |
| streetAddress | AlphaNumericSpecialWithSpace |  | 0 |  |  |
| municipalityOrCity | AlphaNumericSpecialWithSpace |  | 0 |  |  |
| stateOrProvince | string |  | 0 |  |  |
| postalCode | string |  | 0 |  |  |
| country | string |  | 0 |  |  |

## **admissionInformation**

| Tag 1 | Data Type | Length | minOccurs | maxOccurs | Comments |
| ----- | ----- | ----- | ----- | ----- | ----- |
| providerName | AlphaNumericSpecialWithSpace | 41 |  |  |  |
| city | AlphaNumericSpecialWithSpace |  |  |  |  |
| state | string |  | 0 |  |  |
| country | string |  |  |  |  |
| admissionDate | date |  | 0 |  |  |
| estimatedDischargeDate | string |  | 0 |  | Schema says string but title is date? |
| admittingDiagnosisDescription | AlphaNumericSpecialWithSpace | 255 | 0 |  |  |
| emergencyDiagnosis | string | 1 | 0 | 1 |  |
| procedureorSurgeryDescription | AlphaNumericSpecialWithSpace | 255 | 0 |  |  |
| surgeryDate | string |  | 0 |  | Schema says string but title is date? |
| accidentDate | date |  | 0 |  |  |
| reasonInCountry | AlphaNumericSpecialWithSpace | 255 | 0 |  |  |
| jciAccredited | string | 1 | 0 |  |  |
| anticipatedAdmDate | date |  | 0 |  |  |
| procedureorServiceDescription | AlphaNumericSpecialWithSpace | 255 | 0 |  |  |
| procedureorServiceDate | string |  | 0 |  |  |
| diagnosisOrIllnessOrInjuryDescription | AlphaNumericSpecialWithSpace | 255 | 0 |  |  |
| illnessOrAccidentOrPregnancyDate | date |  | 0 |  |  |

## **airTransportationDetails**

| Tag 1 | Data Type | Length | minOccurs | maxOccurs | Comments |
| ----- | ----- | ----- | ----- | ----- | ----- |
| isAirTransportCovered | string | 1 | 0 |  |  |
| isMimMedNecessAirTransportCovered | string | 1 | 0 |  |  |
| isAirTransportCoveredForCompanion | string | 1 | 0 |  |  |
| typesOfTicketsCovered | string | 21 | 0 |  |  |
| isTransportCoveredForCompanion | string | 1 | 0 |  |  |
| airTransportComments1 | string | 255 | 0 |  |  |
| areChangeFeeCovered | string | 1 | 0 |  |  |
| addlDetailsOnChangeFeeCoverage | string | 255 | 0 |  |  |
| areLuggageFeeCovered | string | 1 | 0 |  |  |
| addlDetailsOnLuggageFeeCoverage | string | 255 | 0 |  |  |
| approvedTicketApplyToCompanion | string | 1 | 0 |  |  |
| airTransportComments2 | string | 255 | 0 |  |  |
| addlDetailsOnAirTransport | string | 255 | 0 |  |  |

## **benefitInformation**

| Tag 1 | Tag 2 | Data Type | Length | minOccurs | maxOccurs | Comments |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| paymentAcceptedOnPreClaim |  | string | 1 | 0 |  |  |
| guaranteeingPaymentOnPreClaim |  | string | 1 | 0 |  |  |
| medRecReqToAdjudicateClaim |  | string | 1 | 0 |  |  |
| isPolicyActive |  | string | 1 | 0 |  |  |
| policyEffectiveDate |  | date |  | 0 |  |  |
| terminationDate |  | string |  | 0 |  | Schema says string but title is date? |
| isPatientCoveredMember |  | string | 1 | 0 |  |  |
| isHomePlanCommitToPayAsPayer |  | string | 1 | 0 |  |  |
| isIntlAdmCovered |  | string | 1 | 0 |  |  |
| isAboveProcCoveredForMedTravel |  | string | 1 | 0 |  |  |
| doesMemberHaveMedTravelBenefits |  | string | 1 | 0 |  |  |
| jciAccreditedFacilities |  | string | 36 | 0 |  |  |
| daysCoveredAllowedForAdm |  | string |  | 0 |  |  |
| benefitsAvailableList |  |  |  | 0 |  |  |
|  | benefitsAvailable | string | 17 | 0 | unbounded |  |
| isHomePlanCommitingToPayProfFee |  | string | 36 | 0 |  |  |
| ifProfFeePaidDiffLevelDesc |  | string | 255 | 0 |  |  |
| specificExclusions |  | string | 255 | 0 |  |  |
| additionalNotes |  | string | 512 | 0 |  |  |
| preCertificationReq |  | string | 1 | 0 |  |  |
| phoneNumberPrecert |  | string | 10 | 0 |  |  |
| preCertificationApprovalName |  | AlphaNumericSpecialWithSpace | 40 | 0 | 1 |  |
| precertDate |  | date |  | 0 | 1 |  |
| doesHomeReqPlanInvolvedInCandidencyCons |  | string | 1 | 0 |  |  |
| phoneNumberIfDiffFromPrecert |  | string | 10 | 0 |  |  |
| otherCaseMngtReqForMedicalTravel |  | string | 255 | 0 |  |  |
| memberOrUtilizationReviewPenalty |  | GopBlue2Decimal |  | 0 |  |  |
| memberReviewPenaltySelect |  | string | 12 | 0 |  |  |
| ifHomePlanWantsRegUpdates |  | string | 1 | 0 |  |  |
| updatesPhoneNumber |  | string | 10 | 0 |  |  |
| updatesEmail |  | string |  | 0 |  |  |
| remaingDeductible |  | GopBlue2Decimal |  | 0 |  |  |
| remainingDeductibleSelect |  | string | 15 | 0 |  |  |
| copayment |  | GopBlue2Decimal |  | 0 |  |  |
| copaymentSelect |  | string | 15 | 0 |  |  |
| coinsurance |  | GopBlue2Decimal |  | 0 |  |  |
| rateBeforeOutOfPocketMaxIsMet |  | GopBlue2Decimal |  | 0 |  |  |
| rateOutOfPocketMax |  | GopBlue2Decimal |  | 0 |  |  |
| rateAfterOutOfPocketMaxIsMet |  | GopBlue2Decimal |  | 0 |  |  |
| outOfPockeMaxInclude |  | string | 1 | 0 |  |  |
| outOfPockeMaxIncludeDeduct |  | boolean |  | 0 |  |  |
| outOfPockeMaxIncludeCopay |  | boolean |  | 0 |  |  |
| remainingAmount |  | GopBlue2Decimal |  | 0 |  |  |

## **claimDetail**

| Tag 1 | Tag 2 | Data Type | Length | minOccurs | maxOccurs | Comments |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| hostHomeCode |  | string | 1 | 0 |  |  |
| serviceDateRange |  |  |  | 0 |  |  |
|  | fromDate | date |  |  |  |  |
|  | toDate | date |  |  |  |  |
| [Subscriber](#wiki-test) (goes back to the top just as an example) |  | Common |  | 0 |  |  |
| patient |  |  |  |  |  |  |
|  | firstName | AlphaNumericSpecialWithSpace | 20 | 0 |  |  |
|  | middleInitial | AlphaNumericSpecialWithSpace | 1 | 0 |  |  |
|  | lastName | AlphaNumericSpecialWithSpace | 20 | 0 |  |  |
|  | gender | string | 1 | 0 |  |  |
|  | dateOfBirth | date |  | 0 |  |  |
|  | relationshipWithSubscriber | string | 2 | 0 |  |  |
| [Provider](https://wiki.proteligence.com/view/Payloads#provider) (unedited, just links to the mediawiki website) |  | Common |  |  |  |  |
| processingSiteControlNumber |  | AlphaNumericSpecialWithSpace | 17 | 0 |  |  |
| hostPlanControlNumber |  | AlphaNumericSpecialWithSpace | 17 | 0 |  |  |
| originPlanCode |  | string | 3 | 0 |  |  |
| originStationCode |  | string | 4 | 0 |  |  |
| overlappingDestination |  |  |  | 0 | 4 |  |
|  | destinationPlanCode | string | 3 | 1 | 1 |  |
|  | destinationStationCode | string | 4 | 1 | 1 |  |
| destinationPlanCode |  | string | 3 | 0 |  |  |
| destinationStationCode |  | string | 4 | 0 |  |  |
| generatedSccf |  | string | 17 | 0 |  |  |
| medicalRecordNumber |  | string |  | 0 |  |  |

## **contact**

| Tag 1 | Data Type | Length | minOccurs | maxOccurs | Comments |
| ----- | ----- | ----- | ----- | ----- | ----- |
| name | AlphaNumericSpecialWithSpace |  | 0 |  |  |
| lastName | AlphaNumericSpecialWithSpace |  | 0 |  |  |
| firstName | AlphaNumericSpecialWithSpace |  | 0 |  |  |
| middleInitial | string |  | 0 |  |  |
| [address](#address) | Common |  | 0 |  |  |
| phone | string | 10 | 0 |  |  |
| extension | string | 4 | 0 |  |  |
| fax | string | 10 | 0 |  |  |
| email | string |  | 0 |  |  |

## **groundTransportationDetails**

| Tag 1 | Data Type | Length | minOccurs | maxOccurs | Comments |
| ----- | ----- | ----- | ----- | ----- | ----- |
| isGroundTransportCovered | string | 1 | 0 |  |  |
| isMimMedNecessTransportCovered | string | 1 | 0 |  |  |
| isHomeToAirportTransportCovered | string | 1 | 0 |  |  |
| isAirportToHotelTransportCovered | string | 1 | 0 |  |  |
| isTransportCoveredForCompanion | string | 1 | 0 |  |  |
| groundTransportComments | string | 255 | 0 |  |  |
| isTransportCoveredForRehabilitation | string | 1 | 0 |  |  |
| isTransportCoveredForLeisure | string | 1 | 0 |  |  |
| addlDetailsOnGroundTransport | string | 255 | 0 |  |  |

## **hotelOrLodgingDetails**

| Tag 1 | Data Type | Length | minOccurs | maxOccurs | Comments |
| ----- | ----- | ----- | ----- | ----- | ----- |
| areHotelCovered | string | 1 | 0 |  |  |
| hotelCoveredForComapnion | string | 1 | 0 |  |  |
| hotelComments | string | 255 | 0 |  |  |
| specifyRestricOnAccomCovered | string | 255 | 0 |  |  |
| isRoomServiceCovered | string | 1 | 0 |  |  |
| anyNonCoveredCharges | string | 255 | 0 |  |  |

## **message**

| Tag 1 | Tag 2 | Tag 3 | Tag 4 | Data Type | Length | minOccurs | maxOccurs | Comments |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| key |  |  |  |  |  | 0 |  |  |
|  | messageId |  |  | string | 32 |  |  |  |
| partitionKeyNumber |  |  |  | integer |  | 0 |  |  |
| sourceNode |  |  |  |  |  | 0 |  |  |
|  | nodeId |  |  | string | 32 |  |  |  |
|  | planCode |  |  | string | 3 | 0 |  |  |
|  | stationCode |  |  | string | 4 | 0 |  |  |
| destinationNode |  |  |  |  |  |  |  |  |
|  | nodeId |  |  | string | 32 |  |  |  |
|  | planCode |  |  | string | 3 | 0 |  |  |
|  | stationCode |  |  | string | 4 | 0 |  |  |
| messageDetail |  |  |  |  |  | 0 |  | Depends on message |
| parentMessageId |  |  |  | string | 32 | 0 |  |  |
| messageType |  |  |  | string | 8 |  |  |  |
| messageSubType |  |  |  | string | 1 |  |  |  |
| rootMessageId |  |  |  | string | 32 |  |  |  |
| remoteMessageId |  |  |  | string | 32 | 0 |  |  |
| remoteParentMessageId |  |  |  | string | 32 | 0 |  |  |
| remoteRootId |  |  |  | string | 32 | 0 |  |  |
| messageStates\* |  |  |  | string | 4 | 0 | unbounded | \*Per schema but not seen in sample file; check latest sample files when available. |
|  | key |  |  |  |  |  |  | Per schema but not seen in sample file; check latest sample files when available. |
|  |  | messageId |  | string | 32 |  |  | Per schema but not seen in sample file; check latest sample files when available. |
|  |  | stateTimeStamp |  | dateTime |  |  |  | Per schema but not seen in sample file; check latest sample files when available. |
|  | messageStateCode |  |  | string | 4 |  |  | Per schema but not seen in sample file; check latest sample files when available. |
|  | userId |  |  | string | 40 |  |  | Per schema but not seen in sample file; check latest sample files when available. |
|  | partitionKeyNumber |  |  | integer |  |  |  | Per schema but not seen in sample file; check latest sample files when available. |
|  | stateComment |  |  |  |  | 0 |  | Per schema but not seen in sample file; check latest sample files when available. |
|  |  | key |  |  |  |  |  | Per schema but not seen in sample file; check latest sample files when available. |
|  |  |  | messageId | string | 32 |  |  | Per schema but not seen in sample file; check latest sample files when available. |
|  |  |  | stateTimeStamp | dateTime |  |  |  | Per schema but not seen in sample file; check latest sample files when available. |
|  |  |  | internalMessage | string | 500 |  |  | Per schema but not seen in sample file; check latest sample files when available. |
|  |  |  | partitionKeyNumber | integer |  | 0 |  | Per schema but not seen in sample file; check latest sample files when available. |
|  |  |  | commentTypeCode | string | 1 | 0 |  | Per schema but not seen in sample file; check latest sample files when available. |
| childMessages\* |  |  |  |  |  | 0 | unbounded | \*Per schema but not seen in sample file; check latest sample files when available. |
|  | message |  |  |  |  |  |  | Per schema, additional child messages are here, but in practice, e.g., MRECORD messages look like separate messages with their own detail section, etc. I don't know if any child messages can actually show up as part of another message as schema indicates. Check latest sample files when available. |
| blue2ReleaseId |  |  |  | string |  | 0 |  |  |

##### **Timeline and Restrictions**

Only Path A counts towards this input. If the adjustment cancellation request and response are present (Path B), the input excludes the corresponding adjustment response from consideration.

This considers request-response pairs whether or not the request is closed.

The Cross-Reference SF is considered only if the adjustment request has Adj SF? \== Y.

Note: The only differences from Host Adjustments Processed \<= 7 Days are the perspective, Reason Codes, and age cut-off, as well as the optional presence of the Cross-Reference SF.

|  | Date Field |  | Order |  | Record | Restriction |  |
| ----- | ----- | :---- | ----- | ----- | ----- | ----- | ----- |
|  |  |  | **Path A** | **Path B** |  | **Field** | **Criteria** |
| Start | Date Created | (the later of those records present and considered) | 1 |  | ADJUST request | Direction | Received |
|  |  |  |  |  |  | Reason | \!= (209, 220, 222, 273, 274, 275, 276\) |
|  | Date Posted |  | 2 (optional) |  | SF (Cross-Reference) | SCCF ID | ADJUST request Cross-Reference SCCF ID |
|  |  |  |  | 3 | CNCLADJ request | (none) |  |
|  |  |  |  | 4 | CNCLADJ response | Action | 136 (Approved) |
|  |  |  |  |  |  | Date Created | \<= Interval end date |
| Finish | Date Created | (the earlier of those records present and considered) | 3 (optional) |  | ADJUST response | Direction | Sent |
|  |  |  | 4 |  | DF | Disposition | 4 (Void) |
|  |  |  |  |  |  | Streamline? | \!= Y |

|  | Date Field | Order | Record | Restriction |  |
| ----- | ----- | ----- | ----- | ----- | ----- |
|  |  |  |  | **Field** | **Criteria** |
| Start | Date Posted | 1 | SF | (none) |  |
| Finish | Date Created | 2 | DF | SCCF Suffix | 00 (Original) |
|  |  |  |  | Disposition | \!= 4 (not Void) |
|  |  |  |  | Adj Closeout DF? | \!= Y |

