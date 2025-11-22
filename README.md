# Enterprise Document Indexer

A Flask-based web application for indexing and searching enterprise documents using Elasticsearch. Supports multiple file formats including DOCX, PDF, XLSX, PPTX, images (with OCR), and more.

## Features

- **Multi-format Support**: Index documents in DOCX, XLSX, PDF, PPTX, TXT, RTF, DOC, MD, CSV, XML, PNG, JPEG, JPG
- **OCR Capabilities**: Extract text from images and embedded images in documents
- **Elasticsearch Integration**: Fast and powerful full-text search
- **Real-time Indexing**: Progress tracking with WebSocket updates
- **File Preview**: Preview documents in PDF format
- **Corrupt File Handling**: Automatic detection and recovery of corrupt files
- **Caching**: Intelligent caching system to speed up re-indexing

## Prerequisites

- Python 3.8+
- Elasticsearch 7.x or 8.x (running locally or remotely)
- Tesseract OCR (for image text extraction)
- LibreOffice (optional, for PPTX to PDF conversion)

### Installing Prerequisites

#### macOS
```bash
# Install Elasticsearch
brew install elastic/tap/elasticsearch-full
brew services start elastic/tap/elasticsearch-full

# Install Tesseract
brew install tesseract

# Install LibreOffice (optional)
brew install --cask libreoffice
```

#### Linux (Ubuntu/Debian)
```bash
# Install Elasticsearch
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.12.1-amd64.deb
sudo dpkg -i elasticsearch-8.12.1-amd64.deb
sudo systemctl start elasticsearch

# Install Tesseract
sudo apt-get install tesseract-ocr

# Install LibreOffice
sudo apt-get install libreoffice
```

#### Windows
- Download and install [Elasticsearch](https://www.elastic.co/downloads/elasticsearch)
- Download and install [Tesseract OCR](https://github.com/UB-Mannheim/tesseract/wiki)
- Download and install [LibreOffice](https://www.libreoffice.org/download/)

## Installation

1. **Clone the repository**
   ```bash
   git clone <your-repo-url>
   cd AI
   ```

2. **Create a virtual environment**
   ```bash
   python3 -m venv doc_indexer_env
   source doc_indexer_env/bin/activate  # On Windows: doc_indexer_env\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   cd "Enterprise Document"
   pip install -r requirements.txt
   ```

4. **Set up environment variables**
   ```bash
   cp ../.env.example .env
   # Edit .env and update the configuration values
   ```

5. **Configure environment variables**

   Edit the `.env` file in the `Enterprise Document` directory:

   ```env
   # Required
   SECRET_KEY=your-secret-key-here  # Generate with: python -c "import secrets; print(secrets.token_hex(16))"
   ELASTICSEARCH_URL=http://localhost:9200

   # Optional (will use system PATH if not set)
   TESSERACT_CMD=/opt/homebrew/bin/tesseract  # macOS
   LIBREOFFICE_PATH=/Applications/LibreOffice.app/Contents/MacOS/soffice  # macOS
   DEFAULT_DIRECTORY=~/Documents
   CACHE_DIR=/tmp/cache
   CORRUPT_FILES_DIR=/tmp/corruptedfiles
   ```

## Running the Application

1. **Start Elasticsearch** (if not running as a service)
   ```bash
   # macOS
   /opt/homebrew/opt/elasticsearch-full/bin/elasticsearch
   
   # Or if installed as service
   brew services start elastic/tap/elasticsearch-full
   ```

2. **Verify Elasticsearch is running**
   ```bash
   curl http://localhost:9200
   ```

3. **Run the Flask application**
   ```bash
   cd "Enterprise Document"
   python3 app.py
   ```

4. **Access the application**
   - Open your browser and navigate to: `http://localhost:5000`
   - For development mode: `http://localhost:5000/?dev=true`

## Usage

1. **Index Documents**
   - Enter or select a directory path containing your documents
   - Click "Index Directory" to start indexing
   - Monitor progress in real-time via WebSocket updates

2. **Search Documents**
   - Enter your search query in the search box
   - Results are ranked by relevance
   - Click "Preview" to view documents in PDF format

3. **Advanced Search**
   - Use `metadata.field:value` syntax to search specific metadata fields
   - Example: `metadata.author:John` to find documents by author

## Configuration

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `SECRET_KEY` | Flask secret key for sessions | Required |
| `ELASTICSEARCH_URL` | Elasticsearch connection URL | `http://localhost:9200` |
| `TESSERACT_CMD` | Path to Tesseract executable | `tesseract` (from PATH) |
| `LIBREOFFICE_PATH` | Path to LibreOffice soffice | `soffice` (from PATH) |
| `DEFAULT_DIRECTORY` | Default directory for indexing | `~/Documents` |
| `CACHE_DIR` | Directory for caching extracted text | `/tmp/cache` |
| `CORRUPT_FILES_DIR` | Directory for corrupt files | `/tmp/corruptedfiles` |
| `NGROK_URL` | Public URL for ngrok/external access | `http://localhost:5000` |
| `FLASK_ENV` | Flask environment (development/production) | `development` |

## Project Structure

```
AI/
├── Enterprise Document/
│   ├── app.py              # Main Flask application
│   ├── caching.py          # Caching utilities
│   ├── corruptzip.py       # Corrupt file handling
│   ├── requirements.txt    # Python dependencies
│   └── templates/
│       └── index.html      # Web interface
├── .gitignore              # Git ignore rules
├── .env.example            # Environment variables template
└── README.md               # This file
```

## Troubleshooting

### Elasticsearch Connection Failed
- Ensure Elasticsearch is running: `curl http://localhost:9200`
- Check the `ELASTICSEARCH_URL` in your `.env` file
- Verify Elasticsearch is accessible from your network

### Tesseract Not Found
- Install Tesseract OCR
- Set `TESSERACT_CMD` in `.env` to the full path
- Or ensure Tesseract is in your system PATH

### LibreOffice Conversion Fails
- Install LibreOffice
- Set `LIBREOFFICE_PATH` in `.env` to the full path
- The application will fall back to text extraction if conversion fails

### Permission Errors
- Ensure the application has read access to the directories you want to index
- Check write permissions for cache and corrupt files directories

## Security Notes

- **Never commit your `.env` file** - it contains sensitive information
- Generate a strong `SECRET_KEY` for production use
- If deploying publicly, ensure Elasticsearch is properly secured
- Consider enabling Elasticsearch security features for production

## License

[Add your license here]

## Contributing

[Add contribution guidelines here]

## Support

[Add support information here]
