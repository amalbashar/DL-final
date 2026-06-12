# LLM Functional Interpretation of Differential Gene Expression

This repository contains the notebook and input data files used for LLM-based biological interpretation of colorectal adenoma differential gene-expression signatures, with comparison against GO Biological Process and KEGG enrichment results.

The goal of this workflow is not to replace GO or KEGG enrichment analysis, but to evaluate whether a large language model (LLM) can act as an assisting interpretive layer for summarizing gene-list signals into biological themes.

## Important Rule

Do **not** rename the input data files randomly.
The notebook depends on the dataset name inside the file names to automatically match the UP, DOWN, GO, and KEGG files.

## Recommended Repository Structure

```text
project-folder/
├── notebooks/
│   └── final_llm_gene_interpretation.ipynb
├── data/
│   ├── UP-50-GSE8671.xlsx
│   ├── DOWN-50-GSE8671.xlsx
│   ├── GO-up-GSE8671.txt
│   ├── GO-down-GSE8671.txt
│   ├── KEGG-up-GSE8671.txt
│   └── KEGG-down-GSE8671.txt
├── outputs/
├── requirements.txt
├── .gitignore
└── README.md
```

## File Naming Rules

All files from the same dataset must use the exact same `DATASET_NAME`.

For example, if the dataset name is `GSE8671`, the required file names are:

```text
UP-50-GSE8671.xlsx
DOWN-50-GSE8671.xlsx
GO-up-GSE8671.txt
GO-down-GSE8671.txt
KEGG-up-GSE8671.txt
KEGG-down-GSE8671.txt
```

The gene-list files should follow this format:

```text
UP-50-DATASETNAME.xlsx
DOWN-50-DATASETNAME.xlsx
```

The GO/KEGG enrichment files should follow this format:

```text
GO-up-DATASETNAME.txt
GO-down-DATASETNAME.txt
KEGG-up-DATASETNAME.txt
KEGG-down-DATASETNAME.txt
```

## Dataset Name Rule

The dataset name can be any name chosen by the user. It does not have to be `GSE8671` or `VALIDATION`.

For example, the following dataset names are acceptable:

```text
GSE8671
VALIDATION
DATASET2
TEST
MYDATA
COLON_ADENOMA
```

The only requirement is that the same dataset name must be written exactly the same in all related files.

For example, if the chosen dataset name is `DATASET2`, the required files should be:

```text
UP-50-DATASET2.xlsx
DOWN-50-DATASET2.xlsx
GO-up-DATASET2.txt
GO-down-DATASET2.txt
KEGG-up-DATASET2.txt
KEGG-down-DATASET2.txt
```

The notebook detects the dataset name automatically from the UP and DOWN gene-list files.

To run another dataset, the user only needs to change these two file names at the beginning of the notebook:

```python
up_file = DATA_DIR / "UP-50-GSE8671.xlsx"
down_file = DATA_DIR / "DOWN-50-GSE8671.xlsx"
```

For another dataset:

```python
up_file = DATA_DIR / "UP-50-DATASET2.xlsx"
down_file = DATA_DIR / "DOWN-50-DATASET2.xlsx"
```

After that, the notebook will automatically search for the matching GO and KEGG files using the same dataset name.

Therefore, the code logic does not need to be changed. Only the first two input file names should be updated when switching datasets.

## Column Rules

The UP and DOWN gene-list files should contain a gene-symbol column. Recommended column name:

```text
Gene.symbol
```

The GO/KEGG enrichment files should contain columns for:

* enrichment term or pathway name
* enriched genes
* adjusted p-value

Common accepted column names include:

```text
Term
Description
Pathway
Name
Genes
Adjusted P-value
Adjusted_P_value
adj.P.Val
FDR
qvalue
```

## Main Workflow

The workflow includes:

1. Loading pre-filtered Top-50 upregulated and Top-50 downregulated gene lists.
2. Generating structured LLM-based biological interpretations.
3. Running self-consistency analysis.
4. Applying API-based best-of-k candidate selection.
5. Checking for hallucinated genes.
6. Comparing selected LLM themes with real GO/KEGG enrichment results.
7. Generating LLM-predicted GO-like and KEGG-like profiles.
8. Comparing predicted profiles with real Top-10 GO/KEGG enrichment results.
9. Applying theme stability analysis.
10. Applying GO/KEGG significance caution flags.

## API Key Rule

Do not upload a real API key to GitHub.

Keep this line as a placeholder:

```python
os.environ["NVIDIA_API_KEY"] = "YOUR_API_KEY_HERE"
```

Or set the key locally as an environment variable before running the notebook.

## Main Outputs

The workflow generates CSV and JSON outputs, including:

```text
input_DATASETNAME_top50_upregulated_genes.csv
input_DATASETNAME_top50_downregulated_genes.csv
llm_gene_reasoning_outputs_DATASETNAME_top50.json
real_go_kegg_categorical_comparison_DATASETNAME.csv
real_go_kegg_categorical_summary_DATASETNAME.csv
LLM_predicted_GO_KEGG_profiles_DATASETNAME.csv
LLM_predicted_vs_real_GO_KEGG_categorical_comparison_DATASETNAME.csv
LLM_predicted_vs_real_GO_KEGG_categorical_summary_DATASETNAME.csv
GO_upregulated_top10_terms_DATASETNAME.csv
GO_downregulated_top10_terms_DATASETNAME.csv
KEGG_upregulated_top10_pathways_DATASETNAME.csv
KEGG_downregulated_top10_pathways_DATASETNAME.csv
```

## Notes on Interpretation

LLM outputs should be interpreted as hypothesis-generating biological summaries.

They should not be treated as statistically validated enrichment results because the LLM does not calculate:

* p-values
* adjusted p-values
* enrichment ratios
* database-ranked enrichment statistics

GO and KEGG remain necessary for database-grounded and statistically supported enrichment analysis.

## Disclaimer

This project is intended for academic and exploratory research purposes.
The LLM-generated outputs are not clinical recommendations and should not be used as diagnostic or therapeutic evidence.
