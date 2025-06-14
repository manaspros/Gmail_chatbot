# Gmail RAG Assistant

A powerful Retrieval-Augmented Generation (RAG) system that connects to your Gmail account, creates a vector database from your emails, and uses Google's Gemini AI to answer questions about your email history.

## Features

- 📧 **Gmail Integration**: Direct access to your Gmail account via Google APIs
- 🧠 **AI-Powered**: Uses Google's Gemini for intelligent responses
- 🔍 **Vector Search**: Advanced semantic search through your email history
- 💬 **Interactive Chat**: User-friendly Gradio interface
- 📊 **Visualization**: 2D visualization of your email vector space
- 🔒 **Privacy-First**: All processing done locally on your machine

## Prerequisites

- Python 3.8 or higher
- Google account with Gmail
- Google Cloud Project (free tier available)
- Google API key

## Setup Instructions

### 1. Google Cloud Setup

1. **Create a Google Cloud Project**:
   - Go to [Google Cloud Console](https://console.cloud.google.com/)
   - Click "New Project" and give it a name (e.g., "Gmail RAG Assistant")
   - Note your project ID

2. **Enable Gmail API**:
   - In your project, go to "APIs & Services" > "Library"
   - Search for "Gmail API" and click "Enable"

3. **Create OAuth 2.0 Credentials**:
   - Go to "APIs & Services" > "Credentials"
   - Click "Create Credentials" > "OAuth client ID"
   - Choose "Desktop application"
   - Download the JSON file and rename it to `credentials.json`
   - Place it in the same directory as your notebook

4. **Get Google API Key** (for Gemini):
   - Go to [Google AI Studio](https://makersuite.google.com/app/apikey)
   - Click "Create API Key"
   - Copy the API key

### 2. Environment Setup

1. **Install Required Packages**:
   ```bash
   pip install google-auth google-auth-oauthlib google-auth-httplib2
   pip install googleapis-common-protos beautifulsoup4
   pip install langchain-google-genai
   pip install gradio plotly scikit-learn
   pip install chromadb langchain-chroma
   ```

2. **Create .env File**:
   Create a `.env` file in the project directory:
   ```env
   GOOGLE_API_KEY=your_gemini_api_key_here
   ```

3. **Project Structure**:
   ```
   week5/
   ├── gmail_rag.ipynb
   ├── credentials.json  # Your OAuth credentials
   ├── .env             # Your API keys
   ├── token.pickle     # Auto-generated after first auth
   └── gmail_vector_db/ # Auto-generated vector database
   ```

### 3. Gmail Permissions

The system requires the following Gmail scope:
- `https://www.googleapis.com/auth/gmail.readonly` - Read-only access to your emails

**What the app can access**:
- ✅ Read email content (subject, sender, body, date)
- ✅ Search through your email history
- ❌ Cannot send emails
- ❌ Cannot delete emails
- ❌ Cannot modify emails

## Usage

### First Run

1. **Start the Notebook**:
   - Open `gmail_rag.ipynb` in Jupyter
   - Run all cells

2. **Gmail Authentication**:
   - On first run, your browser will open
   - Sign in to your Google account
   - Grant permissions to the Gmail RAG app
   - You'll see "The authentication flow has completed"

3. **Email Processing**:
   - The system will extract up to 500 emails (configurable)
   - Emails are processed into chunks and embedded
   - A vector database is created locally

4. **Chat Interface**:
   - A Gradio web interface will launch
   - Ask questions about your emails!

### Example Questions

- "What emails did I receive about work projects?"
- "Show me recent emails from my manager"
- "What meeting invitations did I get this week?"
- "Find emails about travel plans"
- "What important announcements did I receive?"
- "Who sent me emails about deadlines?"
- "What were the main topics discussed in recent emails?"

## Configuration

### Email Limits
```python
MAX_EMAILS = 500  # Adjust in the notebook
```

### Search Filters
You can modify the Gmail query in the code:
```python
# Examples:
emails = gmail.get_emails(query='from:manager@company.com')
emails = gmail.get_emails(query='subject:important')
emails = gmail.get_emails(query='after:2024/01/01')
```

### Model Selection
The system automatically uses:
- **Gemini 1.5 Flash** for chat responses
- **Google's embedding-001** for vector embeddings

## Troubleshooting

### Common Issues

1. **"credentials.json not found"**:
   - Download OAuth credentials from Google Cloud Console
   - Rename to `credentials.json` and place in project directory

2. **"Google API key not found"**:
   - Get API key from Google AI Studio
   - Add to `.env` file as `GOOGLE_API_KEY=your_key`

3. **Dependency conflicts**:
   ```bash
   pip install --upgrade google-ai-generativelanguage>=0.6.18
   pip install --force-reinstall langchain-google-genai
   ```

4. **"perplexity must be less than n_samples"**:
   - This happens with very few emails
   - The system automatically adjusts parameters

5. **Gmail authentication fails**:
   - Check that Gmail API is enabled in Google Cloud
   - Verify OAuth credentials are for "Desktop application"
   - Delete `token.pickle` and re-authenticate

### Data Privacy

- **Local Processing**: All email data stays on your machine
- **No Data Sharing**: Emails are not sent to external services
- **Secure Storage**: Vector database is stored locally
- **API Usage**: Only anonymized embeddings sent to Google for processing

## Technical Details

### Architecture
- **Gmail API**: Extracts email content
- **LangChain**: Document processing and RAG pipeline
- **ChromaDB**: Local vector database
- **Gemini**: AI language model for responses
- **Gradio**: Web interface

### Performance
- **Initial Setup**: 5-10 minutes for 500 emails
- **Subsequent Runs**: Instant (uses cached database)
- **Query Speed**: 1-3 seconds per question

## Security Considerations

1. **OAuth Flow**: Uses industry-standard OAuth 2.0
2. **Read-Only Access**: Cannot modify your emails
3. **Local Storage**: All data remains on your machine
4. **API Keys**: Keep your `.env` file secure and private

## Limitations

- Maximum 500 emails by default (configurable)
- Requires internet connection for Gemini API
- Large mailboxes may take longer to process
- Some email formatting may not be perfectly preserved

## Contributing

Feel free to enhance the system by:
- Adding support for attachments
- Implementing email filtering options
- Adding more visualization features
- Improving error handling

## License

This project is for educational purposes as part of the LLM Engineering course.
