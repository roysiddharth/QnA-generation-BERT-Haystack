# Question and Answers Generation Pipeline with BERT Transformers using Haystack Library
In this project, I have implemented the QNA generation features that Haystack library offers after scraping the top results to a query from Google search results.

# Author
[Siddhartha Roy](https://github.com/roysiddharth)

# Libraries Used
- Haystack from Deepset.AI
- Google Search API
- pandas
- Regex
- requests
- BeautifulSoup4
- os
- unicodedata

# Web Scraping using Google Search API
We implemented the Google Search API to get the top 10 results from Google search to the query:
> "What is Blockchain?"

# Working with Haystack
- `TextConverter`: We use the `TextConverter` to convert the text file contents into a format that is recognised by the Haystack preprocessor.
- `PreProcessor`: We import the `PreProcessor` library to perform necessary preprocessing tasks on the document such as splitting by words (can be sentences or paragraphs as well), cleaning whitespaces, setting split length and overlap args.
- `InMemoryDocumentStore`: We had first tried to implement the `ElasticSearchDocumentStore` but it was an unsuccessful attempt as Docker was not running up to standards for us. As a result, considering the relatively smaller size of our data, we have used the `InMemoryDocumentStore` instead.
- Defining the `Retriever`:
>The Retriever is a lightweight filter that can quickly go through the full document store and pass on a set of candidate documents that are relevant to the query. When used in combination with a Reader, it is a tool for sifting out irrelevant documents, saving the Reader from doing more work than it needs to and speeding up the querying process.

Here we have used the `DensePassageRetriever` with arguments for `query_embedding_model`, `passage_embedding_model`, boolean `True` for `use_gpu` and `embed_title`.

- Updating Retriever Embeddings: This is one simple line of code that basically does the text vectorization in our document store as well as the query. It is used to convert large amounts of text contents into vectors, in the context of semantic search systems. This process take time because each document in the document store is passed through the transformer model.
- Defining the `Seq2SeqGenerator`: The `Seq2SeqGenerator` is a Long Form Question Answering Generator that provides full length answers to queries, some of which actually make sense.
- We have used a `GenerativeQAPipeline` with the generator (defined earlier) and retriever (also defined earlier) to streamline the process through a pipeline.

# Results
> Q: What is Blockchain? <br> A: Blockchain is a way of storing information in a way that can't be altered or destroyed. For example, let's say you have a bank account, and you want to transfer money from one bank account to another. You want to make sure that when you send the money to the other bank, it goes to the correct bank. However, you don't want the bank to know that you sent it to the wrong bank. So, you make a list of all the bank accounts that you have, and then you put them all in a database. This database is called a blockchain. When a new bank account is created, it is added to the list of bank accounts in the database, and when a new transaction is made, it's added to that list, and so on.

As is evident, the answer that has been generated is quite apt and can be applied readily to real world scenarios.

# Plans for the Future
We plan to transform this into an end-to-end app that will serve as dashboard for an adequate Question Answering bot.



