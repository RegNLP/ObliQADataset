# ObliQA: Obligation-based Question-Answering Dataset for Regulatory Compliance

## Overview
The ObliQA Dataset is a specialized resource designed to facilitate the development of RegNLP systems capable of understanding and answering questions based on regulatory texts. This dataset leverages regulatory documents from Abu Dhabi Global Markets (ADGM), which set regulations for financial services in the UAE free zones. It includes a meticulously organized collection of regulatory documents, each maintaining the integrity of the complex legal structure.

## Dataset Description
The ObliQA Dataset is built from approximately 640,000 words of regulatory text across 40 documents, each ranging from 30 to 100 pages. These documents include complex internal structures such as subsections, numbered clauses, and cross-references.

### Document Standardization
Each document is manually structured and converted into a text format to maintain data integrity and facilitate computational analysis. The structuring process includes the isolation of tables and graphical content, converting them into textual annotations within the documents.

### Question-Passage Pairs
The dataset consists of 27,869 questions categorized by the number of passages they reference, aiming to cover a wide range of compliance-related topics. Questions are generated using advanced NLP techniques, ensuring relevance and practical applicability to real-world regulatory scenarios.

## Data Collection
The source documents are provided in `.docx` format by ADGM and undergo a rigorous process of standardization and structuring. They are then converted into `.txt` format and finally into structured JSON format for further processing.

## Question Generation
Questions are generated to focus solely on essential regulatory content, removing extensive descriptions and titles. The GPT-4 model is employed to generate both questions and answers, ensuring high relevance and accuracy.

## Validation
The validation of question-passage pairs is conducted using Natural Language Inference (NLI) techniques to ensure that the questions accurately reflect the content of the passages.

## Dataset Distribution
Here is the updated breakdown of questions by the number of input passages:
| Group    | #Question | 1 Passage | 2 Passages | 3 Passages | 4 Passages | 5 Passages | 6 Passages |
|----------|-----------|-----------|------------|------------|------------|------------|------------|
| Group 1  | 7988      | 7988      |            |            |            |            |            |
| Group 2  | 7254      | 5432      | 1822       |            |            |            |            |
| Group 3  | 5365      | 3385      | 1434       | 546        |            |            |            |
| Group 4  | 4558      | 3057      | 1102       | 305        | 94         |            |            |
| Group 10 | 2704      | 1325      | 678        | 345        | 174        | 121        | 61         |
| **Total**| **27869** | **21187** | **5036**   | **1196**   | **268**    | **121**    | **61**     |


## Distribution by Dataset Split
| Split       | #Question | 1 Passage | 2 Passages | 3 Passages | 4 Passages | 5 Passages | 6 Passages |
|-------------|-----------|-----------|------------|------------|------------|------------|------------|
| Train       | 22295     | 16946     | 4016       | 975        | 202        | 100        | 56         |
| Test        | 2786      | 2126      | 506        | 105        | 36         | 9          | 4          |
| Development | 2888      | 2215      | 514        | 116        | 30         | 12         | 1          |
