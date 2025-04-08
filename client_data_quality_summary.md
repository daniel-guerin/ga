# Salesforce Data Migration: Data Quality Assessment Summary

## Executive Summary

**Purpose:** To ensure a successful migration of your valuable business data to Salesforce, we performed an assessment of the source data from the `addressall_db` database.

**Overall Finding:** The assessment reveals significant data quality challenges, including missing information, inconsistent formatting, and a high probability of duplicate company records. Addressing these issues _before_ migration is essential to ensure the data is accurate, reliable, and truly useful in Salesforce.

**Implication:** Successfully migrating clean and trustworthy data requires dedicated effort for data cleansing (fixing errors, standardizing values) and implementing an effective process to identify and merge duplicate records. This foundational work is crucial for the migration's success and achieving the full benefits of Salesforce.

---

## 1. Data Completeness: "What's Missing?"

**Why it Matters:** Incomplete records in Salesforce lead to wasted time, missed opportunities, and unreliable reporting. If critical information like addresses or contact details are missing, it directly hinders sales, marketing, and service effectiveness.

**Findings:** The analysis revealed varying levels of completeness across fields. Some are nearly full, while others are almost entirely empty. To clarify the migration scope, the fields have been categorized below based on their completeness and essential business value.

### Fields Recommended for Migration

These fields are considered essential for business operations, linking records, or contain valuable data despite potential sparseness. They should be included in the migration scope, though cleansing (see Section 2) may still be required for some.

```markdown
| Column               | Null/Empty Count | Percentage | Non-Empty Count | Notes/Rationale                                           |
| -------------------- | ---------------- | ---------- | --------------- | --------------------------------------------------------- |
| project              | 55942            | 99.88%     | 66              | Keep despite sparseness, project context may be valuable. |
| cpd_details          | 55816            | 99.66%     | 192             | Keep despite sparseness, related to CPD activity.         |
| tonnesinvolve        | 55771            | 99.58%     | 237             | Keep despite sparseness, project detail may be valuable.  |
| region               | 55657            | 99.37%     | 351             | Keep despite sparseness for location context. Needs cleansing. |
| numberatpractice     | 55413            | 98.94%     | 595             | Keep despite sparseness. NB: Contains text, not numbers.  |
| cpdid                | 54984            | 98.17%     | 1024            | Critical flag for CPD records. Keep despite sparseness.   |
| mob                  | 51441            | 91.85%     | 4567            | Keep despite sparseness, mobile contact detail. Needs formatting. |
| country              | 50429            | 90.04%     | 5579            | Keep despite sparseness for location context. Needs cleansing. |
| fax                  | 43592            | 77.83%     | 12416           | Contact detail, moderate completeness. Needs formatting review. |
| application          | 42550            | 75.97%     | 13458           | Enquiry detail, needs cleansing/standardization.          |
| email                | 38090            | 68.01%     | 17918           | Core contact detail.                                      |
| county               | 37832            | 67.55%     | 18176           | Address component, moderate completeness.                 |
| publicationstosend   | 32534            | 58.09%     | 23474           | Legacy communication preference.                          |
| tel                  | 25615            | 45.73%     | 30393           | Core contact detail. Needs formatting review.             |
| address2             | 20451            | 36.51%     | 35557           | Core address component.                                   |
| town                 | 19297            | 34.45%     | 36711           | Core address component.                                   |
| lead                 | 18608            | 33.22%     | 37400           | Enquiry detail, moderate completeness.                    |
| dateUpdated          | 18007            | 32.15%     | 37993           | Record update timestamp.                                  |
| postcode             | 17184            | 30.68%     | 38824           | Core address component (used for deduplication). Needs validation. |
| address1             | 15154            | 27.06%     | 40854           | Core address component.                                   |
| enquiry_details      | 14963            | 26.72%     | 41045           | Core enquiry information.                                 |
| actionreqired        | 13256            | 23.67%     | 42752           | Enquiry detail, moderate completeness.                    |
| enquirycategory      | 12654            | 22.59%     | 43354           | Enquiry detail, moderate completeness.                    |
| takenby              | 8219             | 14.67%     | 47789           | Enquiry detail, high completeness.                        |
| contactby            | 7756             | 13.85%     | 48252           | Enquiry detail, high completeness.                        |
| natureofbusiness     | 6392             | 11.41%     | 49616           | Company detail, high completeness.                        |
| publications         | 5708             | 10.19%     | 50300           | Legacy field, keep for historical context if needed.      |
| techadvice           | 5707             | 10.19%     | 50301           | Legacy field, keep for historical context if needed.      |
| meeting              | 5707             | 10.19%     | 50301           | Legacy field, keep for historical context if needed.      |
| presentationcompleted| 5707             | 10.19%     | 50301           | Legacy field, keep for historical context if needed.      |
| GPfollowupcompleated | 5707             | 10.19%     | 50301           | Legacy field, keep for historical context if needed.      |
| GPfollowup           | 5707             | 10.19%     | 50301           | Legacy field, keep for historical context if needed.      |
| GAfollowupcompleated | 5707             | 10.19%     | 50301           | Legacy field, keep for historical context if needed.      |
| GAfollowup           | 5707             | 10.19%     | 50301           | Legacy field, keep for historical context if needed.      |
| awardsprospect       | 5707             | 10.19%     | 50301           | Legacy field, keep for historical context if needed.      |
| presentationprospect | 5707             | 10.19%     | 50301           | Legacy field, keep for historical context if needed.      |
| paperfilecreated     | 5707             | 10.19%     | 50301           | Legacy field, keep for historical context if needed.      |
| printed              | 5707             | 10.19%     | 50301           | Legacy field, keep for historical context if needed.      |
| forename             | 2013             | 3.59%      | 53995           | Core Contact identifier.                                  |
| company_name         | 1951             | 3.48%      | 54057           | Core Account identifier (used for deduplication).         |
| surname              | 781              | 1.39%      | 55227           | Core Contact identifier.                                  |
| title                | 648              | 1.16%      | 55360           | Core Contact detail. Needs cleansing/standardization.     |
| entry_date           | 207              | 0.37%      | 55801           | Record creation timestamp.                                |
| gdprcheck            | 0                | 0.00%      | 56008           | Compliance flag, fully complete.                          |
| input_id             | 0                | 0.00%      | 56008           | Primary Key from source DB, used for Legacy_ID__c.        |
```

### Fields Recommended for Exclusion

These fields are recommended for exclusion from the migration scope primarily due to extremely high levels of missing data combined with low perceived business value or redundancy. Excluding them simplifies the migration process and avoids cluttering Salesforce with largely empty or unused fields.

```markdown
| Column             | Null/Empty Count | Percentage | Non-Empty Count | Notes/Rationale                                                    |
| ------------------ | ---------------- | ---------- | --------------- | ------------------------------------------------------------------ |
| awardwin           | 56008            | 100.00%    | 0               | 100% empty.                                                        |
| awardyr            | 56008            | 100.00%    | 0               | 100% empty.                                                        |
| awardcat           | 56008            | 100.00%    | 0               | 100% empty.                                                        |
| typeoforganisation | 56007            | 100.00%    | 1               | >99.99% empty, value "Edit List" uninformative. Low value.        |
| projects           | 56007            | 100.00%    | 1               | >99.99% empty (distinct from 'project' field). Low value.          |
| awardentry         | 56007            | 100.00%    | 1               | >99.99% empty, low value.                                          |
| jobTitle           | 56006            | 100.00%    | 2               | >99.99% empty, low value. Standard Salesforce Contact field exists. |
| cid                | 56006            | 100.00%    | 2               | >99.99% empty, likely legacy ID, purpose unclear, low value.         |
| contact_details    | 56003            | 99.99%     | 5               | >99.99% empty, low value.                                          |
| ReaderSurvey       | 56002            | 99.99%     | 6               | >99.99% empty, low value.                                          |
| wherecontactmade   | 55996            | 99.98%     | 12              | >99.9% empty, low value.                                           |
| mlid               | 48568            | 86.72%     | 7440            | High emptiness, confirmed no DB relationships, purpose unclear.      |
| sid                | 25856            | 46.16%     | 30152           | High emptiness, confirmed no DB relationships, purpose unclear.      |
| qid                | 14338            | 25.60%     | 41670           | Moderate emptiness, confirmed no DB relationships, purpose unclear. |
```
