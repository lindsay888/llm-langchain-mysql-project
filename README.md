# Langchain LLM SQL Query Generator
## Purpose
To simplify database data extraction by non technical users through asking questions in English language to the database. This project uses LangChain as a framework which connects database (MySQL) to Large Language Model (OpenAI ChatGPT 3.5). LLM then translates questions to SQL Query, query the database, and answer in human's preferred responses (table or human natural language). 

This particular project uses MySQL database and OpenAI ChatGPT, which can be replaced with any databases and other LLMs with the necessary connections in Python Package. 

## Pre-requisite
Before you begin, ensure you have met the following requirements:

1. Python 3.10 or higher
2. An active OpenAI API key
3. MySQL database with tables (The current ones are from this public dataset on Kaggle: https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
4. Virtual environment to run the code in

## Installation Required
Clone the repository and install the dependencies:
```
git clone https://github.com/lindsay888/llm-langchain-mysql-project
cd llm-langchain-mysql-project
pip install -r requirements.txt
```

## Usage
To use the SQL Query Generator, follow these steps:
Edit your OpenAI API key in the following block of code:
```
import os
os.environ['OPENAI_API_KEY'] = [EDIT WITH YOUR API KEY]
```
If you are using MySQL database, edit the block of code here on the db_uri section:
```
db_uri = "mysql+mysqlconnector://[your username]:[your password]@[server location]/[database project name]"
```

## Explanation
1. Using Chain based. 
   LangChain minimally consists of prompt, llm, and parser. In our case, we are going to use templatised input / prompt based on variable denoted by {...}. Hence, we will use RunnablePassthrough to pass through the prompt filled with template and to pass through the database schema for sql query generation. LangChain also use LCEL for its notation, with the associated "|" to separate each element. Each chain created will be using .invoke to start the query. 
   
   The architecture of the LangChain works from Prompt -> LLM parse and run to provide SQL query -> add to the another prompt to query database to get the data -> LLM parsing to return the result depends on what is indicated format in response_type.
   
   There are 2 chains we use in this project: 
   1. sql_chain
      This chain is to generate the SQL Query from the user's question.
      When invoked, this chain will require the question to be passed through dictionary format of
      ```
      sql_chain.invoke({'question':'[type your q here]', 'expected_column':'','response_type': 'human natural language'})
      ```
   2. full_chain
      This chain is to generate response based on the SQL Query generated from sql_chain. It is also to customise the response_type to be either table with specific format result or natural language result.
       When invoked, this chain will require the question and response_type to be passed through dictionary format of
       ```
          full_chain.invoke({'question':'[type your q here]', 'expected_column':'','response_type': 'human natural language'})
       ```
   
   We also use json_output_parser function to parse the result from the LLM into dataframe. 
   ```
   def json_output_parser(json_result):
       clean_json_result = json.loads(json_result.content.replace("json","").replace("```",""))
   
       #check for nested dictionary
       try:
           any(isinstance(i[0],dict) for i in clean_json_result.values())
           outer_key = list(clean_json_result.keys())[0]
           df = pd.DataFrame(clean_json_result[outer_key])
       except:
           df = pd.DataFrame(clean_json_result)
       return df
   ```
   
   There are many other output parser that you can play around with. Check out more from the documentation [here](https://python.langchain.com/docs/modules/model_io/output_parsers/)
2. Agent based
   There is out of the box agent based sql without building our own chain. From my experience playing around, this takes a lot slower as compared to the chain based query.

## Contributing
Contributions to the Langchain LLM SQL Query Generator are welcome! If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".

Don't forget to give the project a star! Thanks again!

## Contact
You can contact me at yepunyoja@gmail.com for a shout out on this project :)
