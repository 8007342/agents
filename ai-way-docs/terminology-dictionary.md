# Terminology Dictionary: Technical → User-Friendly

## Purpose

This dictionary translates technical terms into language Average Joe (AJ) understands. **All agents must use AJ-friendly terms when communicating with users.**

---

## Critical Rule

**NEVER use the technical term with AJ. ALWAYS use the user-friendly alternative.**

---

## Application & System

| Technical Term | AJ-Friendly Term | Example Usage |
|----------------|------------------|---------------|
| Appliance / Virtual Machine / Container | **App** | "Open the app" |
| Fedora Silverblue / Linux / OS | **App** (hide entirely) | Never mention |
| Docker / Podman / Virtualization | **App** | Never mention |
| Image / ISO / Disk Image | **App installer** (only if needed) | "Download the app" |
| Runtime / Environment | (hidden concept) | Never mention |
| Launch / Execute / Run | **Open** | "Open the app" |
| Terminate / Kill Process | **Close** | "Close the app" |
| Boot / Initialize | **Starting...** | "Starting..." |
| Shutdown / Halt | **Closing...** | "Closing..." |

## Files & Data

| Technical Term | AJ-Friendly Term | Example Usage |
|----------------|------------------|---------------|
| Input Box / Upload Area / Drop Zone | **Drop files here** | "Drop your files here" |
| Output Box / Download Area / Results | **Your results** | "Your results are ready" |
| Drag and Drop / File Transfer | **Drop** or **Drag** | "Drag files here" |
| Upload / Import | **Drop files in** | "Drop files to get started" |
| Download / Export | **Save** or **Drag out** | "Drag files to save them" |
| File Path / Directory | **Folder** | "Your files" |
| Extension / File Type | **File type** (only if necessary) | "PDF file" |
| Attachment / Embedded File | **File** | "Your files" |
| Binary / Executable | **App** or **Program** | "The app" |

## AI & Processing

| Technical Term | AJ-Friendly Term | Example Usage |
|----------------|------------------|---------------|
| LLM / Large Language Model | **AI** | "The AI is reading..." |
| Inference / Model Prediction | **Thinking** or **Working** | "Working on it..." |
| RAG / Retrieval-Augmented Generation | (hidden) | "Looking at your files..." |
| Embeddings / Vectors | (hidden) | Never mention |
| Vector Database | (hidden) | Never mention |
| Model / Weights / Parameters | **AI** | "The AI" |
| Token / Context Window | (hidden) | Never mention |
| Prompt / Query / Input | **Question** or **Request** | "What would you like to know?" |
| Response / Output / Completion | **Answer** | "Here's what I found" |
| Fine-tuning / Training | (hidden) | Never mention |
| GPU / CUDA / ROCm | (hidden) | Never mention |
| CPU Inference | (hidden, fallback) | Never mention |
| Quantization / Precision | (hidden) | Never mention |
| Batch Processing | **Working on multiple files** | "Looking at all your files..." |

## Memory & Storage

| Technical Term | AJ-Friendly Term | Example Usage |
|----------------|------------------|---------------|
| RAM / Memory | (hidden) | Never mention |
| tmpfs / RAM disk | (hidden) | Never mention |
| Ephemeral Storage | **Temporary** | "This will disappear when you close" |
| Persistent Storage | **Saved** | "This will be saved" |
| Cache / Caching | (hidden) | Never mention |
| Buffer | (hidden) | Never mention |
| Swap / Page File | (hidden) | Never mention |
| Disk Space / Storage | **Space** | "Not enough space" (rare) |
| Write / Save to Disk | **Save** | "Save your file" |
| Read / Load from Disk | **Open** | "Opening your file" |

## Security & Privacy

| Technical Term | AJ-Friendly Term | Example Usage |
|----------------|------------------|---------------|
| Encryption / SSL / TLS | **Private** or **Secure** | "Your data is private" |
| Certificate / X.509 | (hidden) | Never mention |
| Man-in-the-Middle | (hidden) | Never mention |
| Authentication / OAuth | (hidden, no accounts!) | Never mention |
| Authorization | (hidden) | Never mention |
| PII / Personal Information | **Your information** | "Your private information" |
| Data Breach / Leak | **Leaked** | "Keep your information safe" |
| Firewall / Network Security | (hidden) | Never mention |
| Sandboxing / Isolation | **Separate** | "Keeps everything separate" |
| Air-gapped / Offline | **No internet needed** | "Works without internet" |
| Zero-knowledge / End-to-end | (hidden) | Never mention |
| Metadata | **Information about your files** | Avoid if possible |
| Fingerprinting | (hidden) | Never mention |
| Traffic Analysis | (hidden) | Never mention |

## Network & Connectivity

| Technical Term | AJ-Friendly Term | Example Usage |
|----------------|------------------|---------------|
| Offline Mode | **Works without internet** | "No internet needed" |
| Local / On-device | **On your computer** | "Stays on your computer" |
| Cloud / Remote Server | **Online** or **Internet** | "Requires internet" (we avoid) |
| API / Endpoint | (hidden) | Never mention |
| HTTP / HTTPS | (hidden) | Never mention |
| Request / Response | (hidden) | Never mention |
| Latency / Ping | **Speed** | "Working faster..." (rare) |
| Bandwidth | (hidden) | Never mention |
| Proxy / VPN | (hidden) | Never mention |
| DNS / IP Address | (hidden) | Never mention |

## Performance & Status

| Technical Term | AJ-Friendly Term | Example Usage |
|----------------|------------------|---------------|
| Processing / Computing | **Working** | "Working on it..." |
| Loading / Initializing | **Starting** or **Getting ready** | "Getting ready..." |
| Indexing / Parsing | **Reading your files** | "Reading your files..." |
| Rendering / Generating | **Creating** | "Creating your document..." |
| Optimization | **Making it faster** | Never mention unless relevant |
| Performance / Throughput | **Speed** | "Working faster" |
| Benchmark | (hidden) | Never mention |
| Bottleneck | **Slow down** | Avoid technical diagnosis |
| Crash / Fatal Error | **Stopped working** | "Something went wrong" |
| Hang / Freeze | **Not responding** | "Taking longer than expected" |
| Memory Leak | (hidden) | Never mention (just fix it) |

## Configuration & Settings

| Technical Term | AJ-Friendly Term | Example Usage |
|----------------|------------------|---------------|
| Configuration / Config | **Settings** (minimize!) | "Settings" (only if unavoidable) |
| Parameter / Flag | (hidden) | Never expose to AJ |
| Default / Preset | **Standard** or (hidden) | Use best defaults, don't mention |
| Advanced Options | (hidden entirely!) | Never show AJ |
| Debug Mode | (hidden) | Never expose |
| Verbose / Logging | (hidden) | Never expose |
| Environment Variable | (hidden) | Never expose |
| Registry / Config File | (hidden) | Never expose |

## Updates & Maintenance

| Technical Term | AJ-Friendly Term | Example Usage |
|----------------|------------------|---------------|
| Update / Patch | **Improvement** | "New improvement available" |
| Version / Build | (hidden usually) | "Newer version available" |
| Dependency | (hidden) | Never mention |
| Package / Library | (hidden) | Never mention |
| Migration / Upgrade | **Update** | "Updating..." |
| Rollback / Downgrade | (hidden) | Auto-handle errors |
| Changelog / Release Notes | **What's new** | "What's new" (keep very short) |

## Errors & Problems

| Technical Term | AJ-Friendly Term | Example Usage |
|----------------|------------------|---------------|
| Error / Exception / Failure | **Problem** or **Something went wrong** | "Something went wrong" |
| Timeout | **Taking too long** | "This is taking longer than expected" |
| Out of Memory | **Need more space** | "Not enough room to work" |
| Permission Denied | **Can't access that** | "Can't open that file" |
| File Not Found | **Can't find that file** | "Can't find that file" |
| Invalid Input | **Can't read that** | "Can't read that file" |
| Unsupported Format | **Can't work with that type** | "Can't read that file type" |
| Stack Trace / Debug Info | (hidden!) | Never show to AJ |
| Exit Code / Error Code | (hidden!) | Never show to AJ |
| Failed to connect | **Can't reach that** | Should never happen (offline!) |

## AI-WAY Internal Architecture (Hidden)

These are **internal development terms**. AJ never sees these — they interact with friendly abstractions.

| Internal Term | What AJ Experiences | Notes |
|---------------|---------------------|-------|
| Conductor / Meta-agent | **Yollayah** (default) | Nahuatl: "heart that goes with you." AJ can rename to anything they like. |
| Ambient Companion | **Yollayah** | Technical term for the avatar presence across screens. AJ just sees their companion. |
| Agent / Specialist Agent | **Helper** or (hidden) | "I'll get some help with that" — AJ doesn't need to know about agent architecture |
| Agent handoff | (hidden) | Seamless. AJ just sees continuous conversation. |
| Agent routing | (hidden) | Automatic. AJ never chooses agents manually. |
| Context scope | (hidden) | Memory just works. AJ doesn't manage scopes. |
| Session / Task / Global context | (hidden) | Implementation detail |
| Firehose / Bulk ingestion | (hidden) | Data just appears fresh. AJ doesn't know about ingestion patterns. |
| Query-pattern fingerprinting | (hidden) | Privacy protection is automatic, not explained. |
| Asimov's Laws | (hidden) | Safety is built-in, not advertised. |
| Ephemeral storage | **"Gone when you close"** | Simple explanation only if AJ asks where data went |
| System prompt | (hidden) | AJ doesn't know about prompt engineering |
| RAG pipeline | **"Looking at your files"** | Just describe the action, not the mechanism |
| Embeddings / Vectors | (hidden) | Never mention |
| Local inference | **"On your computer"** | Only if AJ asks about privacy |

### The Naming Principle

The default name is **Yollayah** (Nahuatl: "heart that goes with you").

- AJ can rename it to anything they prefer
- The name persists across sessions (if AJ enables memory)
- The AI responds to its given name
- We **never** call it "Conductor" or "Meta-agent" in UI

**Example interaction:**
> AJ: "Hey Yollayah, can you help with my taxes?"
> Yollayah: "Sure! Let me look at your documents..."
> *(internally: Conductor routes to tax-specialist agent, AJ sees continuous Yollayah conversation)*

## Technical Concepts (Hidden)

These concepts should NEVER be mentioned to AJ in any form:

| Never Say This | Why |
|----------------|-----|
| Virtual Machine | Too technical, not relevant to AJ |
| Hypervisor | Completely hidden concept |
| GPU Acceleration | Works automatically or doesn't |
| CUDA / ROCm / OpenCL | Driver details hidden |
| Inference Engine | Implementation detail |
| Model Quantization | Optimization detail |
| Context Window | LLM limitation (handle gracefully) |
| Token Limit | LLM limitation (handle gracefully) |
| Embeddings | Vector concept too abstract |
| Semantic Search | Just works, no need to explain |
| Dockerfile / Compose | Build process hidden |
| Container Registry | Distribution hidden |
| Operating System | Abstraction layer hidden |
| Kernel / Driver | Too low-level |
| Mount / Bind | File system concept |
| Namespace / cgroup | Linux internals |
| iptables / Firewall | Security implementation |
| SELinux / AppArmor | Security framework |

## Progress & Feedback Messages

### Good Examples (What to Say to AJ)

**Starting:**
- "Getting ready..."
- "Starting up..."
- "One moment..."

**Working:**
- "Working on it..."
- "Reading your files..."
- "Looking at your documents..."
- "Thinking..."
- "Almost done..."

**Success:**
- "All done!"
- "Your results are ready"
- "Here's what I found"
- "Done!"

**Waiting:**
- "This might take a minute..."
- "Working on [X] files..."
- "Reading large file..."

**Errors:**
- "Something went wrong"
- "Couldn't read that file"
- "Try again?"
- "Need a different file type"

### Bad Examples (Never Say This)

❌ "Initializing LLM runtime..."
❌ "Loading model weights into VRAM..."
❌ "Generating embeddings for documents..."
❌ "Executing RAG pipeline..."
❌ "CUDA out of memory error"
❌ "Parsing exception at line 42"
❌ "API rate limit exceeded"
❌ "SSL handshake failed"

## File Type Simplification

| Technical Term | AJ-Friendly Term |
|----------------|------------------|
| .docx / .doc | Word document |
| .xlsx / .xls | Spreadsheet |
| .pptx / .ppt | Presentation |
| .pdf | PDF |
| .txt | Text file |
| .jpg / .jpeg / .png / .gif | Photo or Image |
| .mp4 / .mov / .avi | Video |
| .mp3 / .wav | Audio |
| .zip / .tar.gz / .7z | Compressed files |
| .csv | Spreadsheet |
| .json / .xml / .yaml | Data file (if must mention) |
| .html | Web page |
| .eml / .msg | Email |

## Action Verbs (User-Facing)

| Technical Verb | AJ-Friendly Verb |
|----------------|------------------|
| Execute | Run or Open |
| Terminate | Close or Stop |
| Initialize | Start or Get ready |
| Process | Work on or Read |
| Parse | Read |
| Render | Create or Show |
| Compile | Prepare (hidden) |
| Deploy | Install (rare) |
| Authenticate | Sign in (we don't do this!) |
| Validate | Check |
| Sanitize | Clean up |
| Normalize | Fix |

## Tone Guidelines

### Do Say:
- "Drop your files here"
- "Working on it"
- "All done!"
- "Here's what I found"
- "Try again?"
- "One moment"
- "Almost ready"

### Don't Say:
- "Please upload files to the input buffer"
- "Processing batch job"
- "Operation completed successfully"
- "Query results returned"
- "Retry operation?"
- "Awaiting initialization"
- "Optimization in progress"

---

## Usage Guidelines for Agents

### When Writing Code
1. **All user-facing strings** must use AJ-friendly terms
2. **All error messages** must be translated
3. **All logs visible to users** must be simplified (or hidden)
4. **All tooltips** must use plain language

### When Writing Documentation
1. **User manuals** = AJ-friendly terms only
2. **Developer docs** = technical terms OK
3. **Agent prompts** = understand both, speak AJ-friendly to users

### When Designing UI
1. **Button labels** = verbs AJ knows (Open, Close, Save)
2. **Status messages** = conversational (Working..., Done!)
3. **Placeholders** = helpful hints (Drop files here)
4. **Icons** = universal symbols (X for close, ⬇ for save)

### When Handling Errors
1. **Never** show stack traces
2. **Never** show error codes
3. **Always** explain what went wrong simply
4. **Always** suggest what to do next
5. **Never** blame AJ

---

## Testing the Dictionary

### The Grandma Test
If you can't explain it to AJ's grandma, rephrase it.

### The 5-Year-Old Test
If a smart 5-year-old can't understand it, simplify more.

### The Busy Test
If AJ has to stop and think about what it means, we failed.

---

## Maintenance

This dictionary will evolve as we:
- Discover new technical concepts that need translation
- Get feedback from actual AJs using the app
- Find better, simpler ways to say things

**Rule:** When in doubt, use simpler words.

---

**Remember: AJ doesn't want to learn our language. We must speak AJ's language.**
