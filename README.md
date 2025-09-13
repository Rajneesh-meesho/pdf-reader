# PDF Purchase Order Parser 📄

A full-stack application that extracts structured data from PDF purchase orders using AI and provides a modern dashboard for viewing and filtering orders.

## 🚀 Quick Start

### Option 1: Docker (Recommended)

```bash
cd llm_games

# put your gemini api key in docker-compose.prod.yml (search for your_api_key and replace)
docker-compose -f docker-compose.prod.yml up -d

# Access the application
# Frontend: http://localhost:3000
# Backend API: http://localhost:8000
# API Docs: http://localhost:8000/docs
```




### Option 2: Local Development

```bash
# 1. Start PostgreSQL
docker run --name postgres-parser \
  -e POSTGRES_USER=parser \
  -e POSTGRES_PASSWORD=parser123 \
  -e POSTGRES_DB=parser \
  -p 4000:5432 -d postgres

# 2. Create database schema
docker exec -i postgres-parser psql -U parser -d parser < parser/database_schema.sql


# 3. Setup and start backend
cd parser
python3 -m venv parser
source parser/bin/activate
pip install -r requirements.txt
# put your gemini api key in .env (search for your_api_key and replace)
python server.py



# 4. Setup and start frontend (new terminal)
cd frontend
npm install
npm start
```

## 📋 Features

### PDF Processing
- **AI-powered extraction** using Google Gemini
- **Structured data parsing** from purchase order PDFs
- **Automatic data validation** and normalization
- **Duplicate detection** with data override capability

### Dashboard
- **Orders overview** with key metrics
- **Advanced filtering** by model ID, color, size
- **Full-text search** across PO ID, buyer, supplier
- **Sorting** by date, amount, item count
- **Pagination** for both orders and line items

### Data Management
- **PostgreSQL storage** with optimized schema
- **UUID-based primary keys** for scalability
- **Foreign key relationships** with cascade deletes
- **Indexed columns** for fast queries

## 🏗️ Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   React App     │    │   FastAPI       │    │   PostgreSQL    │
│   (Frontend)    │───▶│   (Backend)     │───▶│   (Database)    │
│   Port 3000     │    │   Port 8000     │    │   Port 5432     │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                                │
                                ▼
                       ┌─────────────────┐
                       │   Google        │
                       │   Gemini AI     │
                       └─────────────────┘
```

## 📁 Project Structure

```
llm_games/
├── parser/                     # Backend (FastAPI)
│   ├── server.py              # Main API server
│   ├── gemini.py              # AI PDF parsing
│   ├── database.py            # Database operations
│   ├── database_schema.sql    # PostgreSQL schema
│   ├── requirements.txt       # Python dependencies
│   └── Dockerfile            # Backend container
├── frontend/                   # Frontend (React)
│   ├── src/
│   │   ├── components/        # React components
│   │   ├── App.js            # Main app
│   │   └── App.css           # Styles
│   ├── package.json          # Node dependencies
│   └── Dockerfile            # Frontend container
├── sample_pdfs/               # Test PDF files
├── docker-compose.yml         # Development setup
├── docker-compose.prod.yml    # Production setup
└── README.md                  # This file
```

## 🔧 API Endpoints

### Upload
- `POST /upload-pdf` - Upload and parse PDF

### Orders
- `GET /orders` - List orders with filtering/pagination
- `GET /orders/{id}` - Get order details with line items

### Utilities
- `GET /filters` - Get available filter options
- `GET /stats` - Get dashboard statistics

### API Documentation
Interactive docs available at: http://localhost:8000/docs

## 🗄️ Database Schema

### Orders Table
- Purchase order information (ID, date, buyer, supplier, totals)
- UUID primary keys for scalability

### Line Items Table
- Individual items with model, color, size, quantity, pricing
- Foreign key relationship to orders with cascade delete

## 🎯 Sample Data

The `sample_pdfs/` directory contains test purchase order PDFs for development and demonstration.

## 🐳 Docker Deployment

### Development
```bash
docker-compose up --build
```

### Production Docker totallay independent
first 
```bash
docker-compose -f docker-compose.prod.yml up -d
```

### Docker Hub Images
- Backend: `rajneesh2311/pdf-parser-backend:latest`
- Frontend: `rajneesh2311/pdf-parser-frontend:latest`

## 🔐 Environment Variables

### Required
- `GEMINI_API_KEY` - Google Gemini API key for PDF parsing

### Optional (with defaults)
- `POSTGRES_HOST` - Database host (default: localhost)
- `POSTGRES_PORT` - Database port (default: 4000 for local, 5432 for Docker)
- `POSTGRES_USER` - Database user (default: parser)
- `POSTGRES_PASSWORD` - Database password (default: parser123)
- `POSTGRES_DB` - Database name (default: parser)

## 🧪 Testing

### Upload Test
1. Go to http://localhost:3000/upload
2. Upload a PDF from `sample_pdfs/`
3. Verify data appears in dashboard

### API Test
```bash
# Health check
curl http://localhost:8000/stats

# Upload PDF
curl -X POST "http://localhost:8000/upload-pdf" \
  -H "Content-Type: multipart/form-data" \
  -F "file=@sample_pdfs/purchase-order.pdf"
```

## 🚀 Deployment to Production

1. **Push to Docker Hub**
2. **Set environment variables** on target server
3. **Run docker-compose.prod.yml**
4. **Access via domain/IP**

## ⚠️ Known Limitations

1. **PDF Format Dependency**: Works best with structured purchase order PDFs. Complex layouts or non-standard formats may require prompt adjustments.

2. **AI Parsing Accuracy**: Depends on Google Gemini's interpretation. Very complex tables or poor scan quality may result in parsing errors.

3. **Single File Processing**: Currently processes one PDF at a time. Batch processing would require additional implementation.

4. **No User Authentication**: Currently open access. Production deployment should add authentication/authorization.

## 🔮 Future Improvements

1. **Batch PDF Processing** - Upload and process multiple PDFs simultaneously
2. **Advanced AI Models** - Support for multiple AI providers (OpenAI, Claude, etc.)
3. **Export Functionality** - Export filtered data to CSV/Excel
4. **Real-time Updates** - WebSocket integration for live data updates
5. **User Management** - Authentication and role-based access
6. **Advanced Analytics** - Charts, trends, and business insights
7. **API Rate Limiting** - Protect against abuse
8. **Audit Logging** - Track all data changes
9. **Data Backup** - Automated backup and restore capabilities
10. **Mobile Responsive** - Enhanced mobile experience

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## 📄 License

MIT License - See LICENSE file for details

## 👨‍💻 Author

Built with ❤️ using FastAPI, React, PostgreSQL, and Google Gemini AI.