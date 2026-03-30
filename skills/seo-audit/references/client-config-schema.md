# SEO Audit — Client Config Schema

Client configs stored at: `~/.claude/skills/seo-audit/clients/{slug}.json`

**File security:** Directory permissions `700`, file permissions `600` (owner read/write only).

## Schema

```json
{
  "slug": "jkkn-institutions",
  "client_name": "JKKN Institutions",
  "industry": "education",
  "delivery_model": "mo",
  "status": "active",
  "urls": [
    { "url": "https://jkkn.ac.in", "label": "Main site" },
    { "url": "https://jkknpc.ac.in", "label": "Pharmacy College" }
  ],
  "competitors": {
    "known": ["https://competitor1.com", "https://competitor2.com"],
    "discovered": []
  },
  "target_queries": [
    { "query": "best pharmacy college Tamil Nadu", "category": "high-intent" },
    { "query": "JKKN reviews", "category": "reputation" }
  ],
  "service_tier": "premium",
  "report_audiences": ["business-owner", "marketing", "it"],
  "gsc_access": { "enabled": true, "property_url": "https://jkkn.ac.in/" },
  "delivery_channel": "whatsapp",
  "regulated_industry": false,
  "created": "2026-03-15",
  "last_audit": null
}
```

## Field Reference

### Required Fields

| Field | Type | Values | Description |
|-------|------|--------|-------------|
| `client_name` | string | — | Company/organization name |
| `urls` | array | URL objects or `[]` | Sites to audit. Empty for competitor-only audit. |
| `service_tier` | string | `basic` or `premium` | Basic = URL only. Premium = URL + GSC + GA. |
| `delivery_model` | string | `pap` or `mo` | One-time (PaP) or monthly retainer (MO). |

### Optional Fields

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `industry` | string | auto-detected | Auto-detected from site content if omitted |
| `competitors` | object | `{known: [], discovered: []}` | Known + auto-discovered competitors |
| `target_queries` | array | `[]` | Queries the client should rank for |
| `gsc_access` | object | `{enabled: false}` | GSC OAuth access (Premium only) |
| `report_audiences` | array | `["business-owner"]` | Which report sections to include |
| `delivery_channel` | string | `whatsapp` | WhatsApp or Email |
| `regulated_industry` | boolean | `false` | Adds compliance disclaimer if true |

### Auto-Generated Fields

| Field | Description |
|-------|-------------|
| `slug` | Lowercase-hyphenated from client name |
| `status` | Defaults to `active`. Values: `active`, `paused`, `cancelled` |
| `created` | ISO date string |
| `last_audit` | ISO date string or null |

## URL Validation Rules

Each URL must:
- Start with `https://`
- Resolve to a public IP
- Reject private ranges: `10.x`, `192.168.x`, `127.0.0.1`
- Reject `localhost`

Validate at `new-client` time AND at audit runtime.

## Query Categories

| Category | Description | Example |
|----------|-------------|---------|
| `high-intent` | Revenue-driving conversion queries | "best pharmacy college Tamil Nadu" |
| `reputation` | Brand/reputation queries | "JKKN reviews" |
| `product` | Specific product/service queries | "B.Pharm admission 2026" |
| `discovered` | Found during audit, not provided by client | (auto-populated) |

## MO Client Storage

For MO clients, configs are ALSO synced to Supabase `clients.config` column (jsonb).
Local files are the source of truth for PaP clients only.
