# DLL Catalog Reconciliation

This is a git repository for experimenting with machine learning models to process metadata records for editions of Latin works to be added to the [DLL Catalog](https://catalog.digitallatin.org/).

Specifically, this repository contains data files and Jupyter notebooks for: 

- fine-tuning the DistilBERT base multilingual cased model to identify authors as Greek or Latin
- fine-tuning the DistilBERT base multilingual cased model to reconcile Latin authors to authority records from the DLL Catalog
- making inferences from real-world data files
- attempting to use LatinCy to process and match titles to DLL Catalog work records

To replicate the environment used in training these models, run `conda install --yes --file requirements.txt` from the root directory of this repository.

## Disclosure

Much of the code in this repository was written and/or modified by Samuel J. Huskey. The following sources have influenced practically every line:

- François Chollet, _Deep Learning with Python_, Second Edition (Manning, 2021)
- Patrick J. Burns, Getting Started with LatinCy (2024): <https://diyclassics.github.io/latincy-book/>
- Chat GPT 4o: <https://chatgpt.com/>
- Gemini on Colab: <https://gemini.google.com/>
- HuggingFace API and Tutorials: <https://huggingface.co/>
- PyTorch Tutorials: <https://pytorch.org/>

Indeed, this entire project is an example of how AI can accelerate digital scholarship, since most of it emerged from conversations with chatbots, particularly Chat GPT, but also GitHub's Copilot and Google's Gemini.

## Structure of this Repository

### colab

This directory contains versions of the Jupyter notebooks and copies of the data files for use on Google Colab. The difference is that `pip` statements have been added to install the necessary modules that aren't included in the standard Colab environment. Each notebook includes an introductory statement about the data files that must be uploaded into the Colab environment.

### data

The `data` directory contains several CSV files used in the Jupyter Notebooks in the `python` directory.

The most important files in `data` are:

- `authors_db.csv`: Used for mapping variant names to authorized names and DLL IDs.
- `deduped-greek_and_latin.csv`: Used for training the Greek-Latin classifier model. It was renamed to `greek_latin_authors.csv` for storage as a [dataset on HuggingFace Hub](https://huggingface.co/datasets/sjhuskey/greek_latin_authors).
- `author_data.csv`: Used for training the author classification model. It was renamed to `latin_author_dll_id.csv` for storage as a [dataset on HuggingFace Hub](https://huggingface.co/datasets/sjhuskey/latin_author_dll_id)
- `1908698974-1722799169.txt`: The original file downloaded from HathiTrust.
- `hathi2.csv`: A cleaned and reformatted version of the original file downloaded from HathiTrust.
- `works_db.csv`: Used for mapping works to their authors.

### output

The `output` directory contains files generated during analysis. Note that some of these files are reused as data files in some notebooks.

### python

This directory contains the Jupyter notebooks and Python files used in working with and analyzing the data. 

The file `python/fine_tune_distilmbert_author_local.ipynb` was used to fine-tune the [DistilBERT Multilingual Cased](https://huggingface.co/distilbert/distilbert-base-multilingual-cased) model to create a model for matching the names of authors of Latin texts with their Digital Latin Library ID: [sjhuskey/distilbert_multilingual_cased_latin_author_identifier](<https://huggingface.co/sjhuskey/distilbert_multilingual_cased_latin_author_identifier>). A version of that notebook is available in the `colab` directory for use on Google Colab.

The file `python/fine_tune_distilmbert_greek_local.ipynb` was used to fine-tune the [DistilBERT Multilingual Cased](https://huggingface.co/distilbert/distilbert-base-multilingual-cased) model to create a model for labeling names of authors as "Greek" or "Latin", according to the language in which they primarily wrote: [sjhuskey/distilbert_multilingual_cased_greek_latin_classifier](<https://huggingface.co/sjhuskey/distilbert_multilingual_cased_greek_latin_classifier>). A version of that notebook is available in the `colab` directory for use on Google Colab.

The file `author_matching.ipynb` is the main notebook for running the models to identify authors as Greek or Latin and to reconcile them with authority records in the DLL Catalog. A version of that notebook is available in the `colab` directory for use on Google Colab.

The file `title_matching.ipynb` was used to attempt to reconcile titles from the test dataset to DLL Catalog work records. It requires the use of a different virtual environment, which can be reproduced by running `conda install --yes --file requirements-dllspacy.txt` in the root directory of this repository. A version of that notebook is available in the `colab` directory for use on Google Colab.

The file `model_probabilistic_deterministic.ipynb` compares the inferences of the models to deterministic and probabilistic (fuzzy) matching. A version of that notebook is available in the `colab` directory for use on Google Colab.

`utilities.py` contains some repeated functions that are used in the Jupyter notebooks.

The `cleaning_exploration` directory contains files used to, well, clean the data and explore it.🧐

### Directory Tree

```bash
|-- LICENSE
|-- README.md
|-- colab
|-- data
|-- output
|-- python
  |-- cleaning_exploration
    |-- greek_cleanup
    |-- input
    |-- output
  |-- author_matching.ipynb
  |-- data-preparation.ipynb
  |-- fine_tune_distilmbert_author_local.ipynb
  |-- fine_tune_distilmbert_greek_local.ipynb
  |-- model_probabilistic_deterministic.ipynb
  |-- title_matching.ipynb
  |-- utilities.py
|-- requirements.txt
|-- requirements-dllspacy.txt
```

## License

The contents of this repository are licensed under the terms of the [GNU Affero General Public License](https://www.gnu.org/licenses/agpl-3.0.html).