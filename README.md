# Article Drafting MVP

A professional article drafting tool that transforms interview transcripts and supporting sources into compelling articles using AI with comprehensive human review capabilities.

## 🎯 Problem Statement

Journalists and content creators often struggle with:
- Manually extracting key insights from long interview transcripts
- Ensuring accurate source attribution and quote verification  
- Maintaining consistency in tone and style across articles
- Managing multiple source materials efficiently
- Verifying quote accuracy against original sources

This MVP solves these challenges by providing an AI-assisted workflow with human oversight at every critical step.

## ✨ Core Features

### 1. **Multi-Source Content Ingestion**
- Upload interview transcripts (TXT, PDF, DOCX)
- Attach supporting documents (PDF brochures, fact sheets)
- Add web sources (YouTube videos, articles, company websites)
- Drag-and-drop file upload with text extraction

### 2. **AI-Powered Key Point Extraction**
- Automatically extracts newsworthy insights from all sources
- Categorizes points by type (quotes, facts, achievements, insights)
- Ranks by importance and relevance
- Provides source attribution for each point

### 3. **Human-in-the-Loop Review System**
- ✅ **Approve/Reject** individual key points
- ✏️ **Edit** extracted points inline
- 🔄 **Reorder** points by priority (drag & drop)
- 🗑️ **Delete** irrelevant points
- Visual progress tracking (approved vs total points)

### 4. **Story Direction Controls**
- **Article Style**: News, Feature Story, Company Profile, Interview Summary
- **Length Target**: Short (400-600), Medium (600-1000), Long (1000+ words)
- **Tone/Angle**: Professional, Conversational, Analytical, Promotional
- **Custom Instructions**: Additional AI guidance field

### 5. **AI Article Generation**
- Uses ONLY approved key points for generation
- Maintains source accuracy and attribution
- Follows specified style, length, and tone
- Incorporates custom instructions and requirements

### 6. **Source Mapping & Attribution**
- Visual source references sidebar
- Paragraph-level source attribution
- Hover tooltips showing supporting sources
- "Used Key Points" tracking panel

### 7. **Quote Verification System**
- Automatically detects quoted text in articles
- Verifies quotes against original sources
- Status indicators:
  - ✅ **Verified**: Exact match found
  - ⚠️ **Partial Match**: Similar content found
  - ❌ **Unverified**: No source match
- Shows source snippets with context

### 8. **Professional Export Options**
- **Markdown**: Blog-ready format
- **HTML**: Web-ready with styling
- **PDF**: Professional document
- **Word**: DOCX-compatible format
- **Provenance JSON**: Complete source mapping metadata
- **Copy to Clipboard**: Quick sharing

## 🏗️ Architecture

### Frontend Architecture
- **Pure HTML/CSS/JavaScript**: No build tools required
- **Responsive Design**: Works on desktop and mobile
- **Progressive Enhancement**: Graceful fallbacks for all features
- **Client-Side Processing**: No server dependencies

### AI Integration
- **Primary**: Groq API (fast inference)
- **Fallback**: Intelligent offline processing
- **Modular Design**: Easy to swap AI providers

### Data Flow
```
Sources → AI Extraction → Human Review → AI Generation → Quote Verification → Export
```

### Key Components
1. **Source Manager**: Handles file uploads and text extraction
2. **AI Analyzer**: Extracts key points and generates content
3. **Review Interface**: Human approval and editing system
4. **Quote Checker**: Verification against original sources
5. **Export Engine**: Multiple format generation with provenance

## 🛠️ Setup & Installation

### Prerequisites
- Modern web browser with JavaScript enabled
- AI API key (see configuration below)

### Quick Start
1. **Clone the repository:**
   ```bash
   git clone https://github.com/DinukaEk/Article-Drafting-MVP.git
   cd article-drafting-mvp
   ```

2. **Configure AI API Key:**
   Open `index.html` and locate **line 299-304**:
   ```javascript
   // AI API Configuration
   const AI_CONFIG = {
       provider: 'GROQ',
       key: 'your-api-key-here', // ← REPLACE THIS
       url: 'https://api.groq.com/openai/v1/chat/completions',
       model: 'mixtral-8x7b-32768'
   };
   ```
   
   **Replace `'your-api-key-here'` with your actual API key**

3. **Open in browser:**
   ```bash
   # Serve locally (recommended)
   python -m http.server 8000
   # OR
   npx serve .
   
   # Then open http://localhost:8000
   ```

### API Key Configuration

**⚠️ IMPORTANT: API Key Required**

The application requires an AI API key to function. **Do NOT commit your API key to version control.**

#### Supported Providers:
- **Groq** (Default - Fast & Free tier available)
- **OpenAI** (GPT models)
- **Anthropic** (Claude models)

#### Getting API Keys:
1. **Groq**: Visit [console.groq.com](https://console.groq.com) → Create account → API Keys
2. **OpenAI**: Visit [platform.openai.com](https://platform.openai.com) → API Keys
3. **Anthropic**: Visit [console.anthropic.com](https://console.anthropic.com) → API Keys

#### Configuration Location:
Edit the `AI_CONFIG` object in `index.html` at **lines 299-304**:

```javascript
const AI_CONFIG = {
    provider: 'GROQ',        // Change provider if needed
    key: 'YOUR_API_KEY_HERE', // ← ADD YOUR KEY HERE
    url: 'https://api.groq.com/openai/v1/chat/completions',
    model: 'mixtral-8x7b-32768'
};
```

#### Security Best Practices:
- Never commit API keys to Git
- Use environment variables in production
- Consider using a backend proxy for production deployments
- Monitor API usage and costs

## 🎮 Usage Guide

### Basic Workflow:
1. **Upload Sources**: Add interview transcript + supporting documents
2. **Extract Points**: AI analyzes and extracts key insights
3. **Review & Approve**: Human reviews, edits, and approves points
4. **Set Direction**: Configure article style, length, and tone
5. **Generate Article**: AI creates article using approved points only
6. **Verify Quotes**: Check all quotes against original sources
7. **Export**: Download in preferred format with full provenance

### Pro Tips:
- **Quality Sources**: Better input = better output
- **Selective Approval**: Only approve the most compelling points
- **Custom Instructions**: Use for specific requirements or focus areas
- **Quote Verification**: Always run before publication
- **Export Provenance**: Keep JSON for editorial transparency

## 🔧 Technical Implementation

### Key Point Approval System
The human-in-the-loop review ensures editorial control:

```javascript
function toggleApproval(index) {
    const point = appData.keyPoints[index];
    if (appData.approvedPoints.includes(index)) {
        // Remove approval - human oversight
        appData.approvedPoints = appData.approvedPoints.filter(i => i !== index);
    } else {
        // Add approval - human decision
        appData.approvedPoints.push(index);
    }
    updateUI(); // Visual feedback
}
```

### Source Mapping Implementation
Every paragraph links back to source material:

```javascript
function createSourceMapping(article, approvedPoints) {
    const mapping = {};
    const paragraphs = article.split('\n\n');
    
    paragraphs.forEach((paragraph, pIndex) => {
        approvedPoints.forEach(point => {
            if (paragraphReferencesPoint(paragraph, point)) {
                if (!mapping[pIndex]) mapping[pIndex] = [];
                mapping[pIndex].push({
                    point: point.text,
                    source: point.source,
                    type: point.type
                });
            }
        });
    });
    
    return mapping;
}
```

### Quote Verification Logic
Ensures editorial integrity:

```javascript
function verifyQuotes(quotes) {
    return quotes.map(quote => {
        const exactMatch = allSources.includes(quote.text);
        const partialMatch = findSimilarContent(quote.text, allSources);
        
        return {
            ...quote,
            status: exactMatch ? 'verified' : partialMatch ? 'partial' : 'unverified',
            sourceSnippet: extractSourceContext(quote.text, allSources)
        };
    });
}
```

## 🚦 Trade-offs & Decisions

### What We Prioritized:
- ✅ **Human Control**: Every key decision requires human approval
- ✅ **Source Accuracy**: Comprehensive quote verification
- ✅ **Simplicity**: Single HTML file, no build process
- ✅ **Reliability**: Offline fallbacks for all AI features
- ✅ **Transparency**: Full provenance tracking

### What We Simplified:
- **Source Extraction**: Manual URL content extraction (could integrate web scraping APIs)
- **Collaboration**: Single-user interface (could add multi-user support)
- **Version Control**: Simple regeneration vs. git-like versioning
- **Advanced NLP**: Basic keyword matching for source mapping (could use semantic similarity)

### Production Considerations:
- **API Security**: Move keys to backend/environment variables
- **Scalability**: Add database for project storage
- **Collaboration**: Multi-user editing and approval workflows
- **Integration**: CMS plugins, editorial system APIs

## 📊 What We'd Build Next (Given More Time)

### High-Priority Enhancements:
1. **Semantic Source Mapping**: Use embeddings for better paragraph-source matching
2. **Collaborative Review**: Multi-editor approval workflows
3. **Template Library**: Pre-built article structures and styles
4. **Advanced Quote Detection**: Handle paraphrases and indirect quotes
5. **Version Management**: Track article revisions and comparisons

### Editorial Features:
6. **Fact-Checking Integration**: API connections to fact-checking services
7. **Style Guide Enforcement**: Automated style and tone consistency
8. **SEO Optimization**: Keyword suggestions and readability scores
9. **Publication Workflow**: Direct CMS publishing integration
10. **Analytics Dashboard**: Track article performance and source effectiveness

### Technical Improvements:
11. **Backend API**: Secure key management and advanced processing
12. **Real-time Collaboration**: WebSocket-based multi-user editing
13. **Advanced AI**: Fine-tuned models for journalistic writing
14. **Mobile App**: Native iOS/Android applications
15. **Browser Extension**: Quick article generation from web content

## 🧪 Testing

### Manual Testing Checklist:
- [ ] Upload various file formats (TXT, PDF, DOCX)
- [ ] Extract key points from complex transcripts
- [ ] Approve/edit/reorder points workflow
- [ ] Generate articles with different styles/tones
- [ ] Verify quote checking accuracy
- [ ] Test all export formats
- [ ] Validate source mapping functionality

### Error Handling:
- ✅ Graceful AI API failures with offline fallback
- ✅ File upload error recovery
- ✅ Invalid input validation
- ✅ Network connectivity issues
- ✅ Malformed content handling

## 📝 Sample Input Files

For testing, use:
- **Interview Transcript**: Any AI/tech company interview (public sources only)
- **Supporting PDF**: Company brochure or fact sheet
- **Web Source**: Company website or news article

*Note: Only use publicly available content. Do not upload private/confidential materials.*

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **PDF.js**: Client-side PDF text extraction
- **Mammoth.js**: DOCX document processing  
- **jsPDF**: PDF generation capabilities
- **Groq**: Fast AI inference API
- **Tailwind CSS**: Utility-first styling approach

## 📞 Support

For questions, issues, or feature requests:
- Open a [GitHub Issue](https://github.com/DinukaEk/Article-Drafting-MVP/issues)
- Check existing issues for solutions
- Provide detailed reproduction steps for bugs

---

**Built for the 24-Hour Low-Code Challenge** - Demonstrating AI-assisted development with human-in-the-loop editorial control.
