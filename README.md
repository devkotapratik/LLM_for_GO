## Large Language Models for Gene Ontology (GO)
***
Ontologies are formal ways or techniques to represent and share knowledge about a particular domain by modeling objects in the domain and their relationships. Agreement on a particular ontological representation allows domain experts to use common vocabulary to describe, store and analyze data. They also facilitate in easier handling of knowledge computationally. Among the different ontologies that exists today, <strong>Gene Ontology (GO)</strong> is the most comprehensive and widely used ontology in the field of biology and bioinformatics.

Concerning the function of genes, the knowledge base is both human-readable and machine-readable and is a foundation for computational analysis of large-scale molecular biology and genetics experiments in biomedical research. GO describes the knowledge of the biological domain with respect to three aspects: `Biological Process (BP)`, `Molecular Function (MF)`, and `Cellular Component (CC)`.

To learn more about GO, visit [Gene Ontology Resource](http://geneontology.org).


##  Colorado Richly Annotated Full Text Corpus (CRAFT)
***
CRAFT is an open-source corpus, semantically and syntactically annotated to serve as a research resource for NLP tasks. Version
5.0.2 provides a collection of 97 full-length, open-access biomedical journal articles from PubMed Central Open Access Subset. The collection contains ontologies with different classes organized across 10 different modules.

For all our experiments, we use CRAFT v5.0.2 as a gold standard corpus for fine-tuning and evaluation. Our work is geared towards identifying GO annotations since GO is the most widely used biological ontology. For Gene Ontology, CRAFT has 97 articles, each article with 1 or more `xml` annotation files which describes annotations within the sentences using character indexes of the
article. To learn more about CRAFT, visit [CRAFT github repository](https://github.com/UCDenver-ccp/CRAFT).

Preprocessing of the data in CRAFT is required to make it model ready for training/ finetuning. This work uses the already preprocessed dataset available from our previous experiments. To understand the preprocessing steps in detail, head over to [NEMO](https://github.com/prashanti/deeplearningNER).

### Finetuning LLAMA models
***
Unlike many other Large Language Models (LLMs) available in HuggingFace, LLAMA weights are not publicly available. To obtain the weights for the LLAMA models, it is required to fill out [this form](https://docs.google.com/forms/d/e/1FAIpQLSfqNECQnMkycAp2jP4Z9TFX0cGR4uf7b_fBxjY_OjhJILlKGA/viewform?usp=send_form) to request access to the models at Meta AI. Once you are granted access, you will receive a presigned url and instruction to download the weights. Follow the instruction to download the weights to the `MODELS/Meta/Llama` directory. Models ranging from 7B to 65B parameters would have their weights in their respective directory. The directories should appear like below:
```bash
LLM_for_GO/
├── data
└── MODELS/
    └── Meta/
        └── Llama/
            ├── 7B/
            │   ├── checklist.chk
            │   ├── consolidated.00.pth
            │   └── params.json
            ├── 13B/
            │   └── ...
            ├── 30B/
            │   └── ...
            ├── 65B/
            │   └── ...
            ├── tokenizer.model
            └── tokenizer_checklist.chk
```

Execute the python script [convert_llama_weights_to_hf.py](./convert_llama_weights_to_hf.py) provided by HuggingFace to convert the Llama weights to HuggingFace Transformers format using the following command:
```
python convert_llama_weights_to_hf.py --input_dir /MODELS/Meta/Llama --model_size 7B --output_dir /MODELS/HF/Llama/7B
```
Change the model_size and output_dir accordingly.

Once the conversion is complete, launch [finetune_llama.ipynb](./finetune_llama.ipynb) notebook to finetune the llama model for our annotation task.
