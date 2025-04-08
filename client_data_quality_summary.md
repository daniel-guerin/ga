# Salesforce Data Migration: Data Quality Assessment Summary

## Executive Summary

**Purpose:** To ensure a successful migration of your valuable business data to Salesforce, we performed an assessment of the source data from the `addressall_db` database.

**Overall Finding:** The assessment reveals significant data quality challenges, including missing information, inconsistent formatting, and a high probability of duplicate company records. Addressing these issues _before_ migration is essential to ensure the data is accurate, reliable, and truly useful in Salesforce.

**Implication:** Successfully migrating clean and trustworthy data requires dedicated effort for data cleansing (fixing errors, standardizing values) and implementing an effective process to identify and merge duplicate records. This foundational work is crucial for the migration's success and achieving the full benefits of Salesforce.

---

## 1. Data Completeness: "What's Missing?"

**Why it Matters:** Incomplete records in Salesforce lead to wasted time, missed opportunities, and unreliable reporting. If critical information like addresses or contact details are missing, it directly hinders sales, marketing, and service effectiveness.

**Findings:** Many columns in the source data are largely empty, rendering them unusable. The table below shows the percentage of missing or empty values for each column, sorted from most empty to least.

```markdown
| Column                | Null/Empty Count | Percentage | Non-Empty Count |
| --------------------- | ---------------- | ---------- | --------------- |
| awardwin              | 56008            | 100.00%    | 0               |
| awardyr               | 56008            | 100.00%    | 0               |
| awardcat              | 56008            | 100.00%    | 0               |
| typeoforganisation    | 56007            | 100.00%    | 1               |
| projects              | 56007            | 100.00%    | 1               |
| awardentry            | 56007            | 100.00%    | 1               |
| jobTitle              | 56006            | 100.00%    | 2               |
| cid                   | 56006            | 100.00%    | 2               |
| contact_details       | 56003            | 99.99%     | 5               |
| ReaderSurvey          | 56002            | 99.99%     | 6               |
| wherecontactmade      | 55996            | 99.98%     | 12              |
| project               | 55942            | 99.88%     | 66              |
| cpd_details           | 55816            | 99.66%     | 192             |
| tonnesinvolve         | 55771            | 99.58%     | 237             |
| region                | 55657            | 99.37%     | 351             |
| numberatpractice      | 55413            | 98.94%     | 595             |
| cpdid                 | 54984            | 98.17%     | 1024            |
| mob                   | 51441            | 91.85%     | 4567            |
| country               | 50429            | 90.04%     | 5579            |
| mlid                  | 48568            | 86.72%     | 7440            |
| fax                   | 43592            | 77.83%     | 12416           |
| application           | 42550            | 75.97%     | 13458           |
| email                 | 38090            | 68.01%     | 17918           |
| county                | 37832            | 67.55%     | 18176           |
| publicationstosend    | 32534            | 58.09%     | 23474           |
| sid                   | 25856            | 46.16%     | 30152           |
| tel                   | 25615            | 45.73%     | 30393           |
| address2              | 20451            | 36.51%     | 35557           |
| town                  | 19297            | 34.45%     | 36711           |
| lead                  | 18608            | 33.22%     | 37400           |
| dateUpdated           | 18007            | 32.15%     | 37993           |
| postcode              | 17184            | 30.68%     | 38824           |
| address1              | 15154            | 27.06%     | 40854           |
| enquiry_details       | 14963            | 26.72%     | 41045           |
| qid                   | 14338            | 25.60%     | 41670           |
| actionreqired         | 13256            | 23.67%     | 42752           |
| enquirycategory       | 12654            | 22.59%     | 43354           |
| takenby               | 8219             | 14.67%     | 47789           |
| contactby             | 7756             | 13.85%     | 48252           |
| natureofbusiness      | 6392             | 11.41%     | 49616           |
| publications          | 5708             | 10.19%     | 50300           |
| printed               | 5707             | 10.19%     | 50301           |
| paperfilecreated      | 5707             | 10.19%     | 50301           |
| presentationprospect  | 5707             | 10.19%     | 50301           |
| awardsprospect        | 5707             | 10.19%     | 50301           |
| GAfollowup            | 5707             | 10.19%     | 50301           |
| GAfollowupcompleated  | 5707             | 10.19%     | 50301           |
| GPfollowup            | 5707             | 10.19%     | 50301           |
| GPfollowupcompleated  | 5707             | 10.19%     | 50301           |
| presentationcompleted | 5707             | 10.19%     | 50301           |
| meeting               | 5707             | 10.19%     | 50301           |
| techadvice            | 5707             | 10.19%     | 50301           |
| forename              | 2013             | 3.59%      | 53995           |
| company_name          | 1951             | 3.48%      | 54057           |
| surname               | 781              | 1.39%      | 55227           |
| title                 | 648              | 1.16%      | 55360           |
| entry_date            | 207              | 0.37%      | 55801           |
| gdprcheck             | 0                | 0.00%      | 56008           |
| input_id              | 0                | 0.00%      | 56008           |
```

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
