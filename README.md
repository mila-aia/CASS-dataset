# CASS: Dataset for French text Summarization

The code available in this repository allows to clean the CASS dataset and gives the split used in
the paper '[STRASS: A Light and Effective Method for Extractive Summarization Based on Sentence Embeddings](https://www.aclweb.org/anthology/papers/P/P19/P19-2034/)' (Bouscarrat et al., 2019) that has appeared in the [Student Research Workshop](https://sites.google.com/view/acl19studentresearchworkshop/accepted-papers) of ACL 2019. It has been subsequently updated by Mila staff members to enable using a more recent version of the CASS dataset. The original source code repository can be found [here](https://github.com/euranova/CASS-dataset).

This dataset is composed of decisions made by the French Court of cassation and summaries of these decisions made by lawyer.

If you use this code, please cite:

```
@inproceedings{bouscarrat-etal-2019-strass,
    title = "{STRASS}: A Light and Effective Method for Extractive Summarization Based on Sentence Embeddings",
    author = "Bouscarrat, L{\'e}o  and
      Bonnefoy, Antoine  and
      Peel, Thomas  and
      Pereira, C{\'e}cile",
    booktitle = "Proceedings of the 57th Conference of the Association for Computational Linguistics: Student Research Workshop",
    month = jul,
    year = "2019",
    address = "Florence, Italy",
    publisher = "Association for Computational Linguistics",
    url = "https://www.aclweb.org/anthology/P19-2034",
    pages = "243--252",
    abstract = "This paper introduces STRASS: Summarization by TRAnsformation Selection and Scoring. It is an extractive text summarization method which leverages the semantic information in existing sentence embedding spaces. Our method creates an extractive summary by selecting the sentences with the closest embeddings to the document embedding. The model earns a transformation of the document embedding to minimize the similarity between the extractive summary and the ground truth summary. As the transformation is only composed of a dense layer, the training can be done on CPU, therefore, inexpensive. Moreover, inference time is short and linear according to the number of sentences. As a second contribution, we introduce the French CASS dataset, composed of judgments from the French Court of cassation and their corresponding summaries. On this dataset, our results show that our method performs similarly to the state of the art extractive methods with effective training and inferring time.",
}
```

## Download

### The data: French 'CASS dataset'
First, the data needs to be downloaded.

Download the file starting with 'freemium_cass' (in our case we used the version 20220417).

This file can be accessed [here](https://drive.google.com/file/d/1T-j5ogtD8kMe3QJKwK1doT34q2HipD5W/view?usp=drive_link).

The most recent version of this file can be accessed [here](https://echanges.dila.gouv.fr/OPENDATA/CASS/).

Information about this dataset can be found [here](https://www.data.gouv.fr/fr/datasets/cass)).

Decompress the file:
```shell
tar -zxvf Freemium_cass_global_20220417-170000.tar.gz
```

### Install dependencies with conda

```shell
conda env create
```


## Preprocess the data

As the data is in an XML format with many information and some documents are fully uppercased
and some others not, there is a need to preprocess the data.

First, activate your virtual environment:
```
conda activate cass
```

Launch preprocessing_CASS.py with the path of your downloaded CASS dataset :

```shell
python preprocessing_CASS.py --data_dir path_to_your_data (--clean_dir path_to_clean_data)
```

Example:

```shell
python preprocessing_CASS.py --data_dir cass
```

With this command all the cleaned files will be created in a folder called cleaned_files. It will contains
all the documents preprocessed in the following format:

```
sent_textA
sent_textB
sent_textC
@highlight
sent_sumA
sent_sumB
```

All sentences are lowercased and accents are removed.
The file contains one sentence per row.

## Data split

In the publication, we used the same split on all our experiments. These splits can be found in the folder
`data_split_20180315`. The split are composed of 80% of train, 10% of valid and 10% of test. In each file, there is one
name of document on each line, without any extension.

New splits can be automatically created using the `create_splits.py` script:
```
$ python create_splits.py -h
usage: create_splits.py [-h] [--cleaned-files-dir CLEANED_FILES_DIR] [--data-split-dir DATA_SPLIT_DIR] [--train-size TRAIN_SIZE] [--val-size VAL_SIZE] [--test-size TEST_SIZE] [--seed SEED]

A utility script to create random splits for the CASS dataset

options:
  -h, --help            show this help message and exit
  --cleaned-files-dir CLEANED_FILES_DIR
                        path to cleaned files directory (default: ./cleaned_files/)
  --data-split-dir DATA_SPLIT_DIR
                        path to data splits directory (default: ./data_split_20220417/)
  --train-size TRAIN_SIZE
                        the proportion of the dataset to include in the train split (default: 0.8)
  --val-size VAL_SIZE   the proportion of the dataset to include in the validation split (default: 0.1)
  --test-size TEST_SIZE
                        the proportion of the dataset to include in the test split (default: 0.1)
  --seed SEED           random seed (default: 42)
```
