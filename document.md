# Document


## Google Cloud vision

Please check: [Online manual](https://cloud.google.com/vision/docs/features-list)




## Google Cloud NLP



### Classifying Content

**Content Classification** analyzes a document and returns a list of content categories that apply to the text found in the document. To classify the content in a document, call the `classifyText` method.

A complete list of content categories returned for the `classifyText` method are found [here](https://cloud.google.com/natural-language/docs/categories).

**Important:** You must supply a text block (document) with at least twenty tokens (words) to the  `classifyText` method.

This section demonstrates how to classify content in a document. For each document, you must submit a separate request.


Here is an example of classifying content provided as a string:

```python
from google.cloud import language
from google.cloud.language import enums
from google.cloud.language import types

def sample_classify_text(text_content):
    """
    Classifying Content in a String
    Args:
      text_content The text content to analyze. Must include at least 20 words.
    """

    client = language.LanguageServiceClient()

    # text_content = 'That actor on TV makes movies in Hollywood and also stars in a variety of popular new TV shows.'

	  # Available types: PLAIN_TEXT, HTML
    document = types.Document(content=text_content,type=enums.Document.Type.PLAIN_TEXT)
    response = client.classify_text(document)
    # Loop through classified categories returned from the API
    for category in response.categories:
        # Get the name of the category representing the document.
        # See the predefined taxonomy of categories:
        # https://cloud.google.com/natural-language/docs/categories
        print(u"Category name: {}".format(category.name))
        # Get the confidence. Number representing how certain the classifier
        # is that this category represents the provided text.
        print(u"Confidence: {}".format(category.confidence))
```



### Analyzing Entities

**Entity Analysis** inspects the given text for known entities (proper nouns such as public figures, landmarks, etc.), and returns information about those entities. Entity analysis is performed with the `analyzeEntities` method. For information about the types of entities Natural Language identifies, see the [Entity](https://cloud.google.com/natural-language/docs/reference/rest/v1/Entity#Type) documentation. For information on which languages are supported by the Natural Language API, see [Language Support](https://cloud.google.com/natural-language/docs/languages).

This section demonstrates how to detect entities in a document. For each document, you must submit a separate request.

Here is an example of performing entity analysis on a text string sent directly to the Natural Language API:

```python
from google.cloud import language
from google.cloud.language import enums
from google.cloud.language import types

def sample_analyze_entities(text_content):
    """
    Analyzing Entities in a String
    Args:
      text_content The text content to analyze. Must include at least 20 words.
    """

    client = language.LanguageServiceClient()

    # text_content = 'That actor on TV makes movies in Hollywood and also stars in a variety of popular new TV shows.'

	  # Available types: PLAIN_TEXT, HTML
    document = types.Document(content=text_content,type=enums.Document.Type.PLAIN_TEXT)
    response = client.analyze_entities(document)
    # Loop through entitites returned from the API
    for entity in response.entities:
        print(u"Representative name for the entity: {}".format(entity.name))

        # Get entity type, e.g. PERSON, LOCATION, ADDRESS, NUMBER, et al
        print(u"Entity type: {}".format(language_v1.Entity.Type(entity.type_).name))

        # Get the salience score associated with the entity in the [0, 1.0] range
        print(u"Salience score: {}".format(entity.salience))

        # Loop over the metadata associated with entity. For many known entities,
        # the metadata is a Wikipedia URL (wikipedia_url) and Knowledge Graph MID (mid).
        # Some entity types may have additional metadata, e.g. ADDRESS entities
        # may have metadata for the address street_name, postal_code, et al.
        for metadata_name, metadata_value in entity.metadata.items():
            print(u"{}: {}".format(metadata_name, metadata_value))

        # Loop over the mentions of this entity in the input document.
        # The API currently supports proper noun mentions.
        for mention in entity.mentions:
            print(u"Mention text: {}".format(mention.text.content))

            # Get the mention type, e.g. PROPER for proper noun
            print(
                u"Mention type: {}".format(language_v1.EntityMention.Type(mention.type_).name)
            )

    # Get the language of the text, which will be the same as
    # the language specified in the request or, if not specified,
    # the automatically-detected language.
    print(u"Language of the text: {}".format(response.language))
```





### Analyzing Sentiment

**Sentiment Analysis** inspects the given text and identifies the prevailing emotional opinion within the text, especially to determine a writer's attitude as positive, negative, or neutral. Sentiment analysis is performed through the `analyzeSentiment` method. For information on which languages are supported by the Natural Language API, see [Language Support](https://cloud.google.com/natural-language/docs/languages). For information on how to interpret the `score` and `magnitude` sentiment values included in the analysis, see [Interpreting sentiment analysis values](https://cloud.google.com/natural-language/docs/basics#interpreting_sentiment_analysis_values).

This section demonstrates how to detect sentiment in a document. For each document, you must submit a separate request.

Here is an example of performing sentiment analysis on a text string sent directly to the Natural Language API:

```python
from google.cloud import language
from google.cloud.language import enums
from google.cloud.language import types

def sample_analyze_entities(text_content):
    """
    Analyzing Entities in a String
    Args:
      text_content The text content to analyze. Must include at least 20 words.
    """

    client = language.LanguageServiceClient()

    # text_content = 'That actor on TV makes movies in Hollywood and also stars in a variety of popular new TV shows.'

	  # Available types: PLAIN_TEXT, HTML
    document = types.Document(content=text_content,type=enums.Document.Type.PLAIN_TEXT)
    response = client.analyze_sentiment(document)
    # Get overall sentiment of the input document
    print(u"Document sentiment score: {}".format(response.document_sentiment.score))
    print(
        u"Document sentiment magnitude: {}".format(
            response.document_sentiment.magnitude
        )
    )
    # Get sentiment for all sentences in the document
    for sentence in response.sentences:
        print(u"Sentence text: {}".format(sentence.text.content))
        print(u"Sentence sentiment score: {}".format(sentence.sentiment.score))
        print(u"Sentence sentiment magnitude: {}".format(sentence.sentiment.magnitude))

    # Get the language of the text, which will be the same as
    # the language specified in the request or, if not specified,
    # the automatically-detected language.
    print(u"Language of the text: {}".format(response.language))
```
