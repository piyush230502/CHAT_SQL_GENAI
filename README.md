# LangChain and Streamlit-Based SQL Database Interaction Application

## Introduction
This project demonstrates a modern approach to interacting with SQL databases using natural language queries through an intuitive web interface. By integrating **LangChain**,
**Streamlit**, and **Groq AI**, the application enables users to ask questions in plain English and receive structured answers derived directly from database queries.
The solution supports both SQLite (local) and MySQL (remote) databases, making it versatile for different deployment scenarios.

## Project Overview
The application consists of two main components:
1. **Database Setup (`sqlite.py`)**: Creates a sample SQLite database with student records.
2. **Streamlit Web App (`app.py`)**: Provides a user interface to connect to databases and process natural language queries using LangChain's SQL agent.

### Key Technologies
- **Streamlit**: For building the interactive web interface.
- **LangChain**: To create an SQL agent that translates natural language to SQL queries.
- **Groq AI**: As the LLM (Large Language Model) for processing user queries.
- **SQLite/MySQL**: Supported databases for querying.

## Component Breakdown

### 1. Database Setup (`sqlite.py`)
This script initializes a SQLite database named `student.db` and populates it with sample data. The table structure includes fields for student names, class, section, and marks.

#### Key Steps:
- **Table Creation**: A `STUDENT` table is created with columns for `NAME`, `CLASS`, `SECTION`, and `MARKS`.
- **Data Insertion**: Sample records are inserted to demonstrate querying capabilities.
- **Output**: The script prints all records to verify successful setup.

### 2. Streamlit Web Application (`app.py`)
The core application provides a user-friendly interface to interact with the database. It supports two database modes: **local SQLite** and **remote MySQL**.

#### User Interface Features:
- **Sidebar Options**: 
  - Toggle between SQLite and MySQL modes.
  - Input fields for MySQL credentials (host, user, password, database name).
  - API key input for Groq authentication.
- **Chat Interface**: 
  - Real-time conversation history.
  - Clear message history button.
  - Streaming responses during query processing.

#### Technical Implementation:
- **Database Configuration**:
  - SQLite mode uses a read-only connection to the local `student.db`.
  - MySQL mode constructs a connection string using user-provided credentials.
- **LangChain Agent**:
  - `create_sql_agent` is configured with Groq as the LLM.
  - The agent uses `SQLDatabaseToolkit` to interact with the database.
  - Queries are executed in real-time, with results streamed to the user.

## Workflow Explanation

### Step 1: Database Selection
Users choose between SQLite (default) or MySQL via the sidebar. If MySQL is selected, additional fields for credentials appear.

### Step 2: Authentication
- **Groq API Key**: Required to authenticate with Groq's LLM service.
- **MySQL Credentials**: Host, user, password, and database name must be valid for remote connections.

### Step 3: Query Processing
1. **User Input**: A natural language question (e.g., "What is the average mark in Data Science?").
2. **Agent Translation**: LangChain's SQL agent converts the question into a valid SQL query.
3. **Database Execution**: The query runs against the selected database.
4. **Response Streaming**: Results are displayed incrementally using `StreamlitCallbackHandler`.

## Technical Deep Dive

### LangChain SQL Agent
The agent uses a **zero-shot react description** approach, meaning it dynamically determines the best tools (SQL queries) to answer questions without prior training.
The `SQLDatabaseToolkit` provides the agent with SQL execution capabilities.

### Groq Integration
Groq's `ChatGroq` model is configured with streaming enabled, allowing real-time response updates. The API key is securely handled via Streamlit's input field.

### Streamlit Optimization
- **Caching**: `st.cache_resource` decorates the database configuration function to avoid redundant setup.
- **Session State**: Manages conversation history to maintain context between interactions.

### Security Considerations
- **API Keys**: Groq credentials are input via a password field and not stored persistently.
- **Database Connections**: MySQL credentials are validated before establishing a connection.
- **Read-Only Mode**: SQLite is opened in read-only mode to prevent accidental data modification.
  
### How to Run the Project

# Prerequisites
Before running the project, ensure you have the following installed:
- Python 3.8 or higher
- Streamlit (`pip install streamlit`)
- LangChain (`pip install langchain`)
- SQLite3 (`pip install sqlite3`)
- SQLAlchemy (`pip install sqlalchemy`)
- MySQL Connector (if using MySQL: `pip install mysql-connector-python`)
- LangChain Groq integration (`pip install langchain_groq`)

# Step-by-Step Instructions

(i). Set Up the Environment
Create a virtual environment and install the required packages:
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install streamlit langchain sqlalchemy mysql-connector-python langchain_groq
```

(ii). Create the Database
Run the SQLite setup script to create the sample database:
```bash
python sqlite.py
```
This will generate a `student.db` file in the project directory.

(iii). Run the Streamlit Application
Start the Streamlit app:
```bash
streamlit run app.py
```
The application will open in your default web browser at `http://localhost:8501`.

(iv). Configure the Application
- **For SQLite Mode**:
  - Select "Use SQLite 3 Database - Student.db" in the sidebar.
  - Enter your Groq API key when prompted.

- **For MySQL Mode**:
  - Select "Connect to your MySQL Database" in the sidebar.
  - Provide MySQL host, user, password, and database name.
  - Enter your Groq API key.

(v). Interact with the Application
- Use the chat interface to ask questions about the database (e.g., "What is the highest mark in Data Science?").
- Clear the conversation history using the "Clear message history" button if needed.

# Troubleshooting
- **Database Connection Issues**: Verify credentials and ensure the database server is accessible.
- **API Key Errors**: Confirm the Groq API key is correct and has the necessary permissions.
- **Query Failures**: Refine ambiguous questions or check the database schema for consistency.


### Use Cases
1. **Educational Institutions**: Query student performance metrics.
2. **Data Analysis**: Extract insights from structured data without SQL knowledge.
3. **Database Debugging**: Test queries interactively using natural language.

### Challenges & Solutions
- **Database Compatibility**: The agent must handle schema differences between SQLite and MySQL.
- **Query Complexity**: The agent may struggle with ambiguous or overly complex questions, requiring user refinement.
- **Streaming Latency**: Groq's streaming responses are optimized for readability but may lag with large datasets.

### Future Enhancements
1. **Error Handling**: Improve feedback for invalid queries or connection failures.
2. **Multi-Database Support**: Extend to PostgreSQL or other SQL variants.
3. **Visualization**: Integrate charts/graphs to represent query results visually.
4. **Authentication**: Add user login for multi-user environments.

### Demo Video Link
Link : https://drive.google.com/file/d/1-NFY-DflaHSiWVTuOX_Rrgjm07hSOhAd/view?usp=sharing
