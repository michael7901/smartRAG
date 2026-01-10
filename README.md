<p align="center">
  <img src="icon.svg" alt="SmartRAG Logo" width="200" height="200">
</p>

<p align="center">
  <b>Multi-Modal RAG for .NET — query databases, documents, images & audio in natural language</b>
</p>

## 🚀 **Quick Start**

### **1. Install SmartRAG**
```bash
dotnet add package SmartRAG
```

### **2. Setup**
```csharp
// For Web API applications
builder.Services.AddSmartRag(builder.Configuration, options =>
{
    options.AIProvider = AIProvider.OpenAI;
    options.StorageProvider = StorageProvider.InMemory;
});

// For Console applications
var serviceProvider = services.UseSmartRag(
    configuration,
    aiProvider: AIProvider.OpenAI,
    storageProvider: StorageProvider.InMemory
);
```

### **3. Configure databases in appsettings.json**
```json
{
  "SmartRAG": {
    "DatabaseConnections": [
      {
        "Name": "Sales",
        "ConnectionString": "Server=localhost;Database=Sales;...",
        "DatabaseType": "SqlServer"
      }
    ]
    }
}
```

### **4. Upload documents & ask questions**
```csharp
// Upload document
var document = await documentService.UploadDocumentAsync(
    fileStream, fileName, contentType, "user-123"
);

// Unified query across databases, documents, images, and audio
var response = await searchService.QueryIntelligenceAsync(
    "Show me all customers who made purchases over $10,000 in the last quarter, their payment history, and any complaints or feedback they provided"
);
// → AI automatically analyzes query intent and routes intelligently:
//   - High confidence + database queries → Searches databases only
//   - High confidence + document queries → Searches documents only  
//   - Medium confidence → Searches both databases and documents, merges results
// → Queries SQL Server (orders), MySQL (payments), PostgreSQL (customer data)
// → Analyzes uploaded PDF contracts, OCR-scanned invoices, and transcribed call recordings
// → Provides unified answer combining all sources
```

### **5. (Optional) Configure MCP Client & File Watcher**
```json
{
  "SmartRAG": {
    "Features": {
      "EnableMcpSearch": true,
      "EnableFileWatcher": true
    },
    "McpServers": [
      {
        "ServerId": "example-server",
        "Endpoint": "https://mcp.example.com/api",
        "TransportType": "Http"
      }
    ],
    "WatchedFolders": [
      {
        "FolderPath": "/path/to/documents",
        "AllowedExtensions": [".pdf", ".docx", ".txt"],
        "AutoUpload": true
      }
    ],
    "DefaultLanguage": "en"
  }
}
```

**Want to test SmartRAG immediately?** → [Jump to Examples & Testing](#-examples--testing)


## 🏆 **Why SmartRAG?**

🎯 **Unified Query Intelligence** - Single query searches across databases, documents, images, and audio automatically

🧠 **Smart Hybrid Routing** - AI analyzes query intent and automatically determines optimal search strategy

🗄️ **Multi-Database RAG** - Query multiple databases simultaneously with natural language

📄 **Multi-Modal Intelligence** - PDF, Word, Excel, Images (OCR), Audio (Speech-to-Text), and more  

🔌 **MCP Client Integration** - Connect to external MCP servers and extend capabilities with external tools

📁 **Automatic File Watching** - Monitor folders and automatically index new documents without manual uploads

🧩 **Modular Architecture** - Strategy Pattern for SQL dialects, scoring, and file parsing

🏠 **100% Local Processing** - GDPR, KVKK, HIPAA compliant

🚀 **Production Ready** - Enterprise-grade, thread-safe, high performance

## 🎯 **Real-World Use Cases**

### **1. Banking - Customer Financial Profile**
```csharp
var answer = await searchService.QueryIntelligenceAsync(
    "Which customers have overdue payments and what's their total outstanding balance?"
);
// → Queries Customer DB, Payment DB, Account DB and combines results
// → Provides comprehensive financial risk assessment for credit decisions
```

### **2. Healthcare - Patient Care Management**
```csharp
var answer = await searchService.QueryIntelligenceAsync(
    "Show me all patients with diabetes who haven't had their HbA1c checked in 6 months"
);
// → Combines Patient DB, Lab Results DB, Appointment DB and identifies at-risk patients
// → Ensures preventive care compliance and reduces complications
```

### **3. Inventory - Supply Chain Optimization**
```csharp
var answer = await searchService.QueryIntelligenceAsync(
    "Which products are running low on stock and which suppliers can restock them fastest?"
);
// → Analyzes Inventory DB, Supplier DB, Order History DB and provides restocking recommendations
// → Prevents stockouts and optimizes supply chain efficiency
```

## 🚀 **What Makes SmartRAG Special?**

- **Native multi-database RAG capabilities** for .NET
- **Automatic schema detection** across different database types  
- **100% local processing** with Ollama and Whisper.net
- **Enterprise-ready** with comprehensive error handling and logging
- **Cross-database queries** without manual SQL writing
- **Multi-modal intelligence** combining documents, databases, and AI
- **MCP Client integration** for extending capabilities with external tools
- **Automatic file watching** for real-time document indexing

## 🧪 **Examples & Testing**

SmartRAG provides comprehensive example applications for different use cases:

### **📁 Available Examples**
```
examples/
├── SmartRAG.API/          # Complete REST API with Swagger UI
└── SmartRAG.Demo/         # Interactive console application
```

### **🚀 Quick Test with Demo**

**Prerequisites:** You need to have databases and AI services running locally, or use Docker for easy setup.

📖 **[SmartRAG.Demo README](examples/SmartRAG.Demo/README.md)** - Complete demo application guide and setup instructions

#### **🐳 Docker Setup (Recommended)**

For the easiest experience with all services pre-configured:

```bash
# Start all services (SQL Server, MySQL, PostgreSQL, Ollama, Qdrant, Redis)
docker-compose up -d

# Setup AI models
docker exec -it smartrag-ollama ollama pull llama3.2
docker exec -it smartrag-ollama ollama pull nomic-embed-text
```

📚 **[Complete Docker Setup Guide](examples/SmartRAG.Demo/README-Docker.md)** - Detailed Docker configuration, troubleshooting, and management

### **📋 Demo Features & Steps:**

**🔗 Database Management:**
- **Step 1-2**: Show connections & health check
- **Step 3-5**: Create test databases (SQL Server, MySQL, PostgreSQL)
- **Step 6**: Create SQLite test database
- **Step 7**: View database schemas and relationships

**🤖 AI & Query Testing:**
- **Step 8**: Query analysis - see how natural language converts to SQL
- **Step 9**: Automatic test queries - pre-built scenarios
- **Step 10**: Multi-database AI queries - ask questions across all databases

**🏠 Local AI Setup:**
- **Step 11**: Setup Ollama models for 100% local processing
- **Step 12**: Test vector stores (InMemory, FileSystem, Redis, SQLite, Qdrant)

**📄 Document Processing:**
- **Step 13**: Upload documents (PDF, Word, Excel, Images, Audio)
- **Step 14**: List and manage uploaded documents
- **Step 15**: Clear documents for fresh testing
- **Step 16**: Conversational Assistant - combine databases + documents + chat
- **Step 17**: MCP Integration - list tools and run MCP queries

**📁 File Watcher:**
- Automatic folder monitoring for new documents
- Real-time document indexing
- Duplicate detection and prevention

**Perfect for:** Quick evaluation, proof-of-concept, team demos, learning SmartRAG capabilities

📚 Step-by-step tutorials and test scenarios

## 🎯 **Supported Data Sources**

**📊 Databases:** SQL Server, MySQL, PostgreSQL, SQLite  
**📄 Documents:** PDF, Word, Excel, PowerPoint, Images, Audio  
**🤖 AI Models:** OpenAI, Anthropic, Gemini, Azure OpenAI, Ollama (local), LM Studio  
**🗄️ Vector Stores:** Qdrant, Redis, InMemory  
**💬 Conversation Storage:** Redis, SQLite, FileSystem, InMemory (independent from document storage)  
**🔌 External Integrations:** MCP (Model Context Protocol) servers for extended tool capabilities  
**📁 File Monitoring:** Automatic folder watching with real-time document indexing

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

**Built with ❤️ by Barış Yerlikaya**

Made in Turkey 🇹🇷 | [Contact](mailto:b.yerlikaya@outlook.com) | [LinkedIn](https://www.linkedin.com/in/barisyerlikaya/)