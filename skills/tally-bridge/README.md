# Tally Bridge

**Sync TallyPrime accounting data to a cloud dashboard — automatically.**

For businesses running TallyPrime, this bridges the gap between desktop accounting and cloud visibility. Your Tally data syncs to a live dashboard your team can access from anywhere.

## What the Client Gets

- **Cloud dashboard** with synced TallyPrime data (vouchers, ledgers, reports)
- **Auto-sync agent** that runs continuously on their machine
- **Mobile-friendly access** — check numbers from anywhere
- **No manual exports** — data flows automatically

## How It Works

```
TallyPrime (client's machine)
        ↓
   Bridge Agent (Node.js, auto-start)
        ↓
   Cloud Dashboard (Vercel + Supabase)
        ↓
   Client views data from anywhere
```

Two-step provisioning:
1. **CBO's laptop** → `new-client` creates cloud account + API key
2. **Client's machine** → `setup` installs bridge agent + syncs data

## Commands

| Command | What it does |
|---------|-------------|
| `new-client` | Create cloud account (run on CBO's laptop) |
| `setup` | Install bridge on client machine + first sync |
| `sync` / `resync` | Re-run data synchronization |
| `status` | Check sync health |
| `debug` | Diagnose connectivity issues |

## Battle-Tested

This is JICATE's most mature skill — **8+ fresh-eyes reviews**, real client deployments, known-fixes database from production use. The service-skill template was extracted from this skill.

---

**Part of [JICATE Marketplace](https://github.com/Ommsharravana/jicate-marketplace)**
