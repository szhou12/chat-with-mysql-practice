# Chat With MySQL Database Using LangChain & OpenAI API
[![badge openai-api](https://badgen.net/badge/icon/openai-api?color=green&label)](https://openai.com/)
[![badge langchain](https://badgen.net/badge/icon/langchain?color=yellow&label)](https://www.langchain.com/)
[![badge mysql](https://badgen.net/badge/icon/mysql?color=blue&label)](https://www.mysql.com/)

*Disclaimer*: This is a practice project that follows the tutorial [Chat with MySQL Database with Python | LangChain Tutorial](https://www.youtube.com/watch?v=9ccl1_Wu24Q&t=1203s&ab_channel=AlejandroAO-Software%26Ai) made by Alejandro AO.


# Table of Contents
* [Step 1: Create A MySQL Database](#step-1-create-a-mysql-database)
* [Step 2: Create A Conda Virtual Environment](#step-2-create-a-conda-virtual-environment)
  * [Step 2.1: Install Python Packages Within Environment](#step-21-install-python-packages-within-environment)
* [Step 3: Create SQL Chain Prompt](#step-3-create-sql-chain-prompt)

## Step 1: Create A MySQL Database
0. Skip SQLite section. Step 1 is done in Terminal.
1. Log in to MySQL and enter password.
    ```
    mysql -u root -p
    ```
    - `-u`: stands for "user". It specifies the username to use when connecting to the MySQL server.
    - `root`: The value follows `-u`. It represents the username you're using to log in to the MySQL server. 你以什么身份登陆MySQL.
    - `-p`: stands for "password". It tells MySQL to prompt you to enter the password for the `root` user account.
2. Check all stored databases.
    ```
    mysql> show databases;
    ```
3. Create a new database.
    ```
    CREATE DATABASE chinook;
    USE chinook;
    SOURCE chinook.sql; --or the name of SQL file to load the database
    ```
    - `CREATE DATABASE <DB-name>;`: create a new database using the specified name.
    - `USE <DB-name>;`: switch the context to the specified database. 进入指定的数据库. You can also use this when you are currently in a database "A" but want to switch to another database "B" (`USE b;`).
    - `SOURCE <absolute-path-SQL-file>;`: load the specified SQL file to the database. Note: provide the absolute file path. You can do this by dragging the file into the terminal. After dragged, the file may be quoted (`'<absolute-path-SQL-file>'`). You need to remove quotes (`<absolute-path-SQL-file>`).
4. Test the database is successfully loaded.
    ```
    SHOW tables;
    +-------------------+
    | Tables_in_chinook |
    +-------------------+
    | Album             |
    | Artist            |
    | Customer          |
    | Employee          |
    | Genre             |
    | Invoice           |
    | InvoiceLine       |
    | MediaType         |
    | Playlist          |
    | PlaylistTrack     |
    | Track             |
    +-------------------+
    SELECT * FROM artist LIMIT 10;
    ```
5. Check MySQL Port: (Helpful for Step 4)
    ```
    SHOW VARIABLES WHERE Variable_name = 'port';
    ```
    OR
    ```
    show variables like 'port';
    ```
6. Log out of MySQL.
    ```
    mysql> quit;
    ```
## Step 2: Create A Conda Virtual Environment
1. Create a new conda environment for this project. Specify the Python version to be 3.10.
    ```
    conda create --name chat-with-mysql-practice python=3.10
    ```
2. Activate the environment.
    ```
    conda activate chat-with-mysql-practice
    ```
3. Quit the environment.
    ```
    conda deactivate
    ```
4. Other useful commands
    - List all conda environments.
    ```
    conda env list
    ```
    - Remove an environment.
    ```
    conda env remove --name <env-name>
    conda env remove                     // if you're currently activated within the environment
    conda remove --name <env-name> --all // equivalent method to conda env remove --name <env-name>
    ```
    - Uninstall a specific package from a Conda environment
    ```
    conda remove --name <env-name> <package-name>
    conda remove <package-name>         // if you're currently activated within the environment
    ```
### Step 2.1: Install Python Packages Within Environment
1. Activate the environment `chat-with-mysql-practice` and within the enviroment, install two packages: `langchain`, `mysql-connector-python`
    ```
    pip install langchain mysql-connector-python
    ```
2. Check the installed verison of LangChain.
    ```
    pip freeze | grep langchain
    ```
    - `pip freeze`: Generate a list of all installed Python packages and their versions in the current environment.
    - `|`: Pipe operator. It takes the output on the left side and feeds it as input to the right side.
    - `grep langchain`: Regular expression matching. Find all packages containing the keyword `langchain`.

## Step 3: Create SQL Chain Prompt
1. Import the required class
    ```
    from langchain_core.prompts import ChatPromptTemplate
    ```
    - import the `ChatPromptTemplate` class from the `langchain_core.prompts` module.
2. Define the template string
    ```
    template = """
    XXX {schema} XXX
    """
    ```
    - `template`: prompt template that will be presented to the language model.
    - `{schema}`: placeholder. It will be replaced by the actual content specified in `.format(schema=...)`
3. Create an instance of ChatPromptTemplate
    ```
    prompt = ChatPromptTemplate.from_template(template) # deprecated
    prompt = ChatPromptTemplate.from_messages([template]) # recommended
    ```
    - create an instance of `ChatPromptTemplate` using the predefined `template` string. 
    - Per documentation, `from_template` is deprecated. Recommend using `from_messages` instead.
    - Note: `from_messages` takes a list of strings as input. So put the `template` string in a list `[]`.
4. Fill up placeholders
    ```
    prompt.format(schema="my schema", question="how many users are there?")
    ```
    - replace placeholders with actual content specified in args of `.format()`.

## Step 4: Load MySQL Database in Python
![db_uri (1)](https://github.com/szhou12/chat-with-mysql-practice/assets/35708194/81376efc-5ead-4b0d-8cba-0bd6225f04f8)


## Resources
- [Chat with MySQL Database with Python | LangChain Tutorial](https://www.youtube.com/watch?v=9ccl1_Wu24Q&t=1203s&ab_channel=AlejandroAO-Software%26Ai)
- [Chat With a MySQL Database Using Python and LangChain](https://alejandro-ao.com/chat-with-mysql-using-python-and-langchain/)
- [lerocha/chinook-database](https://github.com/lerocha/chinook-database)
- [Add Badges to GitHub | Shields IO](https://www.youtube.com/watch?v=uUalQbg-TGA&ab_channel=HariReddy)
