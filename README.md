# First_Repository

## Automated Phishing Response

`scripts/auto_revoke_reset.py` implements an automated workflow for Thinkst Canary phishing alerts. Given a webhook payload containing a *Cloned Site* URL, the script:

1. Resolves the cloned host and filters for public, non‑allow‑listed IPv4 addresses.
2. Adds those addresses to a Conditional Access named location.
3. Queries Azure AD sign‑in logs for users authenticating from the suspicious IPs.
4. Revokes the affected users' sessions and resets their passwords (forcing change at next sign‑in).

### Prerequisites
- Python 3 with the `requests` package (`pip install requests`)
- An Azure AD application with permissions `User.ReadWrite.All` and `Policy.ReadWrite.ConditionalAccess`
- Environment variables:
  - `AZURE_TENANT_ID`
  - `AZURE_CLIENT_ID`
  - `AZURE_CLIENT_SECRET`
- A Thinkst Canary webhook payload saved to disk

### Usage
```bash
python scripts/auto_revoke_reset.py --webhook webhook.json \
    --named-location-name "Phishing - Blocked IPs" --allow 203.0.113.0/24
```

Optional arguments allow you to specify an existing named location ID, adjust lookup windows, and provide multiple `--allow` CIDRs.
