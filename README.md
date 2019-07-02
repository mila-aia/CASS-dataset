# CASS: Dataset for French text Summarization

The code available in this repository allows to clean the CASS dataset and gives the split used in
the paper 'STRASS: A Light and Effective Method for Extractive Summarization Based on Sentence Embeddings' (Bouscarrat et al., 2019)
that will appear in the [Student Research Workshop](https://sites.google.com/view/acl19studentresearchworkshop/accepted-papers) of ACL 2019.

This dataset is composed of decisions made by the French Court of cassation and summaries of these decisions made by lawyer.

If you use this code, please cite:

```
@InProceedings{bouscarrat_cass_2019,
  author = 	"Bouscarrat, Léo and
          Bonnefoy, Antoine and
          Peel, Thomas and
          Pereira, Cécile",
  title = 	"STRASS: A Light and Effective Method for Extractive Summarization Based on Sentence Embeddings ",
  booktitle = "Proceedings of {ACL} 2019, Student Research Workshop",  
  year = 	"2019",
  publisher = 	"Association for Computational Linguistics"
}
```

## Download

### The data: French 'CASS dataset'
First, the data needs to be downloaded.
* Download the file starting with 'freemium_cass' (in our case we used the version 20180315).
This file can be accessed [here](ftp://echanges.dila.gouv.fr/CASS/).
Information about this dataset can be found [here](https://www.data.gouv.fr/fr/datasets/cass)).

```shell
wget ftp://echanges.dila.gouv.fr/CASS/Freemium_cass_global_20180315-170000.tar.gz
```

* Uncompress the file

### Install the Spacy French module

The only module necessary is spacy and the model for the language of the documents. For the CASS dataset it's French.

```shell
python install spacy
python -m spacy download fr
```

or, if you use conda:

```shell
conda install -c conda-forge spacy 
python -m spacy download fr
```


## Preprocess the data

As the data is in an XML format with many information and some documents are fully uppercased
and some others not, there is a need to preprocess the data.

Launch preprocessing_CASS.py with the path of your downloaded CASS dataset :

```shell
python3 preprocessing_CASS.py --data_dir path_to_your_data (--clean_dir path_to_clean_data)
```

Example:

```shell
python3 preprocessing_CASS.py --data_dir input_data/20180315-170000/
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
data_split. The split are composed of 80% of train, 10% of valid and 10% of test. In each file, there is one
name of document on each line, without any extension.