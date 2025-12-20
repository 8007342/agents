# Anonymous Data Ingestion: The AI-WAY

**Problem**: Even with local inference, AI-WAY needs fresh data from the internet. But *the queries themselves are the leak*. If AJ's agents request AAPL, TSLA, and BTC prices at 9:15 AM every trading day, any observer (ISP, API provider, CDN, browser telemetry) can infer AJ's portfolio, trading schedule, and sentiment shifts — potentially before AJ consciously acts on them.

**Principle**: Consume bulk. Filter locally. Never reveal what you're looking for by what you request.

---

## The Threat Model

### Query-Pattern Fingerprinting

```
Traditional approach (LEAKY):
  AJ → "Get AAPL price" → API
  AJ → "Get TSLA price" → API
  AJ → "Get BTC price"  → API

  Observer learns: AJ holds AAPL, TSLA, BTC
  Observer learns: AJ checks at 9:15 AM (trader behavior)
  Observer learns: Query frequency increased (AJ is anxious)
  Observer learns: AJ stopped querying TSLA (sold position?)
```

### Who Sees These Patterns?

| Observer | What They See |
|----------|--------------|
| ISP | All DNS queries, connection timing, data volumes |
| API Provider | Exact queries, frequency, user agent, IP |
| CDN/Proxy | Request patterns, geographic inference |
| Browser | Full request history, local storage, fingerprints |
| Network appliances | Traffic analysis, timing correlations |
| Ad networks | Cross-site tracking, behavioral profiles |

### The Aggregation Danger

No single observer has the full picture, but **correlation attacks** can combine:
- ISP knows *when* you connect to finance APIs
- API provider knows *what* you query
- Browser telemetry knows *how long* you look at results
- Ad network knows *what you bought* afterward

Together: complete financial behavioral profile, updated in real-time.

---

## The AI-WAY Solution: Bulk Ingest, Local Filter

```
AI-WAY approach (PRIVATE):
  Firehose → "ALL stock prices" → Local cache
  Firehose → "ALL crypto prices" → Local cache
  Firehose → "ALL news headlines" → Local cache

  Local agent: Filter for AAPL, TSLA, BTC (never leaves device)

  Observer learns: AJ subscribes to market data (so do millions)
  Observer learns: Nothing about specific interests
```

**Key insight**: Download everything. Filter locally. The query pattern reveals nothing because everyone gets the same firehose.

---

## Implementation Patterns

### 1. RSS/Atom Feeds (90s Wisdom)

The original privacy-preserving data ingestion:

```yaml
feeds:
  finance:
    - https://feeds.finance.yahoo.com/rss/2.0/headline  # All headlines
    - https://feeds.reuters.com/reuters/businessNews    # All business

  # NOT this (leaky):
  # - https://api.finance.com/stock/AAPL  # Reveals interest
```

**Pros**: Simple, battle-tested, widely available, truly one-way
**Cons**: Limited data types, not real-time, feeds are dying

### 2. Bulk Data Downloads

Periodic download of complete datasets:

```bash
# Download ALL stock prices (not just yours)
curl https://data.provider.com/eod-all-stocks-2025-01-15.csv.gz

# Download ALL news from last 24h
curl https://archive.org/news-dump-daily.json.gz

# Local filtering reveals nothing to network
ai-way filter --portfolio ~/.private/my-stocks.txt
```

**Pros**: Complete privacy, works offline, cacheable
**Cons**: Bandwidth, storage, staleness

### 3. Broadcast/Multicast Streams

Subscribe to streams where everyone receives everything:

```
┌─────────────────────────────────────────┐
│           DATA BROADCASTER              │
│  (sends same stream to all subscribers) │
└─────────────────┬───────────────────────┘
                  │
      ┌───────────┼───────────┐
      ▼           ▼           ▼
   ┌─────┐    ┌─────┐    ┌─────┐
   │ AJ  │    │User2│    │User3│
   └─────┘    └─────┘    └─────┘

   All receive: [AAPL, GOOG, MSFT, TSLA, ...]
   AJ filters locally for: [AAPL, TSLA]
   Observer sees: AJ subscribed to "market stream" (like everyone else)
```

**Implementations**:
- WebSocket firehoses (Polygon.io full feed)
- Satellite data broadcast (Blockstream Satellite for Bitcoin)
- Multicast DNS/mDNS patterns
- IPFS pubsub channels

### 4. Tor/Onion Routing for Necessary Queries

When bulk isn't available, anonymize the query origin:

```
AJ → Tor Entry → Relay → Relay → Exit → API
                                    ↓
                              API sees: Tor exit IP
                              API learns: Someone asked for AAPL
                              API doesn't know: It was AJ
```

**Pros**: Origin anonymity
**Cons**: Doesn't hide *what* was queried from the API, just *who* asked

### 5. Query Mixing / Differential Privacy

Add noise to real queries:

```python
# AJ wants: AAPL, TSLA
# AI-WAY requests: AAPL, TSLA, GOOG, AMZN, META, NVDA, ...

real_interests = ["AAPL", "TSLA"]
noise = random.sample(ALL_STOCKS, k=50)
query = real_interests + noise  # Observer can't distinguish signal from noise
```

**Pros**: Works with existing APIs
**Cons**: Bandwidth overhead, imperfect (statistical attacks possible)

### 6. Data Cooperatives / Shared Pools

Community members contribute to shared anonymized pools:

```
┌─────────┐  ┌─────────┐  ┌─────────┐
│ Member1 │  │ Member2 │  │ Member3 │
└────┬────┘  └────┬────┘  └────┬────┘
     │            │            │
     └────────────┼────────────┘
                  ▼
         ┌───────────────┐
         │  SHARED POOL  │  (all queries aggregated)
         │  (anonymous)  │
         └───────┬───────┘
                 ▼
         ┌───────────────┐
         │   INTERNET    │  (sees: "cooperative" not individuals)
         └───────────────┘
```

**Examples**: Tor, I2P, private relays, VPN cooperatives

### 7. Air-Gapped Ingestion (Paranoid Mode)

Physical separation for maximum security:

```
Internet Machine          Air Gap          AI-WAY Machine
┌─────────────┐                           ┌─────────────┐
│ Download    │    USB/SD Card            │ Local only  │
│ bulk data   │ ─────────────────────────►│ inference   │
│ (burner)    │    (one-way transfer)     │ (private)   │
└─────────────┘                           └─────────────┘
```

**Pros**: Absolute network isolation for AI-WAY
**Cons**: Manual, inconvenient, not real-time

---

## Recommended Architecture for AI-WAY

### Tier 1: Default (Firehose Subscriptions)

```yaml
# ai-way-data-sources.yaml
ingestion:
  mode: firehose  # Never individual queries

  sources:
    market_data:
      type: websocket_broadcast
      url: wss://data.provider.com/all-markets
      filter_locally: true

    news:
      type: rss_aggregate
      feeds:
        - https://feeds.reuters.com/all
        - https://feeds.ap.org/all
      refresh: 15m

    weather:
      type: bulk_download
      url: https://openweather.org/bulk-daily.json.gz
      schedule: "0 * * * *"  # Hourly
```

### Tier 2: Enhanced (Anonymized Queries)

For data not available in bulk:

```yaml
anonymization:
  method: tor  # or: i2p, mixnet, cooperative

  # Add query noise
  differential_privacy:
    enabled: true
    noise_ratio: 10  # 10 fake queries per real query

  # Randomize timing
  timing_obfuscation:
    enabled: true
    jitter_seconds: [60, 300]  # Random delay
```

### Tier 3: Paranoid (Air-Gapped)

```yaml
ingestion:
  mode: manual
  import_path: /media/usb-import/
  accepted_formats: [json, csv, xml, rss]

  # Verify no exfiltration
  export_blocked: true
  network_disabled: true
```

---

## What AI-WAY Must Never Do

| Anti-Pattern | Why It Leaks |
|--------------|--------------|
| Individual API queries | Reveals specific interests |
| Query timing correlation | Reveals behavior patterns |
| User-Agent strings | Fingerprints the client |
| Persistent connections | Enables session tracking |
| Retry patterns | Reveals importance of data |
| Error handling differences | Side-channel for interest detection |

---

## Data Source Wishlist

Ideal privacy-preserving data sources for AI-WAY:

| Data Type | Ideal Source | Status |
|-----------|--------------|--------|
| Stock prices | Bulk EOD downloads | Available (various) |
| Crypto prices | Blockchain broadcast | Available (Blockstream) |
| News headlines | RSS aggregators | Available (dying) |
| Weather | Bulk API dumps | Available (OpenWeather) |
| Exchange rates | Daily bulk files | Available (ECB, etc.) |
| Company filings | SEC EDGAR bulk | Available |
| Real-time quotes | Firehose WebSocket | Expensive but exists |

---

## The Fundamental Tradeoff

```
Privacy ◄─────────────────────────────► Freshness

Air-gapped          Bulk download         Real-time API
(max privacy)       (good balance)        (zero privacy)
```

AI-WAY should default to **bulk download** and let AJ opt into fresher (leakier) sources only with explicit informed consent.

---

## Open Questions

1. **Bandwidth budget**: How much bulk data is acceptable for AJ's connection?
2. **Staleness tolerance**: How old is "too old" for different data types?
3. **Cooperative infrastructure**: Should AI-WAY help build/join data cooperatives?
4. **Offline-first**: Can AI-WAY work meaningfully without *any* fresh data?

---

## See Also

- [Agent Fingerprinting](../dangers/AGENT_FINGERPRINTING.md) - Behavioral pattern risks
- [Data Leaks & Exfiltration](../dangers/DATA_LEAKS.md) - Metadata exposure
- [Privacy-First Architecture](privacy-first-architecture.md) - Overall security model

---

**Status**: Draft
**Maintained by**: 8007342
**Last Updated**: 2025-12-20
