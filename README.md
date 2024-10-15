PDF Querying with Langchain and Ollama
This repository demonstrates how to extract text from a PDF and query the extracted content using an LLM (Large Language Model) from Ollama, integrated with the Langchain framework. This allows users to retrieve specific information from PDFs, such as expense data, in an intuitive and automated way.

Requirements
Before running the code, ensure that you have the following Python packages installed:

bash
Copy code
pip install PyPDF2 langchain ollama
Functionality Overview
1. PDF Text Extraction
The extract_text_from_pdf() function extracts the textual content from a PDF file using the PyPDF2 library.

python
Copy code
import PyPDF2

def extract_text_from_pdf(pdf_path):
    text = ""
    with open(pdf_path, "rb") as f:
        pdf_reader = PyPDF2.PdfReader(f)
        for page_num in range(len(pdf_reader.pages)):
            page = pdf_reader.pages[page_num]
            text += page.extract_text()
    return text
Input: Path to a PDF file.
Output: A string containing the extracted text from the PDF.
2. Querying the Extracted Text Using Langchain and Ollama
Once the text is extracted from the PDF, the query_pdf() function allows you to ask questions about the content, using Ollama's language models and Langchain's question-answering chains.

python
Copy code
from langchain.llms import Ollama
from langchain.chains.question_answering import load_qa_chain
from langchain.prompts import PromptTemplate
from langchain.docstore.document import Document

# Initialize Ollama LLM
llm = Ollama(model="llama3")  # Ensure that Ollama supports the selected model

# Load the question-answering chain
qa_chain = load_qa_chain(llm, chain_type="stuff")  # "stuff" is a basic chain type

def query_pdf(pdf_text, query):
    # Create a Document object from the extracted text
    doc = Document(page_content=pdf_text)
    
    # Run the question-answering chain
    response = qa_chain.run(input_documents=[doc], question=query)
    return response
Input:
pdf_text: The text extracted from the PDF.
query: The question you want to ask about the PDF content.
Output: A response from the LLM, which answers your query based on the PDF content.
3. Example Usage
In this example, we extract text from a PDF file (sample_pdf_expense.pdf), then query the total expense by category:

python
Copy code
# Example usage
pdf_path = "sample_pdf_expense.pdf"
pdf_text = extract_text_from_pdf(pdf_path)
query = "Total expense category wise"

response = query_pdf(pdf_text, query)
print("Query :-", query)
print("**********************")
print(response)
Sample Output:
bash
Copy code
Query :- Total expense category wise
**********************

