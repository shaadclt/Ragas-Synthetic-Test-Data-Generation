# Synthetic Test Data Generation using LangChain, Ragas, and Groq API

This project demonstrates how to generate synthetic test data for Retrieval Augmented Generation (RAG) using Ragas. 

## Table of Contents
- [Installation](#installation)
- [Usage](#usage)
  - [Environment Setup](#environment-setup)
  - [Load Documents from PubMed](#load-documents-from-pubmed)
  - [Generate Test Sets](#generate-test-sets)
- [Output](#output)
- [References](#references)
- [License](#license)

## Installation

To get started, clone the repository and install the required dependencies.

```python
!pip install ragas langchain_community langchain_groq sentence_transformers xmltodict -q
```

## Usage
### Environment Setup
1. Import necessary libraries and set up environment variables.
2. Initialize the Groq API for data generation and critique models.
3. Set up the HuggingFace BGE embeddings for document processing.

### Load Documents from PubMed
Use the `PubMedLoader` from `langchain_community` to load documents related to a specific query (e.g., "cancer"). In this project, we load a maximum of 5 documents.

```python
loader = PubMedLoader("cancer", load_max_docs=5)
documents = loader.load()
```

### Generate Test Sets
We use the TestsetGenerator from ragas to generate test sets based on the loaded documents. The test set generation includes:

- Simple questions: 50%
- Multi-context questions: 40%
- Reasoning-based questions: 10%
  
The following code sets up the test set generation:

```python
generator = TestsetGenerator.from_langchain(
    data_generation_model,
    critic_model,
    embeddings
)
distributions = {
    simple: 0.5,
    multi_context: 0.4,
    reasoning: 0.1
}
testset = generator.generate_with_langchain_docs(documents, 5, distributions)
test_df = testset.to_pandas()
```

### Output
The output is a Pandas DataFrame containing the generated test sets, which can be further analyzed or used for model evaluation.

## License
This project is licensed under the [MIT License](/LICENSE.txt). 
