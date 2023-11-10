# Vector IO

Use the universal VDF format for vector datasets to easily export and import data from all vector databases.

Universal Vector Dataset Format (VDF) specification: https://docs.google.com/document/d/1SaZ0nsBw8ZZCCcPXoc2nwTY5A3KBJkTEnmZZvFxHAu4/edit#heading=h.32if60hafsdt

Motivation
Each vector database has their own way of storing vectors and metadata, making it hard for people to transfer a dataset from one into the other.
Existing Alternatives:
Qdrant (https://qdrant.tech/documentation/cloud/backups/) and Weaviate (https://weaviate.io/developers/weaviate/configuration/backups) have their own backup formats, which are not portable to other DBs.
Pinecone has a datasets library: https://pinecone-io.github.io/pinecone-datasets/pinecone_datasets.html but it is not used outside of their demo public datasets. Their backups are stored on their own servers, with their own proprietary format, without the ability to move the data out of their cluster.
Txt-ai: they have a proprietary format that can only be loaded via their library: https://huggingface.co/NeuML/txtai-wikipedia/tree/main. It was used for just one wikipedia embeddings dump.
Cohere (https://huggingface.co/datasets/Cohere/wikipedia-22-12-simple-embeddings) released their model's wikipedia embeddings in an ad-hoc schema in parquet format.
Macrocosm/Alexandria: They provide various embedding dumps. They distribute them as zip files, containing multiple parquet files. The parquet file contains both the embedding as well as metadata like title and doi. They provide an ad-hoc script to read the data, and a params.txt file to record the version and the model+prompt used.

**Vector DB companies don't have an incentive to create an interoperable format for vector datasets, but the community will benefit from it (avoid vendor lock-in).
**

A collection of utility functions and scripts to import and export vector datasets between various vector databases.

The representation that we export to is:
1. parquet file for the vectors (to be changed to Parquet)
2. SQLite for metadata

Feel free to send a PR to add an import/export implementation for your favorite vector DB.

## Export

    Export data from Pinecone, Weaviate and Qdrant to sqlite database and parquet file.

    Usage:
        python export.py <vector_database> [options]

    Arguments:
        vector_database (str): Choose the vectors database to export data from.
            Possible values: "pinecone", "weaviate", "qdrant".

    Options:
        Pinecone:
            -e, --environment (str): Environment of Pinecone instance.
            -i, --index (str): Name of index to export.

        Weaviate:
            -u, --url (str): Location of Weaviate instance.
            -c, --class_name (str): Name of class to export.
            -i, --include_crossrefs (bool): Include cross references, set Y or N.

        Qdrant:
            -u, --url (str): Location of Qdrant instance.
            -c, --collection (str): Name of collection to export.

    Examples:
        Export data from Pinecone:
        python export.py pinecone -e my_env -i my_index

        Export data from Weaviate:
        python export.py weaviate -u http://localhost:8080 -c my_class -i Y

        Export data from Qdrant:
        python export.py qdrant -u http://localhost:6333 -c my_collection

## Import

WIP
