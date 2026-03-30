# GSC OAuth Setup (Premium Clients)

## Prerequisites

- GCP Project: `jicate-seo-audit` (or existing JICATE project), region `asia-south1`
- OAuth consent screen configured: App name "JICATE SEO Audit"
- OAuth client credentials: "Desktop app" type
- Env vars set: `GSC_CLIENT_ID`, `GSC_CLIENT_SECRET`

## Step-by-Step Flow

### 1. Create OAuth Client (one-time setup)

1. Go to Google Cloud Console > APIs & Services > Credentials
2. Create OAuth 2.0 Client ID > Desktop app
3. Download client credentials
4. Set env vars:
   ```bash
   export GSC_CLIENT_ID="your-client-id"
   export GSC_CLIENT_SECRET="your-client-secret"
   ```

### 2. Enable APIs (one-time)

Enable in the same GCP project:
- Search Console API (`searchconsole.googleapis.com`)
- PageSpeed Insights API (`pagespeedonline.googleapis.com`)

### 3. Client Authorization (per client)

1. Generate consent URL with CSRF `state` parameter:
   ```
   https://accounts.google.com/o/oauth2/v2/auth
     ?client_id={GSC_CLIENT_ID}
     &redirect_uri=http://localhost:8642/callback
     &response_type=code
     &scope=https://www.googleapis.com/auth/webmasters.readonly
     &access_type=offline
     &prompt=consent
     &state={random_32_byte_hex_nonce}
   ```
   **SECURITY:** The `state` parameter MUST be a cryptographically random nonce (e.g., `python3 -c "import secrets; print(secrets.token_hex(32))"`). Verify it on callback to prevent CSRF attacks. Port 8642 chosen to avoid conflicts with common dev servers (3000/5173/8080). Ensure port is free before starting.

2. Send URL to client (or walk them through it in a call)
3. Client grants read-only GSC access
4. Callback receives authorization code
5. Exchange code for tokens via POST to `https://oauth2.googleapis.com/token`

### 4. Token Storage

| Client Type | Storage Method |
|-------------|---------------|
| PaP | macOS Keychain via `security add-generic-password` |
| MO | Supabase Vault (encrypted column) |

**NEVER store tokens in:**
- Plain JSON config files
- Git repositories
- Conversation history
- Environment variables (tokens are per-client, not global)

### 5. Token Lifecycle

- **Refresh:** POST to `https://oauth2.googleapis.com/token` with `grant_type=refresh_token`, `refresh_token`, `client_id`, `client_secret`. Refresh proactively if token expires within 5 minutes.
- **Validation:** Skill checks token validity before each GSC API call
- **Revocation:** Tokens revoked when engagement ends:
  - PaP: after delivery + support period
  - MO: at contract termination

### 6. Security Rules

- **Scope:** Only request `webmasters.readonly` — reject tokens with broader scopes
- **Redirect URI:** `http://localhost:3000/callback` (local callback, no remote server)
- **If client cannot grant access:** Downgrade to Basic tier, document limitation

## GSC API Usage

Once authenticated, query Search Console API for:
- **Performance data:** clicks, impressions, average position, CTR
- **URL inspection:** indexation status per page
- **Sitemaps:** submission status

Endpoint: `https://searchconsole.googleapis.com/webmasters/v3/sites/{siteUrl}/searchAnalytics/query`

Rate limit: 2000 requests/day per project. Sufficient for all client audits.
