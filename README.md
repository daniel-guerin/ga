erDiagram
%% --- Salesforce Object Relationships & Data Model ---
%% This diagram shows key standard fields alongside custom fields for context.
%% - It does not include _all_ standard fields available on these objects.
%% - Field Requiredness (mandatory fields) is NOT indicated here but is important.
%% - Lookups: Standard lookups end in 'Id' (e.g., AccountId), Custom lookups end in '**c' (e.g., Account**c).

    ACCOUNT ||--o{ CONTACT : "Has (Std Relationship)"
    ACCOUNT ||--o{ Enquiry_Database__c : "Related To (Custom Lookup)"
    CONTACT ||--o{ Enquiry_Database__c : "Related To (Custom Lookup)"
    CONTACT ||--o{ TASK : "Related To (Std WhoId Lookup)"
    Enquiry_Database__c ||--o{ TASK : "Related To (Std WhatId Lookup)"

    %% --- Account Object (Standard) ---
    ACCOUNT {
        string Id "(Std) Salesforce Record ID"
        string OwnerId "(Std) Owning User/Queue ID"
        datetime CreatedDate "(Std) Record Creation Timestamp"
        datetime LastModifiedDate "(Std) Record Last Update Timestamp"
        string Name "(Std) <- addressall_db.company_name"
        string BillingStreet "(Std) <- addressall_db.address1 + address2"
        string BillingCity "(Std) <- addressall_db.town"
        string BillingState "(Std) <- addressall_db.county"
        string BillingPostalCode "(Std) <- addressall_db.postcode"
        string BillingCountry "(Std) <- addressall_db.country"
        string Phone "(Std) <- addressall_db.tel"
        string Fax "(Std) <- addressall_db.fax"
        string Industry "(Std) <- addressall_db.natureofbusiness (MAPPED Picklist)"
        string Type "(Std) <- addressall_db.typeoforganisation (MAPPED Picklist)"
        int NumberOfEmployees "(Std) <- addressall_db.numberatpractice (TRANSFORMED Varchar->Number)"
        string Legacy_Company_ID__c "(Custom) (EXTERNAL ID, UNIQUE - Generated from Name)"
    }

    %% --- Contact Object (Standard) ---
    CONTACT {
        string Id "(Std) Salesforce Record ID"
        string OwnerId "(Std) Owning User/Queue ID"
        datetime CreatedDate "(Std) Record Creation Timestamp"
        datetime LastModifiedDate "(Std) Record Last Update Timestamp"
        string Salutation "(Std) <- addressall_db.title (MAPPED Picklist)"
        string FirstName "(Std) <- addressall_db.forename"
        string LastName "(Std) <- addressall_db.surname"
        string Email "(Std) <- addressall_db.email"
        string Phone "(Std) <- addressall_db.tel"
        string MobilePhone "(Std) <- addressall_db.mob"
        string Fax "(Std) <- addressall_db.fax"
        string Title "(Std) <- addressall_db.jobTitle"
        lookup AccountId "(Std) --> Links to ACCOUNT via Legacy_Company_ID__c"
        bool GDPR_Consent__c "(Custom) <- addressall_db.gdprcheck (TRANSFORMED String->Checkbox)"
        string Legacy_Contact_ID__c "(Custom) (EXTERNAL ID, UNIQUE - Generated from Name+AccountRef)"
    }

    %% --- Enquiry Database Object (Custom) ---
    Enquiry_Database__c {
        string Id "(System - Custom Obj) Salesforce Record ID"
        string OwnerId "(System - Custom Obj) Owning User/Queue ID"
        datetime CreatedDate "(System - Custom Obj) Record Creation Timestamp"
        datetime LastModifiedDate "(System - Custom Obj) Record Last Update Timestamp"
        string Name "(System - Custom Obj) (AutoNumber Recommended, e.g., ENQ-{0000}, Exact formula TBD)"
        string Legacy_ID__c "(Custom) <- addressall_db.input_id (EXTERNAL ID)"
        date Entry_Date__c "(Custom) <- addressall_db.entry_date"
        bool Printed__c "(Custom) <- addressall_db.printed (TRANSFORMED String->Checkbox)"
        string QID__c "(Custom) <- addressall_db.qid"
        string Contact_Method__c "(Custom) <- addressall_db.contactby (MAPPED Picklist)"
        string Lead_Source__c "(Custom) <- addressall_db.lead (MAPPED Picklist)"
        string Enquiry_Category__c "(Custom) <- addressall_db.enquirycategory (MAPPED Picklist)"
        string Application__c "(Custom) <- addressall_db.application (MAPPED Picklist)"
        string Project__c "(Custom) <- addressall_db.project"
        string Tonnes_Involved__c "(Custom) <- addressall_db.tonnesinvolve"
        string Taken_By__c "(Custom) <- addressall_db.takenby"
        bool Paper_File_Created__c "(Custom) <- addressall_db.paperfilecreated (TRANSFORMED String->Checkbox)"
        bool Publications_Sent__c "(Custom) <- addressall_db.publications (TRANSFORMED String->Checkbox)"
        bool Presentation_Prospect__c "(Custom) <- addressall_db.presentationprospect (TRANSFORMED String->Checkbox)"
        bool Awards_Prospect__c "(Custom) <- addressall_db.awardsprospect (TRANSFORMED String->Checkbox)"
        bool GA_Followup_Required__c "(Custom) <- addressall_db.GAfollowup (TRANSFORMED String->Checkbox)"
        bool GA_Followup_Completed__c "(Custom) <- addressall_db.GAfollowupcompleated (TRANSFORMED String->Checkbox)"
        bool GP_Followup_Required__c "(Custom) <- addressall_db.GPfollowup (TRANSFORMED String->Checkbox)"
        bool GP_Followup_Completed__c "(Custom) <- addressall_db.GPfollowupcompleated (TRANSFORMED String->Checkbox)"
        string CPD_ID__c "(Custom) <- addressall_db.cpdid"
        string CPD_Details__c "(Custom) <- addressall_db.cpd_details (LONGTEXT)"
        bool Presentation_Completed__c "(Custom) <- addressall_db.presentationcompleted (TRANSFORMED String->Checkbox)"
        bool Meeting_Held__c "(Custom) <- addressall_db.meeting (TRANSFORMED String->Checkbox)"
        bool Technical_Advice__c "(Custom) <- addressall_db.techadvice (TRANSFORMED String->Checkbox)"
        bool Action_Required__c "(Custom) <- addressall_db.actionreqired (TRANSFORMED String->Checkbox)"
        string Projects__c "(Custom) <- addressall_db.projects (LONGTEXT)"
        string Mailing_List_ID__c "(Custom) <- addressall_db.mlid"
        string Region__c "(Custom) <- addressall_db.region (MAPPED Picklist)"
        string Reader_Survey__c "(Custom) <- addressall_db.ReaderSurvey"
        string Customer_ID__c "(Custom) <- addressall_db.cid"
        string Where_Contact_Made__c "(Custom) <- addressall_db.wherecontactmade (LONGTEXT)"
        string Additional_Contact_Info__c "(Custom) <- addressall_db.contact_details (LONGTEXT)"
        string System_ID__c "(Custom) <- addressall_db.sid"
        string Publications_To_Send__c "(Custom) <- addressall_db.publicationstosend (LONGTEXT)"
        bool Award_Entry__c "(Custom) <- addressall_db.awardentry (TRANSFORMED String->Checkbox)"
        bool Award_Win__c "(Custom) <- addressall_db.awardwin (TRANSFORMED String->Checkbox)"
        string Award_Year__c "(Custom) <- addressall_db.awardyr"
        string Award_Category__c "(Custom) <- addressall_db.awardcat"
        date Last_Updated_Date__c "(Custom) <- addressall_db.dateUpdated"
        lookup Account__c "(Custom) --> Links to ACCOUNT via Legacy_Company_ID__c"
        lookup Contact__c "(Custom) --> Links to CONTACT via Legacy_Contact_ID__c"
    }

    %% --- Task Object (Standard) ---
    TASK {
        string Id "(Std) Salesforce Record ID"
        string OwnerId "(Std) Owning User/Queue ID"
        datetime CreatedDate "(Std) Record Creation Timestamp"
        datetime LastModifiedDate "(Std) Record Last Update Timestamp"
        string Subject "(Std) (Generated, e.g., 'Enquiry Activity: ' + Enquiry_Name, Exact formula TBD)"
        string Type "(Std) <- activitytypes.activitytypename (MAPPED Picklist)"
        string Status "(Std) (Default or Set by Automation)"
        string Priority "(Std) (Default or Set by Automation)"
        date ActivityDate "(Std) Due Date (Default or Set by Automation)"
        lookup WhoId "(Std) --> Links to CONTACT via Legacy_Contact_ID__c"
        lookup WhatId "(Std) --> Links to Enquiry_Database__c via Legacy_ID__c"
        string Activity_Group__c "(Custom) <- activitytypes.activitygroup (MAPPED Picklist)"
    }
