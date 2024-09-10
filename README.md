# ObliQA: Obligation-based Question-Answering Dataset for Regulatory Compliance
![Figure illustrating how the ObliQA dataset was created](https://raw.githubusercontent.com/RegNLP/ObliQADataset/main/datasetPrepartion.png "ObliQA Dataset creation.")

## Overview
The ObliQA Dataset is a specialized resource designed to facilitate the development of RegNLP (Regulatory Natural Language Processing) systems capable of understanding and answering questions based on regulatory texts. This dataset leverages regulatory documents from Abu Dhabi Global Markets (ADGM), which set regulations for financial services in the UAE free zones. It includes a meticulously organized collection of regulatory documents, each maintaining the integrity of the complex legal structure. The ObliQA Dataset is built from approximately 640,000 words of regulatory text across 40 documents, each ranging from 30 to 100 pages. These documents include complex internal structures such as subsections, numbered clauses, and cross-references.

ObliQA is valuable for a wide range of regulatory NLP research applications, including:

* Automated Compliance Checking: Training models to verify if an entity's practices align with regulatory requirements.
* Regulatory Information Retrieval: Enhancing search engines and retrieval systems tailored to regulatory texts.
* Question Answering Systems: Developing systems that provide precise answers to regulatory queries.
* Summarization and Simplification: Creating tools to convert complex regulatory texts into simpler, more accessible formats.
* Cross-Document Analysis: Facilitating the analysis of multiple regulatory documents to identify common themes or conflicting requirements.
* Legal and Compliance Chatbots: Building conversational agents to assist with real-time regulatory compliance queries.
* Policy Analysis and Impact Assessment: Analyzing the potential impact of new or modified regulations.
* Regulatory Gap Analysis: Identifying gaps or ambiguities in existing regulations.
* Compliance Auditing: Supporting auditing processes by automatically identifying compliance issues.
* Obligation Detection: Detecting and extracting specific obligations and requirements from regulatory texts.
  
By providing a comprehensive and structured dataset, ObliQA enables the development of advanced tools and systems that enhance the efficiency, accuracy, and accessibility of regulatory compliance processes, benefiting regulatory bodies, compliance officers, legal professionals, and organizations subject to regulatory oversight.




## Dataset Preparation Process
1. **Document Collection:** The source documents from ADGM are provided in .docx format. They undergo meticulous manual structuring to ensure consistent formatting, including isolating tables with \Table Start and \Table End tags and converting graphical content into text with \Figure Start and \Figure End tags. These documents are then converted into .txt and finally into structured JSON format. Access the structured documents [here](https://github.com/RegNLP/ObliQADataset/blob/main/StructuredRegulatoryDocuments.zip).

    #### Structure of the Documents
    
    ``` json
    {
        "ID": "e3515a08-2bd7-4da4-b0ff-9044873943b6",
        "DocumentID": 11,
        "PassageID": "1.1.2",
        "Passage": "Where a Rule prescribes a requirement on a Listed Entity or an Undertaking, each Director, Partner or other Person charged with the management of that Listed Entity or Undertaking must take all reasonable steps within its control to secure compliance with the requirement by the Reporting Entity or Undertaking."
    }
    ```

2. **Input Definition:** After standardizing the documents, the next step is to define the inputs for the GPT-4 model. This involves determining if a specific rule is applicable and categorizing the questions based on the desired question-passage pairs.

  * Single Passage Questions: For questions that reference a single passage, the input is straightforward and doesn't require additional structuring.

  * Multiple Passage Questions: For questions that reference multiple passages, topics and related obligations are categorized and grouped based on real-world scenarios. Topics are defined with related keywords to ensure relevance. Below are some sample topics and their keywords:
    ``` 
    topics_keywords = {
      "AML Compliance": [
          "money laundering", "compliance program", "regulatory requirements"
      ], 
      "Anti-Money Laundering": [
          "AML", "money laundering", "KYC", "know your customer", "financial crime", "terrorist financing",
          "due diligence", "suspicious activity", "compliance program"
      ],
      "Audit and Monitoring": [
          "audit", "compliance monitoring", "internal audits", "external audits"
      ],
      "Blockchain Technology": [
          "blockchain", "blockchain technology", "smart contract", "tokenization"
      ],
      "Blockchain-based Securities": [
          "blockchain-based securities", "blockchain technology in securities"
      ],
      ....
      "Virtual Asset Regulation": [
          "virtual assets", "crypto assets", "digital asset regulation", "virtual asset service providers", 
          "VASP", "crypto exchanges", "crypto custodians", "ICO regulations", "token classifications"
      ]
    }
    ```

3. **GPT-4 Generation:** The `gpt-4-turbo-1106` model is employed to generate both questions and answers. The prompts used for generating questions are tailored based on whether they reference single or multiple passages.
   
    **Prompt for Single Passage Questions:**

      > Your task is to generate realistic and applied questions that pertain to the provided regulatory or compliance material. Ensure that the context implicitly contains the answer to the question.

    **Prompt Multi-Passage Questions:**

      > Imagine you are preparing a list of questions that a compliance officer at a company would ask regulatory authorities like the ADGM to clarify their understanding of compliance requirements and to ensure adherence to regulatory standards. Each question should be direct, practical, and structured to get detailed insights into specific regulatory rules. Avoid formulations that imply the company is describing its own processes, and instead frame questions that seek clarity on regulatory expectations.

4. **NLI Validation:** The generated question-passage pairs are validated using the `nli-deberta-v3-xsmall` model. The passage is the premise, and the question is the hypothesis. The NLI model checks if the question is entailed by the passage.

  * Entailment: Pairs where the passage entails the question are included in the dataset.
  * Contradictions: Pairs where the passage contradicts the question are excluded.
  * Neutral: Pairs with neutral scores are evaluated further to decide their inclusion based on their proximity to entailment or contradiction.

5. **ObliQA Dataset:** The final validated question-passage pairs are compiled into a synthetic dataset, ready for use in training and evaluating RegNLP systems. The dataset consists of 27,869 questions categorized by the number of passages they reference.

    #### Structure of the Questions and Passages
    ``` json
    {
        "QuestionID": "7824b4b4-bb50-432f-bc81-9f4cb80b2320",
        "Question": "What kind of documentation and verification does the FSRA require from a Mining Reporting Entity to prove adherence to the appropriate Mining Reporting Standard when disclosing Exploration Targets and Production Targets?",
        "Passages": [
            {
                "DocumentID": 30,
                "PassageID": "3)",
                "Passage": "INTRODUCTION. The disclosure framework for Mining Reporting Entities in Chapter 11 of MKT is substantially driven by three Mining Reporting Standards; the JORC Code, the SAMREC Code and NI 43-101 and the CIM Standards. These Mining Reporting Standards set guidelines that provide for standardized definitions and a comprehensive classification system for Mineral Resources and Ore Reserves. The FSRA intends for Chapter 11 of MKT to work closely, and be consistent, with the Mining Reporting Standards. If there are any inconsistencies, however, between FSMR or MKT and the Mining Reporting Standards, FSMR or MKT will prevail. However, this does not mean that where a Mining Reporting Standards requirement is not required under FSMR or MKT that it is acceptable for the Mining Reporting Standard requirement to be ignored."
            },
            {
                "DocumentID": 30,
                "PassageID": "9)",
                "Passage": "DISCLOSURES TO BE PREPARED IN ACCORDANCE WITH THE MINING REPORTING STANDARDS. The FSRA considers that Rules 11.2.1 and 11.2.2 are the most important Rules in relation to the requirement for Minerals activity disclosures within ADGM. Rule 11.2.1 requires that any disclosure by a Mining Reporting Entity that includes a statement about Exploration Targets, Exploration Results, Mineral Resources, Ore Reserves or Production Targets, must be prepared in accordance with a Mining Reporting Standard and in accordance with the requirements of MKT Chapter 11."
            }
        ],
        "Group": 3
    }
    
    ```

      
      #### Dataset Distribution
      Here is the updated breakdown of questions by the number of input passages:
      | Group    | #Question | 1 Passage | 2 Passages | 3 Passages | 4 Passages | 5 Passages | 6 Passages |
      |----------|-----------|-----------|------------|------------|------------|------------|------------|
      | Group 1  | 7988      | 7988      |            |            |            |            |            |
      | Group 2  | 7254      | 5432      | 1822       |            |            |            |            |
      | Group 3  | 5365      | 3385      | 1434       | 546        |            |            |            |
      | Group 4  | 4558      | 3057      | 1102       | 305        | 94         |            |            |
      | Group 10 | 2704      | 1325      | 678        | 345        | 174        | 121        | 61         |
      | **Total**| **27869** | **21187** | **5036**   | **1196**   | **268**    | **121**    | **61**     |
      
      
      #### Distribution by Dataset Split
      | Split       | #Question | 1 Passage | 2 Passages | 3 Passages | 4 Passages | 5 Passages | 6 Passages |
      |-------------|-----------|-----------|------------|------------|------------|------------|------------|
      | Train       | 22295     | 16946     | 4016       | 975        | 202        | 100        | 56         |
      | Test        | 2786      | 2126      | 506        | 105        | 36         | 9          | 4          |
      | Development | 2888      | 2215      | 514        | 116        | 30         | 12         | 1          |

## Paper
ObliQA is described in detail in the paper [Question Answering in Regulatory Compliance: Benchmark Dataset for Multi-Document Passage Retrieval in RegNLP] by Tuba Gokhan, Kexin Wang, Iryna Gurevych, and Ted Briscoe. Please cite our paper in research work that uses or discusses ObliQA. Further details will be announced soon.

### BibTeX

```shell
@misc{gokhan2024regnlpactionfacilitatingcompliance,
      title={RegNLP in Action: Facilitating Compliance Through Automated Information Retrieval and Answer Generation}, 
      author={Tuba Gokhan and Kexin Wang and Iryna Gurevych and Ted Briscoe},
      year={2024},
      eprint={2409.05677},
      archivePrefix={arXiv},
      primaryClass={cs.CL},
      url={https://arxiv.org/abs/2409.05677}, 
}

```

## Contact

Questions about the ObliQA dataset can asked by creating an issue on this repository or by sending them to <a href="mailto:regnlp2025@gmail.com<">
regnlp2025@gmail.com</a>
