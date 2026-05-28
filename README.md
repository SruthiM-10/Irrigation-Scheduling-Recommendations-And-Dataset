# Irrigation-Scheduling-Recommendations-And-Dataset

## Abstract

Irrigation scheduling strongly influences crop yield and water use efficiency, yet practical adoption of “smart” scheduling methods remains limited because method performance varies across crops, climates, soils, and farm constraints. Prior work is dominated by localized case studies, making it difficult for growers to select an appropriate method for their context. This study developed a large language model (LLM)-assisted literature mining pipeline to extract and normalize reported irrigation scheduling methods and outcomes across published studies. The final processed dataset contains 1083 samples from 328 papers, covering 1037 unique irrigation scheduling methods, 345 plant types, and 415 geographical locations. To illustrate potential usage, we built a yield prediction model that, for a given scenario, predicts plant yield for candidate scheduling methods. Six different irrigation methods, including those based on soil moisture, evapotranspiration, and deficit irrigation, were used in this analysis. XGBoost achieved the best performance with an R2 score of 0.867. Therefore, the unique contributions of this paper are a LLM-assisted literature mining pipeline, the first known dataset of its kind containing irrigation scheduling methods, and a demonstrated example of how ML models can be trained on this dataset and be used for personalized irrigation method effectiveness prediction at scale.

**Link To Research Paper**
[TBD]

**Link to Related Website**
[www.irrigationmatch.com](https://www.irrigationmatch.com/home)

## Setup and Running the Code

### 1. Repository Setup

Clone this repository to your local machine:
```bash
git clone <repository-url>
cd Irrigation-Scheduling-Recommendations-And-Dataset
```

### 2. Install Dependencies

It is recommended to use a virtual environment. Install the required Python packages using the provided `requirements.txt` file:
```bash
pip install -r requirements.txt
```

### 3. Environment Variables (`.env`)

You need to create a `.env` file in the root directory of this repository to store your API keys and configuration settings.

Set up your `.env` file with the following variables:

```ini
LANGCHAIN_TRACING_V2="false"
LANGCHAIN_ENDPOINT="https://api.smith.langchain.com"
LANGCHAIN_PROJECT="irrigation-crawler"
OPENAI_API_KEY="your_openai_api_key_here"
SERPSTACK_API_KEY="your_serpstack_api_key_here"
GEOCODING_API_KEY="your_geocoding_api_key_here"
```
*Note: We recommend keeping the Langchain configuration values exactly as filled in above.*

### 4. Important Notes on Running the Code

* **Review Path Names:** Please ensure that all file and directory path names in the scripts are reviewed and changed accordingly to match your local file system structure before running them.
* **Google Colab / Jupyter Notebooks Recommended:** It is highly recommended to run these scripts in Google Colab or Jupyter Notebooks. You can convert the `.py` files to `.ipynb` notebooks for better visualization and step-by-step execution, although they can also be executed as standard `.py` scripts.

## Directories 
All code was originally made on Google Colab before being transported to this repository.

### Code
Contains 3 subdirectories:
  - Crawler
      - Step 1: `updated_crawler+scoring.py` contains full crawling and scraping methodology for downloading papers
      - Step 2: `feature_extraction_only.py` contains full LLM prompting and generates the final raw dataset of samples
      - Step 3: `data_deduplication.py` contains the post-crawl checks for data duplication

  - Data Processing
      - Step 1: `data_exploration.py` analyzes which features are most populated
      - Step 2: `initial_analysis_and_cleaning.py` drops samples without irrigation scheduling methods and plant yields (the two most important features) and begins removing non-numeric values from plant yield
      - Step 3: `categorization_and_unit_conversions.py` standardizes both categorical and numerical features
      - Step 4: `filling_null_values.py` fills null values of important but less populated features like soil type and water productivity
      - Step 5: `prep_for_model.py` prepares for model training by dropping outliers and doing train/test split

  - Evaluation
      - `model_training.py`
      - `prompt_sensitivity.py` - for LLM relevance scoring prompt 
      - `skew_analysis.py` - on raw dataset

### Data
  - `all_downloaded_papers_doi.csv` - all downloaded papers after crawling for 72 hours
  - `final_raw_dataset_updated_doi.csv` - raw dataset after extracting data from all downloaded papers
  - `all_manually_extracted_data_vs_llm_output.csv` - manually extracted data from 10% of dataset versus what the LLM extracted
  - `summary_for_manual_check_of_feature_extraction.csv` - a table with all experimental recalls and descriptions
  - `final_dataset_updated_doi.csv` - final processed dataset
