# Installation_Close_Check history

<!-- This page has been generated to be viewed with Zensical, you can not view it just as markdown . Activate content tabs following the doc at https://zensical.org/docs/ -->

=== "Nov 24, 2026 (Initial)"

    _Nov 24, 2026, by mkyv in commit chore(monitoring): org state on 2026-11-24 21:16 for monitoring_orgfarm_a1d96f0d2b_dev_ed_develop [skip ci]_

    
    ## Flow Diagram [(_View History_)](Installation_Close_Check-history.md)
    
    ![diagram](./Installation_Close_Check-history-1.svg)
    
    <!-- Flow description -->
    
    ## General Information
    
    |<!-- -->|<!-- -->|
    |:---|:---|
    |Object|Installation__c|
    |Process Type|Auto Launched Flow|
    |Trigger Type|Record Before Save|
    |Record Trigger Type|Update|
    |Label|Installation Close Check|
    |Status|Active|
    |Does Require Record Changed To Meet Criteria|✅|
    |Description|An installation cannot be closed without an install date.|
    |Environments|Default|
    |Interview Label|Installation Close Check {!$Flow.CurrentDateTime}|
    |BuilderType (PM)|LightningFlowBuilder|
    |CanvasMode (PM)|AUTO_LAYOUT_CANVAS|
    |Connector|[No_Install_Date](#no_install_date)|
    |Next Node|[No_Install_Date](#no_install_date)|
    
    
    #### Filters (logic: **and**)
    
    |Filter Id|Field|Operator|Value|
    |:-- |:-- |:--:|:--: |
    |1|Status__c|Equal To|Completed|
    
    
    ## Flow Nodes Details
    
    ### Handover_is_not_Done
    
    |<!-- -->|<!-- -->|
    |:---|:---|
    |Type|Custom Error|
    |Label|Handover is not Done|
    |Custom Error Messages|errorMessage: Complete all the handover<br/>isFieldError: false<br/>|
    
    
    ### Install_Date_Missing
    
    |<!-- -->|<!-- -->|
    |:---|:---|
    |Type|Custom Error|
    |Label|Install Date Missing|
    |Description|Refuses to close an installation that was never given an install date.|
    |Custom Error Messages|errorMessage: Set the install date before closing this installation.<br/>isFieldError: false<br/>|
    
    
    ### handover_item_is_pending
    
    |<!-- -->|<!-- -->|
    |:---|:---|
    |Type|Decision|
    |Label|handover item is pending|
    |Default Connector Label|Default Outcome|
    
    
    #### Rule yes (yes)
    
    |<!-- -->|<!-- -->|
    |:---|:---|
    |Connector|[Handover_is_not_Done](#handover_is_not_done)|
    |Condition Logic|and|
    
    
    
    
    |Condition Id|Left Value Reference|Operator|Right Value|
    |:-- |:-- |:--:|:--: |
    |1|get_Hndover_items.Id|Is Null|False|
    
    
    
    
    ### No_Install_Date
    
    |<!-- -->|<!-- -->|
    |:---|:---|
    |Type|Decision|
    |Label|No Install Date|
    |Description|Was the installation ever given an install date?|
    |Default Connector|[get_Hndover_items](#get_hndover_items)|
    |Default Connector Label|Install date set|
    
    
    #### Rule Date_Missing (Date missing)
    
    |<!-- -->|<!-- -->|
    |:---|:---|
    |Connector|[Install_Date_Missing](#install_date_missing)|
    |Condition Logic|and|
    
    
    
    
    |Condition Id|Left Value Reference|Operator|Right Value|
    |:-- |:-- |:--:|:--: |
    |1|$Record.Install_Date__c|Is Null|True|
    
    
    
    
    ### get_Hndover_items
    
    |<!-- -->|<!-- -->|
    |:---|:---|
    |Type|Record Lookup|
    |Object|Handover_Item__c|
    |Label|get Hndover items|
    |Assign Null Values If No Records Found|⬜|
    |Get First Record Only|✅|
    |Store Output Automatically|✅|
    |Connector|[handover_item_is_pending](#handover_item_is_pending)|
    
    
    #### Filters (logic: **and**)
    
    |Filter Id|Field|Operator|Value|
    |:-- |:-- |:--:|:--: |
    |1|Installation__c|Equal To|$Record.Id|
    |2|Is_Done__c|Equal To|⬜|
    
    
    
    
    
    
    
    
    ___
    
    _Documentation generated from branch monitoring_orgfarm_a1d96f0d2b_dev_ed_develop by [sfdx-hardis](https://sfdx-hardis.cloudity.com), featuring [salesforce-flow-visualiser](https://github.com/toddhalfpenny/salesforce-flow-visualiser)_

