# Comprehensive Data Quality Assessment for Salesforce Migration

Based on a detailed analysis of the `ganew2.addressall_db` table, here's a comprehensive assessment of the data quality issues affecting the migration to Salesforce:

## 1. Account Object Data Quality Issues

### Completeness Problems:

- **Company names:** 3.5% missing (1,951 empty out of 56,008)
- **Address data:** Significant gaps across all address components
  - Address line 1: 27% missing (15,154 empty)
  - Town: 34% missing (19,297 empty)
  - County: 68% missing (37,832 empty)
  - Postcode: 31% missing (17,184 empty)
  - Country: 90% missing (50,429 empty)
- **Phone/Fax:** 46% missing phone numbers (25,615), 78% missing fax numbers (43,592)
- **Industry data:** 11% missing business type (6,392 empty)
- **Employee count:** 99% missing (55,413 empty)
- **Organization type:** Nearly 100% missing (56,007 empty)

### Duplication Issues:

- 54,057 non-empty company names but only 23,583 unique values
- **Duplication rate:** ~56% of company records are potential duplicates

### Format/Standardization Issues:

- **Industry field (`natureofbusiness`):** Contains 19 distinct values with inconsistent formatting (e.g., "Civial/Struct Engineer" has a typo)
- **Postcode field:** Contains non-postcode values (e.g., "Ireland" appears 518 times)
- **Company names:** Show variations requiring normalization (e.g., "Joseph Ash" vs "Joseph Ash Galvanizing")

## 2. Contact Object Data Quality Issues

### Completeness Problems:

- **Name components:**
  - Title: 1.2% missing (648 empty)
  - Forename: 3.6% missing (2,013 empty)
  - Surname: 1.4% missing (781 empty)
- **Contact information:**
  - Email: 68% missing (38,090 empty)
  - Phone: 46% missing (25,615 empty)
  - Mobile: 92% missing (51,441 empty)
  - Fax: 78% missing (43,592 empty)
- **Job Title:** Nearly 100% missing (56,006 empty)
- **GDPR Consent:** All records have a value, but 99.98% are marked as "False" (55,999) with only 9 marked as "True"

### Duplication Issues:

- **Based on Email (Existing Analysis):**
  - 17,918 non-empty emails but only 11,672 unique values.
  - **Duplication rate (Email):** ~35% of _non-empty email addresses_ are duplicates. (Note: Email field has 68% missing values overall).
- **Based on Normalized Name + Company Name (New Analysis):**
  - Analysis performed on records with non-empty `forename`, `surname`, and `company_name`.
  - Found 35,908 unique combinations of normalized Name + Company Name.
  - 5,991 of these combinations appear more than once.
  - These duplicate Name + Company Name combinations account for **22,085 records**.
- **Based on Phone (`tel`) (New Analysis):**
  - ~30,393 records have a non-empty `tel` value (overall ~46% missing).
  - 4,322 unique phone numbers appear more than once.
  - These duplicate phone numbers account for **18,615 records** (out of the ~30k with a phone number).
- **Based on Mobile (`mob`) (New Analysis):**
  - ~4,567 records have a non-empty `mob` value (overall ~92% missing).
  - 516 unique mobile numbers appear more than once.
  - These duplicate mobile numbers account for **1,696 records** (out of the ~4.5k with a mobile number).
- **Overall Contact Duplication:** The duplication rate for Contacts is significantly higher than initially estimated based solely on email. Analysis using Name+Company and Phone reveals substantial duplication, indicating a need for a robust contact deduplication strategy beyond just email matching.

### Format/Standardization Issues:

- **Title field:** Contains 49 distinct values with inconsistencies:
  - Misspellings (e.g., "Mt" instead of "Mr", "Nr" instead of "Mr")
  - Format variations (e.g., "Prof" vs "Prof.")
  - Non-title values (e.g., "RIBA", "Eng")
  - Multi-value entries (e.g., "Mr/Ms")

## 3. Enquiry_Database\_\_c Object Data Quality Issues

### Completeness Problems:

- **Enquiry category:** 23% missing (12,654 empty)
- **Enquiry details:** 27% missing (14,963 empty)
- **Application field:** 76% missing (42,550 empty)
- **Region field:** 99% missing (55,657 empty)
- **Contact method:** 14% missing (7,756 empty)
- **Lead source:** 33% missing (18,608 empty)

### Categorization Issues:

- **Enquiry category:** 28 distinct values with heavy concentration in generic categories:
  - "Gen. Information": 13,338 records (31% of non-empty values)
  - "Miscellaneous": 3,104 records (7% of non-empty values)
- **Contact method:** 11 distinct values with inconsistencies:
  - Contains "Edit List" (3 records) and "zz" (2 records) which appear to be test/placeholder values
- **Lead source:** 21 distinct values with inconsistencies:
  - Contains "Edit List" (2 records) which appears to be a test/placeholder value
  - Some values appear to be time-bound (e.g., "GalvIt2004") which may need updating

## 4. Overall Data Structure Issues

- **Single flat table structure:** All Account, Contact, and Enquiry data is stored in a single table without proper relational structure
- **No guaranteed unique identifiers:** No reliable way to identify unique companies or contacts
- **Inconsistent data entry:** Evidence of inconsistent data entry practices across multiple fields
- **Test/placeholder values:** Several fields contain values like "Edit List" that appear to be artifacts of the data entry system

## Scope of Work Required

### 1. Data Cleaning & Normalization

- **Account Data (High Effort):**
  - Normalize ~23,583 unique company names (handle variations, abbreviations, legal suffixes)
  - Standardize ~38,824 address records (separate components, handle abbreviations)
  - Clean and validate ~38,824 postcodes (remove non-postcode values, standardize format)
  - Map 19 distinct industry values to Salesforce standard Industry picklist values
- **Contact Data (High Effort):**
  - Standardize ~55,360 name records (consistent capitalization, handling of initials)
  - Normalize ~30,393 phone numbers to consistent format
  - Validate and correct ~17,918 email formats
  - Standardize 49 distinct title values to Salesforce standard Salutation picklist values
- **Enquiry Data (Medium-High Effort):**
  - Standardize 28 distinct enquiry categories for the custom Enquiry_Category\_\_c picklist
  - Standardize 11 distinct contact methods for the custom Contact_Method\_\_c picklist
  - Standardize 21 distinct lead sources for the custom Lead_Source\_\_c picklist
  - Clean and normalize ~13,458 application values for the custom Application\_\_c picklist

### 2. Deduplication (Very High Effort)

- **Current Approach Status:**
  - Using `recordlinkage` library with rule-based/threshold classification
  - Current performance: Precision=1.0000, Recall=0.6667, F1=0.8000
  - One False Negative pair (4959, 10267) remains problematic
- **Expanded Scope:**
  - Need to extend deduplication beyond the validation dataset to all 56,008 records
  - Must handle the ~56% duplication rate in company records (~30,000 potential duplicates)
  - Must address the ~35% duplication rate in contact records (~6,000 potential duplicates)
  - Need to explore Machine Learning classifiers as mentioned in the plan to improve recall while maintaining precision

### 3. Data Transformation & Migration (High Effort)

- **Structural Changes:**
  - Split the flat table into proper relational structure:
    - Accounts table (~23,583 unique companies after deduplication)
    - Contacts table (~11,672 unique contacts after deduplication)
    - Enquiries table (up to 56,008 records, linked to the appropriate Account and Contact)
- **External ID Generation:**
  - Generate `Account.Legacy_Company_ID__c` for each unique company entity
  - Generate `Contact.Legacy_Contact_ID__c` for each unique contact
  - Use source `input_id` directly as `Enquiry_Database__c.Legacy_ID__c`
- **Field Mappings & Transformations:**
  - Implement all field mappings defined in `salesforce_mapping.csv`
  - Apply specific transformation rules for each field type
  - Handle picklist value mappings for standard and custom fields

## Recommendations

1.  **Prioritize Account Deduplication:**
    - Continue improving the `recordlinkage` approach
    - Implement and evaluate Machine Learning classifiers as planned
    - Consider adding more fields to the comparison vectors if needed
2.  **Implement Tiered Data Cleaning:**
    - Tier 1: Critical fields for record identification and relationships
    - Tier 2: Fields needed for basic functionality and reporting
    - Tier 3: Nice-to-have fields with lower business impact
3.  **Address Completeness Gaps:**
    - For critical fields with high missing rates, consider:
      - Default values where appropriate
      - Marking records for post-migration review
      - Excluding severely incomplete records from migration
4.  **Enhance Test Harness:**
    - Expand the validation dataset to include more edge cases
    - Implement comprehensive logging of comparison vectors
    - Add visualization of similarity scores to aid in threshold tuning
5.  **Implement Robust Error Handling:**
    - Design the migration script to handle and log all data quality issues
    - Create detailed error reports for manual review
    - Implement a retry mechanism for failed records

## Conclusion

The data quality issues in the `ganew2.addressall_db` table are significant and will require substantial effort to address for a successful Salesforce migration. The current deduplication work using the `recordlinkage` library is on the right track but needs enhancement to handle the full dataset's complexity.

The most critical challenges are:

1.  The high duplication rates in both company (56%) and contact data (35%)
2.  The lack of guaranteed unique identifiers
3.  The significant completeness gaps in key fields
4.  The inconsistent formatting across multiple fields

With a structured approach focusing first on deduplication and critical data cleaning, these challenges can be addressed to ensure a successful migration to Salesforce.

## Estimated Time to Solution

Achieving a validated, repeatable pipeline for cleaning, deduplicating (Accounts then Contacts), and preparing all 56,000 records from `ganew2.addressall_db` for a high-precision Salesforce migration is a significant task. Considering the identified data quality issues (completeness, formatting, high duplication) and the necessary iterative steps:

1.  **Iterative Cleaning & Standardization:** Applying and refining rules across numerous fields.
2.  **Account Deduplication:** Tuning and validating `recordlinkage` (rule-based or ML) to achieve high precision is crucial and requires iteration against ground truth.
3.  **Contact Deduplication:** Designing and implementing a robust strategy (likely fuzzy matching + phone/email within confirmed Account groups), heavily dependent on accurate Account deduplication results.
4.  **Pipeline Integration & Testing:** Building the end-to-end process, generating IDs, mapping fields, and validating outputs thoroughly.

A realistic timeframe for a **dedicated expert**, assuming clear business requirements and access to appropriate tools/environments, is **3 to 5 working weeks (15-25 working days)**. This range reflects the inherent complexity, dependencies (especially Contact dedupe relying on Account dedupe), and iterative nature of data quality work at this scale. Factors like stakeholder review cycles or unforeseen data anomalies could extend this timeframe. While alternative approaches (e.g., migrating with less cleaning, using newer AI tools) might offer faster _initial_ results, they carry different risks and may not meet the goal of a high-fidelity initial migration.
