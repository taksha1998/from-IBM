# Entity Linking: disambiguation of "Emerson" mentions in sentences

<a href="https://www.youtube.com/watch?v=8u57WSXVpmw" target="_blank"><img src="https://user-images.githubusercontent.com/13643239/81293769-216fd180-906e-11ea-9f9c-d9dec9163dcc.png" width="300" height="auto" align="right" /></a>

This directory contains the datasets and scripts for an example project using
[spaCy's Entity Linking (EL) functionality ](https://spacy.io/usage/linguistic-features#entity-linking)
to disambiguate "Emerson" mentions in text to unique identifiers from Wikidata. As an example use-case, we consider
three different people called Emerson: [an Australian tennis player](https://www.wikidata.org/wiki/Q312545),
[an American writer](https://www.wikidata.org/wiki/Q48226), and a
[Brazilian footballer](https://www.wikidata.org/wiki/Q215952).

Roughly speaking, the following steps are performed in this project.
First, a pretrained model is used to perform Named Entity Recognition (NER).
Then, we create a Knowledge Base (KB) in spaCy that holds the information of the entities we want to disambiguate.
Next, we use [Prodigy](https://prodi.gy) to create some manually
annotated data with a custom annotation recipe. Finally, we create a new Entity Linking component in spaCy,
and train it with the annotated data. We test the model on a few unseen sentences.

> 📺 **This project was created as part of a [step-by-step video tutorial](https://www.youtube.com/watch?v=8u57WSXVpmw).**

## Scripts

All code to create the KB and the EL component in spaCy, can be found in [`el_tutorial.py`](scripts/el_tutorial.py).
Alternatively, you can execute this code in a Jupyter notebook: [`notebook_video.ipynb`](scripts/notebook_video.ipynb).
Both files cover the same steps:
 * Read in a pre-defined CSV file with the information to construct our Knowledge Base
 * Parse the manually annotated data and convert it to the right training format
 * Create a new entity linking pipe and train it
 * Apply the entity linker to some unseen data to test its performance

## Prodigy annotation

To perform the manual annotation in Prodigy, we have written a custom recipe [`el_recipe.py`](scripts/el_recipe.py).

As input, we need to provide the Knowledge base `my_kb` and NER pipeline `my_nlp` that are created
with the scripts described in the previous section. Further, the file
[`emerson_input_text.txt`](prodigy/emerson_input_text) lists 30 sentences from Wikipedia containing just
the mention "Emerson" and not the full name. These sentences are then annotated with Prodigy by executing the command
```
prodigy entity_linker.manual emersons_annotated emerson_input_text.txt my_nlp/ my_kb entitites.csv -F el_recipe.py
```
The final results are stored to file with
```
prodigy db-out emersons_annotated >> emerson_annotated_text.jsonl
```
This JSONL file is included here as well in the [`prodigy`](prodigy) subdirectory so the scripts can be run without
having to (re)do this manual annotation.
