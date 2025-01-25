# EduSales-Chatbot

This project allows users to interact with a website by ingesting its content, storing it in a vector database, and providing a conversational interface through a custom chatbot powered by OpenAI's GPT-3.5.

The system uses **Streamlit** for the user interface, **PostgreSQL** for storing chat history, and **LangChain** for document processing and conversational retrieval.

## Features
- Chat with a website's content by querying information stored from the website.
- Retrieve relevant information from documents using a vector database (Chroma).
- Conversational history is stored in a PostgreSQL database.
- Dynamic session management with unique session IDs for each user.
- Real-time message streaming for a more interactive chat experience.

## Installation

### Prerequisites

Before you begin, ensure you have the following software installed:

- Python 3.8+
- PostgreSQL
- OpenAI API Key (if using OpenAI models for the chatbot)
- Streamlit
- Required Python packages (listed below)

### 1. Clone the repository
```bash
git clone https://github.com/your-username/chat-with-website.git
cd chat-with-website
```

### 2. Install the dependencies

You can install all the required dependencies using `pip`. It is recommended to create a virtual environment for this project.

```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### 3. Install PostgreSQL

Ensure you have PostgreSQL installed and running on your local machine or server. Follow the instructions in the [PostgreSQL documentation](https://www.postgresql.org/docs/) for installation.

### 4. Set Up the Environment Variables

Create a `.env` file in the root directory of the project and define the following environment variables:

```env
WEBSITE_URL="https://example.com"  # Replace with the website URL you want to ingest
POSTGRES_CONNECTION_STRING="postgresql://username:password@localhost:5432/dbname"  # Replace with your PostgreSQL connection string
```

### 5. Initialize the PostgreSQL Database

Once your PostgreSQL instance is set up, you can initialize the `chat_history` table by running the app. The application will create the table automatically if it doesn't already exist.

Alternatively, you can manually create the table by running the following SQL:

```sql
CREATE TABLE IF NOT EXISTS chat_history (
    id SERIAL PRIMARY KEY,
    session_id TEXT,
    message_type TEXT,
    message_content TEXT,
    timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

## Usage

### 1. Run the Streamlit App

Once everything is set up, run the app using Streamlit:

```bash
streamlit run app.py
```

This will start the application, and you should be able to interact with the website content through the chat interface.

### 2. Interacting with the Chatbot

- Open the app in your web browser (usually at `http://localhost:8501`).
- Type in your queries related to the website's content, and the chatbot will respond based on the ingested website data.
- If you need to clear the chat history, click on the "Clear message history" button in the sidebar.

### 3. Viewing Chat History

All interactions between the user and the chatbot are stored in the PostgreSQL database. You can retrieve and analyze these interactions directly from the database or use the app's interface to interact with the content.

## How It Works

### 1. Ingesting Website Content

The website content is ingested using the `WebBaseLoader` from **LangChain**, which loads the content of the given website URL. The content is then split into smaller chunks using `RecursiveCharacterTextSplitter`, which helps in managing larger documents.

### 2. Creating and Using Vector Store

The ingested content is processed and stored in a **Chroma** vector database, which uses OpenAI embeddings to represent the document contents. This allows the chatbot to retrieve relevant documents based on user queries using a **ConversationalRetrievalChain**.

### 3. Chat History Storage

Each conversation is stored in a PostgreSQL database, and every message (both user and AI) is logged with the session ID, message type (user or assistant), and the content. This ensures that the chat history is persistent and can be cleared or retrieved as needed.

### 4. Dynamic Session Management

The app generates a unique session ID for each user interaction using Python's `uuid`. This helps ensure that each user's chat history is kept separate and can be cleared independently.

### 5. Real-Time Message Streaming

When the chatbot generates a response, the system uses **real-time streaming** to display the response gradually, creating a more interactive and engaging experience for the user.

## Configuration

### 1. Changing the Website URL

If you'd like to ingest a different website's content, update the `WEBSITE_URL` in the `.env` file with the new URL.

### 2. Adjusting PostgreSQL Connection

Modify the `POSTGRES_CONNECTION_STRING` in the `.env` file to match your PostgreSQL database credentials.

### 3. Model and Embeddings

The current version of the app uses OpenAI's GPT-3.5 model (`gpt-3.5-turbo`) for conversation. If you'd like to use a different model, you can modify the `model_name` parameter in the `ChatOpenAI` object in the code.

The embeddings are handled by **OpenAIEmbeddings**, but you can swap this out for other embedding models supported by LangChain.

### 4. Caching

Streamlit caches the vector store and retriever for improved performance. You can adjust the cache duration by modifying the `ttl` parameter in the `@st.cache_resource` decorators.

## Troubleshooting

- **Database connection error**: If you're seeing database connection issues, double-check your PostgreSQL connection string and ensure your PostgreSQL server is running.
- **Website ingestion error**: If the website ingestion fails, ensure the URL is accessible and that the content can be parsed correctly by the `WebBaseLoader`.
- **Streamlit errors**: If Streamlit isn't running properly, check the terminal output for error messages, and ensure all dependencies are installed correctly.

## Contributing

Contributions are welcome! Feel free to open issues and submit pull requests.

1. Fork the repository
2. Create a new branch (`git checkout -b feature-name`)
3. Make your changes
4. Commit your changes (`git commit -am 'Add feature'`)
5. Push to the branch (`git push origin feature-name`)
6. Open a pull request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

### Contact

If you have any questions or need assistance, feel free to open an issue on the GitHub repository.

---

Happy coding and enjoy chatting with your website content!
```

### Explanation:

- **Markdown Syntax**: The `README.md` is written using standard Markdown syntax, which GitHub and other platforms render in a user-friendly format.
- **Sections**: The file is divided into logical sections like installation, usage, configuration, how it works, troubleshooting, and contributing.
- **Environment Variables**: Instructions for setting up `.env` are clear and explain which values to modify.
- **Configuration Options**: The README provides guidance on how users can adjust the website URL, database connection string, and other settings.

