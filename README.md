# Langchain LLM SQL Query Generator
## Purpose
To simplify database data extraction by non technical users through asking in English language. This project uses LangChain as a framework which connects database (MySQL) to Large Language Model (OpenAI ChatGPT 3.5). LLM then translates questions to SQL Query, query the database, and answer in human's preferred responses (table or human natural language). 

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


## Contributing
Contributions to the Langchain LLM SQL Query Generator are welcome! If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".

Don't forget to give the project a star! Thanks again!

## Contact
You can contact me at yepunyoja@gmail.com for a shout out on this project :)
