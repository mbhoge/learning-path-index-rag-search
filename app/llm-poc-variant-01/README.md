# lpiGPT - Learning Path Index GPT

A standalone GPT app based on [Ollama](https://github.com/jmorganca/ollama) and the [Learning Path Index Dataset](https://www.kaggle.com/datasets/neomatrix369/learning-path-index-dataset).

It's simple and runs on the local machine with smaller sized and free LLMs.

> Note: credits to this program goes to the original authors of [privateGPT](https://github.com/jmorganca/ollama/blob/main/examples/privategpt/README.md) from Ivan Martinez who contributed to an example on [jmorganca/ollama](https://github.com/jmorganca/ollama).

## Models

### Embeddings models

For embeddings model, the example uses a sentence-transformers model https://www.sbert.net/docs/pretrained_models.html 
The `all-mpnet-base-v2` model provides the best quality, while `all-MiniLM-L6-v2` is 5 times faster and still offers good quality.

### Chat models

For chat models, have a look at [this list](https://github.com/jmorganca/ollama/#model-library) on [Ollama's github repo](https://github.com/jmorganca/ollama/). The list is basic, hence other LLM resources must be consulted i.e.

- [Kaggle models](https://www.kaggle.com/models?query=LLM)
- [HuggingFace models](https://huggingface.co/models?other=LLM)
- ...(others)..

_Please share your resources on either or both of the Embeddings and Chat models with us_

## Setup

Set up a virtual environment (optional):

```shell
python3 -m venv .venv
source .venv/bin/activate
```

Install the Python dependencies:

```shell
pip install -r requirements.txt
```

Pull the model you'd like to use:

```shell
ollama pull llama2-uncensored
```

### Getting Learning Path Index datasets

```
mkdir -p source_documents

curl https://raw.githubusercontent.com/Niskarsh12/learning-path-index/main/data/Courses%20and%20Learning%20Material.csv -o "source_documents/Courses and Learning Material.csv"

curl https://raw.githubusercontent.com/Niskarsh12/learning-path-index/main/data/Learning%20Pathway%20Index.csv -o "source_documents/Learning Pathway Index.csv"
```

Or you can manually download them from the Kaggle Dataset: Learning Path Index Dataset](https://www.kaggle.com/datasets/neomatrix369/learning-path-index-dataset).

### Ingesting files

#### via native shell CLI

```shell
python ingest.py
```

Output should look like this:

```shell
root@sai-XPS-15-9560:/home# python ingest.py 
Downloading (…)e9125/.gitattributes: 100%|███████████████████████████████████████████████████████████████████| 1.18k/1.18k [00:00<00:00, 2.07MB/s]
Downloading (…)_Pooling/config.json: 100%|████████████████████████████████████████████████████████████████████████| 190/190 [00:00<00:00, 378kB/s]
Downloading (…)7e55de9125/README.md: 100%|███████████████████████████████████████████████████████████████████| 10.6k/10.6k [00:00<00:00, 16.2MB/s]
Downloading (…)55de9125/config.json: 100%|███████████████████████████████████████████████████████████████████████| 612/612 [00:00<00:00, 1.53MB/s]
Downloading (…)ce_transformers.json: 100%|████████████████████████████████████████████████████████████████████████| 116/116 [00:00<00:00, 252kB/s]
Downloading (…)125/data_config.json: 100%|███████████████████████████████████████████████████████████████████| 39.3k/39.3k [00:00<00:00, 29.4MB/s]
Downloading pytorch_model.bin: 100%|█████████████████████████████████████████████████████████████████████████| 90.9M/90.9M [00:09<00:00, 9.11MB/s]
Downloading (…)nce_bert_config.json: 100%|█████████████████████████████████████████████████████████████████████| 53.0/53.0 [00:00<00:00, 97.4kB/s]
Downloading (…)cial_tokens_map.json: 100%|████████████████████████████████████████████████████████████████████████| 112/112 [00:00<00:00, 698kB/s]
Downloading (…)e9125/tokenizer.json: 100%|█████████████████████████████████████████████████████████████████████| 466k/466k [00:00<00:00, 5.22MB/s]
Downloading (…)okenizer_config.json: 100%|████████████████████████████████████████████████████████████████████████| 350/350 [00:00<00:00, 627kB/s]
Downloading (…)9125/train_script.py: 100%|███████████████████████████████████████████████████████████████████| 13.2k/13.2k [00:00<00:00, 21.1MB/s]
Downloading (…)7e55de9125/vocab.txt: 100%|█████████████████████████████████████████████████████████████████████| 232k/232k [00:00<00:00, 10.7MB/s]
Downloading (…)5de9125/modules.json: 100%|████████████████████████████████████████████████████████████████████████| 349/349 [00:00<00:00, 721kB/s]
Creating new vectorstore
Loading documents from source_documents
Loading new documents: 100%|██████████████████████| 2/2 [00:00<00:00, 40.44it/s]
Loaded 1414 new documents from source_documents
Split into 2214 chunks of text (max. 500 tokens each)
Creating embeddings. May take some minutes...
Ingestion complete! You can now run lpiGPT.py to query your documents
```

### Ask questions

#### via native shell CLI

```shell
python lpiGPT.py

Enter a query: Fetch me all machine learning courses of the advanced level from the Learning Path Index and show me results in a tabular form

Start time: 2023-10-07 16:14:18
> Question:
Fetch me all machine learning courses of the advanced level from the Learning Path Index and show me results in a tabular form
End time: 2023-10-07 16:17:19
Answer (took about 181.3118166923523 seconds):
| Course Name | Level | Type | Duration | Module / Sub-module | Keywords/Tags/Skills/Interests/Categories | Links |
|-------------------------------|--------|-------|----------|--------------------------------------|-----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|
1. Machine Learning Engineer Learning Path | Intermediate to Advanced | Free during mentorship period | AI Foundations: Quiz | Machine Learning/ Cloud/Data/Infrastructure/Bigquery/| https://www.cloudskillsboost.google/course_sessions/4968855/quizzes/387518
2. Machine Learning Engineer Learning Path | Intermediate to Advanced | Free during mentorship period | AI Development Workflow: Quiz | AI/Development/API/Vertex AI/MLOps/Workflow| https://www.cloudskillsboost.google/course_sessions/4968855/quizzes/387541
3. Machine Learning Engineer Learning Path | Intermediate to Advanced | Free during mentorship period | AI Development Options: Quiz | AI/Development/API/Vertex AI/AutoML/Workflow| https://www.cloudskillsboost.google/course_sessions/4968855/quizzes/387529
4. Machine Learning Engineer Learning Path | Intermediate to Advanced | Free during mentorship period | BigQuery Machine Learning: Develop ML Models Where Your Data Lives: Introduction | Big Query/Explanable AI/ML models/Hyperparameter. tuning/recommendation system| https://www.cloudskillsboost.google/course_sessions/4968855/quizzes/387530
Note: The results will be displayed in a table format with columns for Course Name, Level, Type, Duration, Module / Sub-module, Keywords/Tags/Skills/Interests/Categories and Links.

.
.
.
[A list of source documents it got the results from]
.
.
.

```

To exit the GPT prompt, press Ctrl-C or Ctrl-D and it will return to the Linux/Command-prompt.

### Known issues

When trying to ingest and also run the GPT app, we can get this error on system with Python 3.10 or older:

```python
RuntimeError: Your system has an unsupported version of sqlite3. Chroma requires sqlite3 >= 3.35.0.
```

If this occurs then use the Docker container to run your commands, instructions are given below under each sub-section.

#### via Docker container

```shell
cd docker
./build-docker-image.sh
```

when finished with building the container run the below

```shell
./run-docker-container.sh
```

you will get a prompt like this:

```shell
root@[your machine name]:/home#:
```

in there, type the same commands as in the **via native shell CLI** sections of [Ingesting files](#ingesting-files) and [Ask questions](#ask-questions) respectively.


### Try a different model:

```
ollama pull llama2:13b
MODEL=llama2:13b python lpiGPT.py
```

## Adding more files

Put any and all your files into the `source_documents` directory

The supported extensions are:

- `.csv`: CSV
and others, we have trimmed them off from here to keep this example simple and concise.