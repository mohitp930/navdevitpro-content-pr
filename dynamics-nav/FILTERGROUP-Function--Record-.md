---
title: "FILTERGROUP Function (Record)"
description: FILTERGROUP Function (Record)
ms.custom: na
ms.date: 06/05/2016
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.prod: "dynamics-nav-2018"
ms.assetid: e3deefda-9561-45fb-abd6-f6ca0fa19602
caps.latest.revision: 25
manager: edupont
---
# FILTERGROUP Function (Record)
Gets or sets the filter group that is applied to a table.  
  
## Syntax  
  
```  
  
[CurrGroup] := Record.FILTERGROUP([NewGroup])  
```  
  
#### Parameters  
 *Record*  
 Type: Record  
  
 The record from the table for which the filter group will be set or returned.  
  
 *NewGroup*  
 Type: Integer  
  
 The number of the filter group to set.  
  
## Property Value/Return Value  
 Type: Integer  
  
 The number of the current filter group.  
  
## Remarks  
 A filter group can contain a filter for a Record that has been set earlier with the [SETFILTER Function \(Record\)](SETFILTER-Function--Record-.md) or the [SETRANGE Function \(Record\)](SETRANGE-Function--Record-.md). The total filter applied is the combination of all the filters set in all the filtergroups.  
  
 When you select a filter group, subsequent filter settings by the [SETFILTER Function \(Record\)](SETFILTER-Function--Record-.md) or the [SETRANGE Function \(Record\)](SETRANGE-Function--Record-.md) apply to that group.  
  
 All groups are active at all times. The only way to disable a group is to remove the filters set in that group.  
  
 Filters in different groups are all effective simultaneously. For example, if in one group, a filter is set on customer numbers 1000 to 2000, while in another group, a filter is set on customer numbers 1800 to 3000, then only numbers in the range 1800 to 2000 are visible.  
  
 [!INCLUDE[navnow](includes/navnow_md.md)] uses the following filter groups internally.  
  
|Number|Name|Description|  
|------------|----------|-----------------|  
|-1|Cross-column|Used to support the cross-column search.|  
|0|Std|The default group where filters are placed when no other group has been selected explicitly. This group is used for filters that can be set from the filter dialogs by the end user.|  
|1|Global|Used for filters that apply globally to the entire application.|  
|2|Form|Used for the filtering actions that result from the following:<br /><br /> -   [SETTABLEVIEW Function \(Page, Report, XMLport\)](SETTABLEVIEW-Function--Page--Report--XMLport-.md)<br />-   [SourceTableView Property](SourceTableView-Property.md)<br />-   [DataItemTableView Property](DataItemTableView-Property.md).|  
|3|Exec|Used for the filtering actions that result from the following:<br /><br /> -   [SubPageView Property](SubPageView-Property.md)<br />-   [RunPageView Property](RunPageView-Property.md)|  
|4|Link|Used for the filtering actions that result from the following:<br /><br /> -   [DataItemLink Property \(Reports\)](DataItemLink-Property--Reports-.md)<br />-   [SubPageLink Property](SubPageLink-Property.md)|  
|5|Temp|Not currently used.|  
|6|Security|Used for applying security filters for user permissions.|  
|7|Factboxes|Used for clearing the state of factboxes.|  
  
 A filter set in a group different from filter group 0 cannot be changed by a user that uses a filter dialog to set a filter. If, for example, a filter has been set on customer numbers 1000 to 2000 in group 4, then the user can set a filter that delimits this selection further, but cannot widen it to include customer numbers outside the range 1000 to 2000.  
  
 It is possible to use one of the internally used groups from C/AL. If you do this, you replace the filter that [!INCLUDE[navnow](includes/navnow_md.md)] assumes is in this group. If, for example, you use filter group 4 in a page, you will replace the filtering that is actually the result of applying the [SubPageLink Property](SubPageLink-Property.md). This could seriously alter the way pages and subpages interact.  
  
 Using filter group 7 may cause factboxes to not work as intended.  
  
 To reset the filters in filter group 1, you add an empty filter to the group. To add an empty filter, to filter group 1, you must first set the filter group.  
  
```  
Rec.FILTERGROUP(1);  
```  
  
 Then, for each field in the table that to which the Rec variable refers, set an empty filter.  
  
```  
Rec.SETFILTER(<field>,’’);  
```  
  
## Example 1 
 The following example uses the [SETFILTER Function \(Record\)](SETFILTER-Function--Record-.md) to set a filter that selects records with No. field between 10000 and 20000. Then the **FILTERGROUP** function returns the number for the filter group. No filter group was selected explicitly so the filter is set in filter group 0. This value is stored in the varOrigGroup variable and displayed in a message box. Next, the **FILTERGROUP** function changes the filter group to 1. The new value is stored in the varCurrGroup variable and displayed in a message box.  
  
 This example requires that you create the following variables and text constants in the **C/AL Globals** window.  
  
|Variable name|DataType|Subtype|  
|-------------------|--------------|-------------|  
|MyRecord|Record|Customer|  
|varOrigGroup|Integer|Not applicable|  
|varCurrGroup|Integer|Not applicable|  
  
|Text constant name|ConstValue|  
|------------------------|----------------|  
|Text000|The original filtergroup is: %1|  
|Text001|The current filtergroup is: %1|  
  
```  
MyRecord.SETFILTER("No.", '10000..20000');  
varOrigGroup := MyRecord.FILTERGROUP;  
MESSAGE(Text000, varOrigGroup);  
varCurrGroup := MyRecord.FILTERGROUP(1);  
MESSAGE(Text001, varCurrGroup);  
```  
  
## Example 2 
 The following example finds all customers where the Customer Name or Contact Name contains the string **John**.  
  
 This example requires that you create the following variable in the **C/AL Globals** window.  
  
|Variable Name|Datatype|  
|-------------------|--------------|  
|SearchString|Text|  
  
```  
Customer.FILTERGROUP := -1;  
SearchString := '@*John*';  
Customer.SETFILTER(Customer.Name, SearchString);  
Customer.SETFILTER(Customer.Contact, SearchString);  
```  
  
## See Also  
 [Record Data Type](Record-Data-Type.md)