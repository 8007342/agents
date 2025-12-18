# The Appliance: ai-way Overview

## What is ai-way?

**From AJ's Perspective:**
An app that answers questions about their business files. Drop files in, ask questions, get answers. Simple.

**Technical Reality:**
A privacy-first AI appliance running local LLM inference in an isolated environment for secure, offline document analysis.

---

## The Problem We're Solving

### AJ's Pain Points
1. **Data Chaos**: Files everywhere in different formats (receipts, spreadsheets, emails, photos)
2. **Time Poverty**: No time to organize or analyze data
3. **Insight Deficit**: Knows answers are in the data, can't extract them
4. **Tool Fatigue**: Tried business software, too complicated
5. **Privacy Concerns**: Doesn't trust cloud services with business data

### Existing Solutions Fall Short
- **Cloud AI (ChatGPT, Claude, etc.)**:
  - Privacy concerns (data sent to servers)
  - Requires internet
  - Subscription costs
  - Can't handle local files easily

- **Local Tools (Excel, etc.)**:
  - Require manual organization
  - Steep learning curve
  - No natural language interface
  - Limited analysis capabilities

- **Business Software**:
  - Expensive
  - Complex setup
  - Requires training
  - Ongoing maintenance

### Our Solution: The Appliance
- Works offline (private, no internet required)
- No setup (downloads, runs)
- Natural language (ask questions like talking to a person)
- Handles any file type (photos, PDFs, spreadsheets, emails)
- Completely private (everything on AJ's computer)
- Free to use (no subscriptions after download)

---

## Core Features

### 1. Universal File Support
**What AJ Sees:**
- Drop any file type into the app
- AI reads and understands it

**What We Support:**
- Documents: PDF, DOCX, TXT, RTF, MD
- Spreadsheets: XLSX, XLS, CSV, ODS
- Presentations: PPTX, PPT
- Images: JPG, PNG (OCR for text extraction)
- Emails: EML, MSG, MBOX
- Archives: ZIP (extract and process contents)
- Web: HTML (for saved web pages)

**Future:**
- Videos: MP4 (transcription)
- Audio: MP3, WAV (transcription)
- Scans: Multi-page PDF OCR

### 2. Natural Language Queries
**What AJ Asks:**
- "Which customers haven't ordered in 90 days?"
- "How much did I spend on supplies last month?"
- "Find the invoice for John Smith from March"
- "Make a list of all pending orders"
- "Which products are low in stock?"

**What We Do:**
- Parse natural language
- Search through all dropped files
- Extract relevant information
- Synthesize coherent answers
- Generate structured outputs (lists, tables, reports)

### 3. Output Generation
**What AJ Gets:**
- Text answers in the output box
- Generated files (CSV, PDF, DOCX)
- Summaries and reports
- Data visualizations (future)

**Examples:**
- List of customers (CSV)
- Email draft to customers (TXT/DOCX)
- Expense report (PDF)
- Inventory summary (XLSX)

### 4. Privacy Guarantee
**What AJ Understands:**
- "Everything stays on your computer"
- "Works without internet"
- "Disappears when you close"

**What We Guarantee:**
- Zero data sent over network
- Zero persistent storage (unless AJ saves)
- Zero tracking or analytics
- Zero third-party access

---

## User Workflow

### The Simplest Possible UX

```
┌─────────────────────────────────────────┐
│  ai-way                          ─ □ ✕  │
├─────────────────────────────────────────┤
│                                         │
│  ╔═══════════════════════════════════╗ │
│  ║                                   ║ │
│  ║     Drop your files here          ║ │
│  ║                                   ║ │
│  ║       [Drag & Drop Area]          ║ │
│  ║                                   ║ │
│  ╚═══════════════════════════════════╝ │
│                                         │
│  ┌───────────────────────────────────┐ │
│  │ What would you like to know?      │ │
│  │                                   │ │
│  └───────────────────────────────────┘ │
│                          [Ask] button   │
│                                         │
│  ╔═══════════════════════════════════╗ │
│  ║ Your Results                      ║ │
│  ║                                   ║ │
│  ║  (Files and answers appear here)  ║ │
│  ║                                   ║ │
│  ║  [Drag files out to save them]    ║ │
│  ╚═══════════════════════════════════╝ │
│                                         │
│  Private • No Internet Needed           │
└─────────────────────────────────────────┘
```

### Step by Step

**Step 1: Open**
- AJ double-clicks app icon
- Window opens
- Shows: "Drop your files here"

**Step 2: Add Files**
- AJ drags files from desktop/folders
- Files appear as thumbnails/list
- Status: "Ready to help!"

**Step 3: Ask**
- AJ types question in plain English
- Clicks "Ask" or presses Enter
- Status changes: "Working on it..."

**Step 4: Wait**
- Progress indicator shows activity
- Messages like "Reading your files..." or "Thinking..."
- Typically 5-30 seconds depending on data volume

**Step 5: Review**
- Answer appears in results area
- Generated files appear as downloadable items
- AJ can ask follow-up questions

**Step 6: Save (Optional)**
- AJ drags files from results to desktop
- Only what AJ drags out gets saved
- Everything else stays ephemeral

**Step 7: Close**
- AJ clicks X to close window
- Everything disappears
- Next time: clean slate

---

## Technical Architecture

### The Stack (Simplified)

```
┌────────────────────────────────────────┐
│  AJ's View: "Just an App"              │
│  • Window with input/output boxes      │
│  • Drag and drop interface             │
│  • Simple, obvious, works              │
└────────────────────────────────────────┘
                  ↓
┌────────────────────────────────────────┐
│  Display Layer                         │
│  • Electron or Tauri (cross-platform)  │
│  • Native window controls              │
│  • File drag-drop handling             │
└────────────────────────────────────────┘
                  ↓
┌────────────────────────────────────────┐
│  Application Layer                     │
│  • Python backend (FastAPI)            │
│  • File parsing (PyMuPDF, python-docx) │
│  • NLP query processing                │
│  • Result generation                   │
└────────────────────────────────────────┘
                  ↓
┌────────────────────────────────────────┐
│  AI Layer                              │
│  • LLM (Ollama: Llama 3.1, Mistral)    │
│  • Vector DB (ChromaDB in-memory)      │
│  • RAG pipeline for document search    │
│  • GPU-accelerated inference           │
└────────────────────────────────────────┘
                  ↓
┌────────────────────────────────────────┐
│  Storage Layer                         │
│  • tmpfs (RAM disk) - ephemeral        │
│  • All files in RAM only               │
│  • Destroyed on close                  │
└────────────────────────────────────────┘
                  ↓
┌────────────────────────────────────────┐
│  Isolation Layer                       │
│  • Fedora Silverblue (minimal OS)      │
│  • Podman/Docker container             │
│  • No network access (air-gapped)      │
│  • Isolated from host system           │
└────────────────────────────────────────┘
                  ↓
┌────────────────────────────────────────┐
│  Hardware Layer                        │
│  • AJ's gaming laptop                  │
│  • NVIDIA/AMD GPU (or CPU fallback)    │
│  • 16-32GB RAM                         │
│  • Windows/Mac/Linux host OS           │
└────────────────────────────────────────┘
```

### Component Details

**Frontend (What AJ Sees):**
- Electron or Tauri app
- Single window interface
- Minimalist design (UX/UI Designer spec)
- Drag-drop file handling
- Real-time status updates

**Backend (Processing):**
- FastAPI server (local, not networked)
- File parsers for each format
- Query understanding (intent classification)
- RAG orchestration
- Response generation

**AI Engine:**
- Ollama for model serving
- Models: Llama 3.1 8B, Mistral 7B, Phi-3
- ChromaDB for vector storage
- Embeddings: sentence-transformers
- Context window: 8K-32K tokens

**Storage:**
- tmpfs mounted at /workspace
- All uploads → /workspace/input
- All outputs → /workspace/output
- All temp → /workspace/tmp
- Unmount on close = instant wipe

**OS/Container:**
- Fedora Silverblue base
- Minimal packages (no desktop environment)
- NVIDIA drivers pre-installed
- AMD ROCm support (secondary)
- Read-only root filesystem (security)

---

## Use Case Examples

### Example 1: Customer Outreach

**AJ's Files:**
- sales_2024.xlsx (all sales this year)
- sales_2023.xlsx (all sales last year)
- customer_emails.mbox (email archive)

**AJ's Query:**
"Which customers bought from me last year but haven't ordered anything this year?"

**What Happens:**
1. Parse both Excel files
2. Extract customer names from both years
3. Compare: customers in 2023 but not in 2024
4. Cross-reference with email archive
5. Generate list with contact info

**Output:**
- CSV file: "Inactive_Customers_2024.csv" (name, email, phone, last order date)
- Text answer: "Found 23 customers who haven't ordered this year. Last orders were between Jan-Dec 2023."

**AJ's Action:**
- Drags CSV to desktop
- Uses it to plan outreach campaign

---

### Example 2: Expense Tracking

**AJ's Files:**
- 47 photos of receipts (phone camera shots)
- 12 PDF invoices from suppliers
- expense_log.txt (partial notes)

**AJ's Query:**
"How much did I spend on materials in March?"

**What Happens:**
1. OCR all receipt photos
2. Extract dates, amounts, categories
3. Parse PDF invoices
4. Parse text notes
5. Filter for "materials" in March
6. Sum totals

**Output:**
- Text answer: "Total materials spending in March: $3,487.23"
- Detailed breakdown:
  - Lumber: $1,890.00 (4 receipts)
  - Hardware: $543.75 (8 receipts)
  - Paint: $876.48 (5 receipts)
  - Other: $177.00 (2 receipts)
- CSV export of all March material expenses

**AJ's Action:**
- Reviews breakdown
- Realizes paint costs are high
- Plans to shop for better supplier

---

### Example 3: Route Planning

**AJ's Files:**
- delivery_schedule.xlsx (addresses and dates)
- customer_addresses.csv (full contact info)
- google_maps_export.kml (previous routes)

**AJ's Query:**
"Plan my delivery route for next Tuesday to minimize drive time"

**What Happens:**
1. Extract Tuesday's deliveries
2. Geocode addresses
3. Apply traveling salesman approximation
4. Generate optimized route
5. Create turn-by-turn list

**Output:**
- Text answer: "Optimized route for Tuesday: 8 stops, estimated 4.5 hours driving"
- Route list in order with addresses
- Exported Google Maps link (future: KML file)

**AJ's Action:**
- Copies route to phone notes
- Follows route on Tuesday
- Saves 1.5 hours vs. ad-hoc routing

---

## Deployment & Distribution

### Phase 1: Single Executable (MVP)
**Target:** Windows 10/11

**Deliverable:**
- `ai-way.exe` (portable executable)
- Includes entire appliance in one file
- No installation needed
- Double-click to run
- ~8GB download (includes models)

**Distribution:**
- Direct download from website
- GitHub releases
- Torrent (for privacy-conscious users)

### Phase 2: Multi-Platform
**Targets:**
- Windows 10/11
- macOS (Intel & Apple Silicon)
- Linux (Ubuntu, Fedora)

**Deliverables:**
- Platform-specific installers
- Portable versions (no install)
- Package managers (future: apt, brew, flatpak)

### Phase 3: Automatic Updates (Opt-In)
**User Control:**
- AJ opts-in to check for updates
- Updates downloaded when AJ chooses
- AJ approves before installing
- Can always use offline

**Privacy:**
- Anonymous update checks (no telemetry)
- Signed binaries (verify integrity)
- Reproducible builds (trustworthy)

---

## System Requirements

### Minimum Specs
- **OS:** Windows 10/11, macOS 11+, Linux (kernel 5.x+)
- **CPU:** 4-core processor (Intel i5/AMD Ryzen 5 equivalent)
- **RAM:** 16GB
- **GPU:** Optional (Intel integrated graphics OK)
- **Storage:** 10GB free space for app + models
- **Display:** 1280x720 minimum resolution

### Recommended Specs (AJ's Gaming Laptop)
- **OS:** Windows 11
- **CPU:** 6-8 core (Intel i7/AMD Ryzen 7)
- **RAM:** 32GB
- **GPU:** NVIDIA RTX 3060+ or AMD RX 6600+
- **Storage:** 20GB free on SSD
- **Display:** 1920x1080+

### Performance Expectations
**CPU-Only (Minimum Specs):**
- Simple queries: 10-30 seconds
- Complex queries: 30-120 seconds
- Large document sets: 1-3 minutes

**With GPU (Recommended Specs):**
- Simple queries: 2-5 seconds
- Complex queries: 5-15 seconds
- Large document sets: 15-45 seconds

---

## Roadmap

### Version 1.0: MVP (Core Appliance)
**Features:**
- ✅ Drag-drop file input
- ✅ Natural language queries
- ✅ Text answers
- ✅ File output (CSV, TXT)
- ✅ Offline operation
- ✅ Ephemeral storage
- ✅ Windows support

**Models:**
- Llama 3.1 8B (instruction-tuned)
- Mistral 7B

**File Types:**
- PDF, DOCX, TXT, XLSX, CSV, images (basic OCR)

### Version 1.5: Enhanced UX
**Features:**
- [ ] Conversation history (session-only, RAM-based)
- [ ] Follow-up questions
- [ ] Multi-turn conversations
- [ ] Better progress indicators
- [ ] Richer output formatting

**Improvements:**
- [ ] Faster OCR
- [ ] Better table extraction
- [ ] Improved query understanding

### Version 2.0: Advanced Features
**Features:**
- [ ] Multi-modal (image understanding)
- [ ] Audio transcription
- [ ] Video analysis
- [ ] Data visualization (charts, graphs)
- [ ] Template-based outputs

**Platform:**
- [ ] macOS support
- [ ] Linux support
- [ ] ARM support (Apple Silicon, Raspberry Pi)

### Version 3.0: Opt-In Persistence
**Features:**
- [ ] Saved custom agents (AJ's specialized helpers)
- [ ] Personal knowledge base (opt-in)
- [ ] User preferences (saved settings)
- [ ] Project workspaces (organize by project)

**Privacy:**
- [ ] All stored data encrypted at rest
- [ ] User controls what persists
- [ ] Easy export/import
- [ ] Secure deletion

### Version 4.0: Collaboration (Maybe)
**Features:**
- [ ] Shared agents (between AJ and family members)
- [ ] Multi-user mode (without revealing data)
- [ ] Encrypted sync (between AJ's devices)

**Research Needed:**
- Multi-party computation
- Homomorphic encryption
- Zero-knowledge proofs

---

## Success Metrics

### User Metrics
- **Time to Value:** <5 minutes from download to first useful answer
- **Query Success Rate:** >80% of queries answered satisfactorily
- **User Retention:** AJ uses it weekly for 3+ months
- **Net Promoter Score:** AJ recommends to other small business owners

### Technical Metrics
- **Query Latency:** <15 seconds with GPU, <60 seconds CPU-only
- **Accuracy:** >85% correct answers (validated against manual analysis)
- **Crash Rate:** <1% of sessions
- **Privacy Audit:** Zero network traffic, zero persistent storage

### Business Metrics (If Commercial)
- **Downloads:** Track adoption
- **Support Requests:** Should be minimal (app is simple)
- **Word of Mouth:** AJs tell other AJs
- **Impact:** AJs report time savings, better business decisions

---

## Competitive Differentiation

| Feature | ai-way | ChatGPT/Claude | Microsoft 365 | QuickBooks |
|---------|--------|----------------|---------------|------------|
| **Privacy** | Complete (local) | Low (cloud) | Medium (cloud) | Medium (cloud) |
| **Offline** | Yes | No | Partial | No |
| **Setup Time** | <5 min | Instant | Hours/days | Hours/days |
| **Learning Curve** | Minutes | Minutes | Weeks | Weeks |
| **Cost** | Free* | $20/mo | $100+/yr | $300+/yr |
| **File Support** | Universal | Limited | Specific | Specific |
| **Natural Language** | Yes | Yes | No | Limited |
| **Customization** | Future | Limited | Extensive | Moderate |

*Free after download; models included

---

## Team Agents for ai-way Development

### Primary Agents
1. **Solutions Architect** - Overall appliance architecture
2. **UX/UI Designer** - Minimalist interface, AJ-focused design
3. **Backend Engineer** - Python backend, file parsing, RAG pipeline
4. **Frontend Specialist** - Electron/Tauri app, drag-drop UI
5. **Vector Database Expert** - ChromaDB integration, embeddings, RAG
6. **DevOps Engineer** - Container build, distribution, updates
7. **Security Auditor** - Privacy review, threat modeling
8. **Relational Database Expert** - Future: structured data handling
9. **Performance Optimizer** - GPU optimization, query speed
10. **QA Engineer** - Testing across platforms and file types
11. **Documentation Specialist** - User docs in AJ-friendly language

### Supporting Agents
12. **Mad Scientist Intern** - Experimental features (multi-modal, etc.)
13. **Chaos Monkey Intern** - Edge case testing, failure scenarios
14. **Compliance Lawyer** - Privacy regulations, licensing

---

## Conclusion

### The Vision

**For AJ:**
An app that just works. Drop files, ask questions, get answers. Private, fast, simple.

**For Us:**
A privacy-respecting AI appliance that empowers small business owners without exploiting their data or technical inexperience.

**For the World:**
Proof that AI can be powerful, accessible, AND privacy-preserving. You don't need to choose between capability and privacy.

### The Promise

> "Your business data is yours. We built this app to help you understand it, without ever seeing it ourselves. Everything happens on your computer. Nothing is sent anywhere. When you close the app, it's like it was never there. That's our commitment."

---

**ai-way: Your business intelligence, your way. Private. Simple. Powerful.**
