# Salesforce Data Migration: Data Quality Assessment Summary

## Executive Summary

**Purpose:** To ensure a successful migration of your valuable business data to Salesforce, we performed an assessment of the source data from the `addressall_db` database.

**Overall Finding:** The assessment reveals significant data quality challenges, including missing information, inconsistent formatting, and a high probability of duplicate company records. Addressing these issues _before_ migration is essential to ensure the data is accurate, reliable, and truly useful in Salesforce.

**Implication:** Successfully migrating clean and trustworthy data requires dedicated effort for data cleansing (fixing errors, standardizing values) and implementing an effective process to identify and merge duplicate records. This foundational work is crucial for the migration's success and achieving the full benefits of Salesforce.

---

## 1. Data Completeness: "What's Missing?"

**Why it Matters:** Incomplete records in Salesforce lead to wasted time, missed opportunities, and unreliable reporting. If critical information like addresses or contact details are missing, it directly hinders sales, marketing, and service effectiveness.

**Findings:** Many columns in the source data are largely empty or have questionable utility. The tables below separate fields recommended for migration from those recommended for exclusion, based on data completeness and perceived business value.

### Fields Recommended for Migration

These fields are considered essential for business operations, linking records, or contain valuable data despite potential sparseness. They should be included in the migration scope, though cleansing (see Section 2) may still be required for some.

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

### Fields Recommended for Exclusion

These fields are recommended for exclusion from the migration scope primarily due to extremely high levels of missing data combined with low perceived business value or redundancy. Excluding them simplifies the migration process and avoids cluttering Salesforce with largely empty or unused fields.

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

---

## 2. Data Consistency & Formatting: "Cleaning Up"

**Why it Matters:** Inconsistent data makes searching unreliable, leads to inaccurate reports, and creates a confusing experience for users trying to understand customer information. Standardizing data ensures everyone sees the same information in the same way.

**Findings & Examples:**

- **Titles:** Contact titles (`title`) are stored inconsistently.

  - _Examples:_ `Mr`, `MR`, `Mr.`, `Ms`, `Mrs`, `Miss`, `M`, `Dr` (and many others!).
  - _Benefit of Action:_ Standardizing these ensures consistent, professional communication and allows for accurate segmentation and reporting.

- **UK Postcodes:** A significant number of postcodes (`postcode`) are incomplete or incorrectly formatted.

  - _Examples:_ `SW1 E5BH` (incorrect spacing), `SE10 OQ1` (letter 'O' used instead of zero '0'), `RG13` (incomplete).
  - _Benefit of Action:_ Correcting these ensures mailings reach their destination and enables reliable location-based analysis and territory management.

- **Yes/No Fields (Checkboxes):** Fields intended to be simple Yes/No answers are stored in multiple ways.
  - _Examples:_ `presentationcompleted` contains `True`, `False`, `1`, and empty values. Others contain similar mixes.
  - _Benefit of Action:_ Converting these to a consistent `True`/`False` format ensures Salesforce features relying on these fields (like workflows or reports) function correctly.

---

## 3. Duplicate Data Challenge: "Seeing Double (or Triple!)"

**Why it Matters:** Duplicate company (Account) records are a major problem. They lead to:
_ **Inaccurate Reporting:** Skewed counts of customers or prospects, hindering accurate business analysis.
_ **Wasted Effort:** Sales or marketing might contact the same company multiple times unknowingly, wasting resources and potentially damaging relationships.
_ **Inconsistent View:** Different users might update different duplicate records, leading to a fragmented and unreliable understanding of your customers.
_ **Relationship Issues:** It becomes difficult to reliably link related information (like Contacts or Enquiries) to the _correct_ single Account, undermining the goal of a unified customer view.

**Findings:** Our initial analysis flagged approximately **7,000 potential groups** of duplicate company records, involving over 33,000 individual entries. This indicates a significant deduplication task is required. Manually identifying these is impractical due to the volume and inconsistencies.

**Examples of Why It's Difficult:**

- **Example 1: Minor Name & Address Typos**

  - Record A (`input_id: 14956`): `Fischer Fixings`, `Hidhercroft Road`, `OX10 9AT`
  - Record B (`input_id: 29853`): ` Fischer Fixings`, `Hithercroft Road`, `OX10 9AT`
  - _Issue:_ Very similar name (leading space) and slight address typo. Are they the same? Likely, but requires verification.

- **Example 2: Similar Name, Address Variations**

  - Record A (`input_id: 37567`): `3 E Consulting Engineers`, `4 Colder Close`, Wakefield, `WF4 3BA`
  - Record B (`input_id: 55960`): `3 E Consulting Engineers`, `4 Calder Close`, Wakefield, `WF4 3BA`
  - _Issue:_ Same company name and postcode, but a typo in the address line (`Colder` vs `Calder`). Again, likely the same entity.

- **Example 3: Company Name Variations & Multiple Locations**

  - Record A (`input_id: 4456`): `% BDP`, PO Box 4WD, LONDON, `W1A 4WD`
  - Record B (`input_id: 6229`): `B D P`, PO Box 85, Quay Street [Manchester], `M60 3JA`
  - Record C (`input_id: 14351`): `BDP`, 2 Bruce St, Belfast, `BT2 7JD`
  - Record D (`input_id: 46803`): `BDP`, 85-89 Colmore Row, Birmingham, `B3 2BB`
  - _Issue:_ Is this the same company with inconsistent naming (`% BDP`, `B D P`, `BDP`) across multiple branches, or different entities? Simple matching fails here; requires intelligent comparison and potentially manual review to confirm relationships.

- **Example 4: Missing Information**

  - Record A (`input_id: 62778`): `Acme Architects`, (No Address1), London, (No Postcode)
  - Record B (`input_id: 62788`): `Acme Architects`, `76 Tabernacle Street`, London, `EC2 A4EA`
  - _Issue:_ Same company name and town, but one record lacks the specific address and postcode. Are these the same London office, or different branches? Needs careful handling during deduplication.

- **Example 5: Multiple Incomplete Records**

  - Record A (`input_id: 13609`): `Adey Steel`, `Flacon Industrial Estate`, Loughborough, `LE11 1HL`
  - Record B (`input_id: 61259`): `Adey Steel`, (No Address1), Loughborough, (No Postcode)
  - Record C (`input_id: 15542`): `Adey Steel`, `Falcon Industrial Estate`, Loughborough, `LE11 1HL` (Note 'Falcon' vs 'Flacon')
  - _Issue:_ Multiple entries for likely the same company, with missing information and slight variations making simple matching unreliable.

- **Example 6: Different Contacts at Same Location**

  - Record A (`input_id: 14956`): ` Fischer Fixings`, `Hidhercroft Road`, Oxfordshire, `OX10 9AT`, Contact: Mirka Valovic, Tel: ...923
  - Record B (`input_id: 29853`): ` Fischer Fixings`, `Hithercroft Road`, Oxfordshire, `OX10 9AT`, Contact: David McLaren, Tel: ...920
  - _Issue:_ Likely the same company location (minor address/name variations), but entered as separate records for different contacts. These should be merged into a single Account record in Salesforce, with both individuals linked as Contacts.

**Required Action:** These examples highlight why simple checks fail. An automated, intelligent process is needed during migration. This process must compare records across multiple fields (name, address parts, postcode, phone), effectively handle variations and missing data, and confidently identify and merge duplicates. This is the only way to ensure each real-world company exists just once in Salesforce, providing a true single source of truth.

---

## 4. Recommendations & Next Steps

To ensure a successful migration and a reliable Salesforce system, we recommend the following:

1.  **Data Cleansing:** Allocate time to systematically clean the source data _before_ migration. This includes:
    - Standardizing values in fields like `title`, `country`, `application`, etc.
    - Correcting invalid postcode formats.
    - Converting boolean/checkbox fields to a consistent `True`/`False` format.
    - Addressing other identified inconsistencies.
    - _Note:_ This cleansing effort should also review the non-empty values within the sparse fields being kept (see point 2).
2.  **Targeted Field Migration:**
    - **Exclude** columns that are over 90% empty _and_ provide little business value (e.g., `awardwin`, `awardyr`, `awardcat`, `typeoforganisation`, `projects` [distinct from `project`], `awardentry`, `jobTitle`, `cid`, `contact_details`, `ReaderSurvey`, `wherecontactmade`). This simplifies the migration and avoids cluttering Salesforce.
    - **Include** specific fields despite >90% emptiness because their non-empty records contain valuable business context. These fields are: `project`, `cpd_details`, `tonnesinvolve`, `region`, `numberatpractice`, `cpdid` (critical for identifying CPD activity), `mob`, and `country`. Ensure these are part of the migration scope.
3.  **Implement Smart Deduplication:** Employ an automated deduplication process during the migration itself. This process will intelligently compare and merge duplicate Account (and potentially Contact) records based on multiple criteria, ensuring data accuracy.

This preparatory work is critical for data integrity and successful user adoption in Salesforce. Investing this effort now will result in a much cleaner, more reliable, and ultimately more valuable system for your business long-term.
