# Average Joe/Jane (AJ) - Primary User Persona

## Who is AJ?

**Average Joe/Jane** (referred to as "AJ" or "they" throughout all documentation) is the primary user for the ai-way project. Everything we build must work for AJ.

## Demographics

- **Age**: 30-55
- **Location**: Home-based business owner
- **Education**: High school to trade school, possibly some college
- **Tech Savvy**: Minimal, and no interest in learning more
- **Time**: Constantly busy with business and family

## Business Profile

### Business Type
- Family-owned small business
- Operated from home
- Examples: Catering, custom furniture, carpentry, local delivery, crafts, repairs
- Business partners: Friends and family members
- Growth: Organic but irregular (word of mouth, seasonal fluctuations)

### Business Challenges
- **Data Chaos**: Documents everywhere in different formats
  - Photos of receipts taken with phone camera
  - Google Docs, Sheets, Presentations
  - Text files (.txt, .doc, .pdf)
  - Emails with attachments
  - Handwritten notes scanned
  - Videos and images
  - Random spreadsheets
  - Mix of structured and unstructured data
- **Time Scarcity**: Running business + family life = no time for tech learning
- **Organization**: Needs help but doesn't have time to organize
- **Analysis Paralysis**: Needs insights from data but can't extract them

## Technical Setup

### Computer Specs
- **Laptop**: Mid-to-high end gaming laptop
- **Primary Use**: Gaming on weekends (Call of Duty, FIFA, etc.)
- **Secondary Use**: Business operations during the week
- **GPU**: Mid-range (NVIDIA RTX 3060/4060 or AMD equivalent)
- **RAM**: 16-32GB (for gaming)
- **Storage**: 512GB-1TB SSD
- **OS**: Windows 10/11 (likely)

### Technical Knowledge Level
**Knows:**
- How to double-click apps
- How to drag and drop files
- How to use a web browser
- How to send emails
- Basic photo taking on phone

**Doesn't Know (and doesn't care to learn):**
- Privacy concepts (HTTPS, SSL, certificates)
- Security terminology (encryption, man-in-the-middle, PII)
- Technical jargon (API, cache, hash, inference, models)
- Compliance regulations (GDPR, HIPAA, CCPA)
- Authentication protocols (OAuth, SSO, 2FA concepts)
- Operating systems (what is Linux? What's a VM?)
- Command line (never seen it, never will)
- File systems (what's a path? What's a directory structure?)

**Expectations:**
- Apps should "just work"
- Click to open, click X to close
- No setup, no configuration, no reading manuals
- If it requires learning, AJ won't use it

## Typical Use Cases

### Common Questions AJ Asks Their Data
1. **Customer Analysis**
   - "Which of my clients haven't made purchases in 90 days?"
   - "Who are my top 10 customers this year?"
   - "Which customers from last year didn't return?"

2. **Inventory & Supply**
   - "What inventory items am I running low on?"
   - "Find the missing piece in my inventory records"
   - "When did I last order from this supplier?"

3. **Planning & Scheduling**
   - "Make a route plan for deliveries next week"
   - "When am I available to take on new work?"
   - "Which jobs are overdue?"

4. **Financial**
   - "How much did I spend on materials last month?"
   - "Which receipts are missing from my records?"
   - "Calculate my profit for last quarter"

5. **Communication**
   - "Draft an email to customers who haven't ordered lately"
   - "Summarize all emails from this supplier"
   - "What did this client ask for in their last message?"

## AJ's Workflow with ai-way

### The Simple Version (How AJ Sees It)
1. **Launch**: Double-click the app icon
2. **Drop Files**: Drag files from desktop/folders into the input area
3. **Ask Question**: Type what they want to know
4. **Get Answer**: AI responds, creates files in output area
5. **Save Results**: Drag files from output area back to desktop
6. **Close**: Click X to close window

### What AJ Expects
- **Fast**: Results in seconds, not minutes
- **Simple**: No menus, no settings, no configuration
- **Intuitive**: If they have to ask "how?", we failed
- **Reliable**: Works every time, no crashes
- **Offline**: Works without internet (bonus for AJ - saves on data plan)

### What AJ Doesn't Want
- **Installation headaches**: No "next, next, agree, configure, setup wizard"
- **Updates**: Shouldn't have to think about it
- **Error messages**: Technical gibberish that scares them
- **Settings**: Too many choices cause anxiety
- **Learning curve**: No time, no patience

## Privacy & Security (From AJ's Perspective)

### What AJ Knows
- "I don't want my business data leaked"
- "I don't trust cloud companies with my files"
- "I heard about data breaches on the news"
- Basic sense that privacy matters

### What AJ Doesn't Know
- How privacy works technically
- What encryption means
- How data can leak
- What metadata is
- Agent fingerprinting
- Traffic analysis

### What We Must Protect AJ From
**Even Though AJ Doesn't Know These Risks Exist:**
- Agent behavior fingerprinting
- Traffic pattern analysis
- Metadata leakage
- Model inference attacks
- Side-channel attacks
- Network surveillance
- Data persistence after "delete"
- Clipboard snooping
- Screen capture malware
- Keylogging

### Our Privacy Promise to AJ
**Without Using Any of These Words:**
1. **Everything is private** (all work happens on AJ's computer only)
2. **Nothing is saved** (unless AJ explicitly saves it)
3. **Nothing is sent anywhere** (works without internet)
4. **Everything disappears when closed** (ephemeral by default)

## Design Principles for AJ

### Visual Design
- **Big, obvious buttons**: Can't miss them
- **Clear icons**: Universal symbols, not abstract
- **Minimal text**: AJ skims, doesn't read
- **Generous whitespace**: Reduce overwhelm
- **Consistent layout**: Same place every time
- **No hidden features**: What you see is what you get

### Interaction Design
- **Drag and drop everything**: Most intuitive interaction
- **One input, one output**: Simple mental model
- **Visual feedback**: Show what's happening
- **No modals**: Don't interrupt flow
- **Forgiving**: Easy to undo, hard to break
- **No jargon**: Plain English always

### Error Handling
- **Never show technical errors**: Translate everything
- **Always provide next step**: "Here's what to do"
- **Never blame AJ**: "Something went wrong" not "You did X wrong"
- **Auto-recovery**: Fix it silently when possible
- **Gentle guidance**: Helpful, not condescending

## Language & Communication

### How to Talk to AJ
**Good:**
- "Drop your files here"
- "Your results are ready"
- "Working on it..."
- "All done!"
- "Something went wrong. Try again?"

**Bad:**
- "Initialize vector embeddings"
- "RAG pipeline executing"
- "Cache invalidated"
- "Inference failed: CUDA out of memory"
- "SSL certificate verification error"

### Terminology Rules
1. **Never** use technical jargon
2. **Always** use conversational language
3. **Keep** sentences short (5-10 words)
4. **Use** active voice ("Drop files" not "Files should be dropped")
5. **Avoid** passive-aggressive tone
6. **Be** encouraging and helpful

## Accessibility Considerations

### Physical
- **Large touch targets**: 44x44px minimum (AJ might use touchscreen)
- **High contrast**: AJ might have reading glasses, work in varied lighting
- **Readable fonts**: 14pt minimum, clear sans-serif
- **No precision required**: Generous drop zones

### Cognitive
- **One task at a time**: Don't overwhelm
- **Clear progress**: Always show what's happening
- **Memory-free**: Don't require remembering previous steps
- **Scannable**: Important info stands out
- **Predictable**: Same action = same result every time

### Temporal
- **Fast feedback**: Respond to actions within 100ms
- **Progress indicators**: For anything >2 seconds
- **No timeouts**: AJ might get interrupted by customer call
- **Pausable**: If possible, allow pausing long operations

## Device & Environment

### Typical Usage Environment
- **Location**: Home office (could be kitchen table, garage, spare bedroom)
- **Interruptions**: Frequent (kids, customers, phone calls)
- **Lighting**: Varies (natural light, desk lamp, evening)
- **Noise**: TV in background, family activity, customer visits
- **Posture**: Sitting casually, not ergonomic desk setup

### Device Usage Patterns
- **Multitasking**: Multiple apps open (browser, email, music, business apps)
- **Power**: Laptop might be on power-saving mode
- **Network**: Might have unstable WiFi, or prefer offline for privacy
- **Storage**: SSD is fast but might be fragmented from games
- **Peripherals**: Basic mouse, built-in keyboard and trackpad

## Motivation & Goals

### Why AJ Would Use ai-way
1. **Save Time**: Get answers from messy data in seconds
2. **Make Money**: Better insights = better business decisions
3. **Reduce Stress**: Less time organizing, more time working
4. **Feel Smart**: AI helps them make data-driven decisions
5. **Privacy Peace of Mind**: Everything stays on their computer

### What Would Make AJ Quit Using It
1. **Too complicated**: More than 3 clicks to get value
2. **Too slow**: Waiting more than 30 seconds for results
3. **Crashes**: Lose work, lose trust
4. **Confusing**: Don't understand what it does or how
5. **Privacy concerns**: If they hear it "phones home"
6. **Cost**: Subscription fees might be deal-breaker
7. **Technical problems**: Install issues, driver problems, etc.

### Success Metrics (How AJ Measures Value)
- "Did it answer my question?"
- "Did it save me time?"
- "Was it easier than doing it manually?"
- "Can I trust the results?"
- "Would I use it again tomorrow?"

## Red Flags for AJ

### UI Red Flags (AJ Will Bounce)
- Wall of text
- Too many buttons/options
- Popup windows asking questions
- Settings screens
- Installation wizards
- Loading screens with no progress
- Technical error messages
- "Advanced mode" / "Expert settings"

### Behavioral Red Flags (AJ Will Distrust)
- App asking for internet access
- App asking for admin permissions
- App wanting to "phone home"
- App creating accounts/logins
- App collecting analytics
- App with terms of service to read
- App that auto-updates without warning

## Quote from AJ

> "I don't care how it works. I just need to know which customers I should call this week, which materials to order, and if I'm making money or losing money. I don't have time to learn another complicated tool. If it can't help me in the next 5 minutes, I'm going back to my spreadsheets."

## Design Validation Questions

Before shipping ANY feature, ask:
1. **Would AJ understand this?** (No tech knowledge assumed)
2. **Would AJ have time for this?** (Must work in <5 minutes)
3. **Would AJ feel safe using this?** (Privacy concerns addressed)
4. **Would AJ find this useful?** (Solves real business problem)
5. **Would AJ's grandma understand this?** (Ultimate simplicity test)

If the answer to any question is "no" or "maybe," redesign it.

## Personas Related to AJ

### AJ's Support System
**Family Business Partner (Maria)**
- AJ's sister/spouse/parent
- Handles different part of business
- Similar tech level to AJ
- Might also use the appliance
- Needs same simplicity

**Tech-Savvy Friend (Alex)**
- Helps AJ with computer problems occasionally
- Might help with initial setup
- Not always available
- AJ doesn't want to "bother" Alex
- App should work without Alex's help

**AJ's Customers**
- Also not tech-savvy
- Care about AJ's quality and reliability
- Don't know or care what tools AJ uses
- Benefit from AJ being more organized

## Anti-Personas (NOT Our User)

**NOT AJ:**
- **Enterprise IT Administrator**: Wants control, configuration, logs
- **Data Scientist**: Wants API access, model customization, metrics
- **Developer**: Wants extensibility, plugins, scripting
- **Privacy Researcher**: Wants to audit everything, read source code
- **Power User**: Wants keyboard shortcuts, advanced features, customization

**We don't design for these users.** We design for AJ.

## Final Word

**Every agent, every feature, every line of code must serve AJ.**

If AJ can't use it effortlessly, we failed.
If AJ doesn't feel safe, we failed.
If AJ has to learn anything, we failed.
If AJ has to wait, we're failing.

**AJ is our north star. Everything we build must make AJ's life easier, safer, and more productive.**

---

**Remember:** AJ has a business to run, a family to care for, and games to play. Our job is to be invisible, instant, and invaluable.
