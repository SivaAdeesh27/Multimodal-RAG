# Multimodal-RAG

## DocRAG: Document Retrieval Augmented Generation with Images

This project is a Gradio-based interface that answers user queries using a Retrieval-Augmented Generation (RAG) approach. It retrieves relevant documents, generates concise responses, and displays related images. The system utilizes a combination of the LangChain library and the ChatGroq model to perform document retrieval and question-answering tasks. Below is a breakdown of the main components of the project and how they function.

## Key Components

### 1. **Document Processing**
The `process_documents` function handles the core logic of retrieving documents and generating a response based on user queries. The function involves several key steps:

#### 1.1 **Generating Search Queries**
- A **ChatPromptTemplate** is used to generate multiple search queries based on the original query. The assistant generates 4 related search queries, which improves the scope of document retrieval.
- This is done by chaining templates through **LangChain** and the **ChatGroq** model (using a versatile LLaMA-3.1 model).
- The generated queries are split into individual lines.

#### 1.2 **Reciprocal Rank Fusion (RRF)**
- **Reciprocal Rank Fusion** is a ranking algorithm that fuses the rankings of multiple search results.
- It takes multiple lists of retrieved documents (from different search queries) and combines them into a single list based on the ranks of documents across the lists.
- Documents that appear in multiple results with high ranks are given higher importance.
  
#### 1.3 **Answering Questions Based on Context**
- After retrieving and ranking the documents, the system generates a concise answer based on the context from the top-ranked documents.
- A specific prompt template is used to instruct the model to answer as concisely as possible and append "thanks for asking!" to the response.
- The **ChatGroq** model is used here to process the prompt and generate the final answer.

#### 1.4 **Displaying Relevant Images**
- The project retrieves image paths from the document metadata. 
- It checks if the document contains an `image_path` and appends it to the list of images for display.
- These images are then displayed as part of the output in the Gradio interface.

### 2. **Gradio Interface**
The project uses **Gradio** to create a user-friendly interface where users can input their queries and get both text-based answers and image outputs.

#### 2.1 **User Input**
- The user provides a query through a textbox in the Gradio interface. This query is passed to the `gradio_interface` function.

#### 2.2 **Document Retrieval and Response Generation**
- The `gradio_interface` function invokes the document retrieval process using a vector database retriever (`vectordb.as_retriever(k=4)`). This fetches the top 4 most relevant documents.
- The function then processes the documents, generates an answer, and extracts any related images.

#### 2.3 **Image Display**
- The images extracted from the documents are opened using the **PIL** library and displayed using the Gradio **Gallery** component.
- If an image cannot be found, the system displays an error message indicating the missing image.

### 3. **Key Libraries and Technologies**
The project relies on several important libraries:
- **LangChain**: Provides the framework for chaining together prompts and models.
- **ChatGroq**: A powerful language model (LLaMA-3.1) used for generating queries and answers.
- **Gradio**: Used to create an interactive interface that accepts user input and displays both text and images.
- **PIL (Python Imaging Library)**: For loading and handling images.
- **Matplotlib**: Provides the option to visualize data, though it is not used directly in this code.

Add the secrets for the colab from colab_secrets.txt and image_descriptions_gemini.csv contains all the image descriptions and its metadata from pdf file(only vits.pdf), add it to the colab and as well as the vits.pdf also.

## Detailed Workflow
1. **Input Query**: The user enters a query into the Gradio textbox.
2. **Query Generation**: The system generates multiple related search queries using the input query.
3. **Document Retrieval**: These search queries are used to retrieve documents from a vector-based retriever.
4. **Rank Fusion**: The retrieved documents are ranked using the Reciprocal Rank Fusion algorithm.
5. **Answer Generation**: The system generates a concise answer based on the context of the top-ranked documents.
6. **Image Extraction**: The system checks for any images within the top documents and prepares them for display.
7. **Output Display**: The Gradio interface displays the answer and any associated images.

## Key Functions

### `process_documents(original_query, retriever)`
- Handles the document retrieval process.
- Generates search queries and fuses the results from multiple queries.
- Produces a ranked list of documents and answers the user’s question based on the context.

### `reciprocal_rank_fusion(results: list[list], k=60)`
- Implements the Reciprocal Rank Fusion algorithm.
- Combines multiple document result lists into a single ranked list based on document scores.

### `answer_question_based_on_context(res, original_query)`
- Generates a concise answer from the top 3 ranked documents.
- Uses the **ChatGroq** model to process the context and generate an appropriate response.

### `display_images(documents)`
- Extracts image paths from document metadata and loads them for display in the Gradio interface.

## Gradio Interface Components

### `gradio_interface(original_query)`
- The main function that connects the query input to the document processing logic.
- It returns the generated answer and the extracted images.

### Gradio UI Components
- **Textbox**: For user input.
- **Textbox**: Displays the generated text-based answer.
- **Gallery**: Displays images retrieved from the documents.

## Conclusion
DocRAG is a versatile system that combines advanced language models, document retrieval techniques, and image processing to provide rich, informative responses to user queries. The integration with Gradio makes the system interactive and user-friendly, allowing users to both visualize images and obtain text-based answers in one interface.
