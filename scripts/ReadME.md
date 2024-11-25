
# Evaluation of ObliQA

The **ObliQA Dataset** is a curated resource for research in regulatory compliance. A total of **991 questions** were randomly selected from the dataset and evaluated by regulatory experts to ensure their applicability to real-world compliance scenarios. These questions are associated with **1,847 passages**, categorized as follows:

-   **1,605 passages (86.91%)** marked as relevant.
-   **65 passages (3.52%)** marked as irrelevant.
-   **8 passages (0.43%)** marked as null.
-   **169 passages (9.14%)** identified as relevant but not directly answering the corresponding questions.
## Dataset Splits

The validated dataset is divided into two parts:

### Part 1

-   **Number of questions and passages:** 571
-   **Published Data:** Available in [ObliQA-EvaluationResults.json](https://github.com/RegNLP/ObliQADataset/blob/main/scripts/ObliQA-EvaluationResults.json).
-   **Additional Key:** `Relation`
    -   `Relation: 1` → Relevant
    -   `Relation: 0` → Irrelevant
    -   `Relation: 2` → Relevant but does not directly answer the question.

#### Part 1 Relation Counts:

-   **Relevant (`Relation: 1`):** 773
-   **Irrelevant (`Relation: 0`):** 57
-   **Relevant but not directly answering (`Relation: 2`):** 166

### Part 2

-   **Number of questions and passages:** 446
-   **Status:** Not published due to its role in the ongoing **RIRAG shared task.**
