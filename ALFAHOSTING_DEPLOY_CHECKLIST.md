# Alfahosting Deploy Checklist

## Main Quest

Before any real relaunch happens, there are two top-priority blockers:

1. the Alfahosting invoice / account state
2. the DNS zone for `asfmedia.org` and `web.asfmedia.org`

If billing is broken, deployment work is secondary.

## Current Reality

- Local repo exists at `~/Repos/asfmedia.org`
- Repo currently has no commits and no visible remote
- Static landing page was cleaned up locally
- `web.asfmedia.org` is live right now, but appears to be an older site

This means the domain cutover is not blocked by design work anymore. It is blocked by hosting/account control.

## Exact Order Of Operations

### 1. Fix Billing First

In Alfahosting:

- verify the customer account is active
- check whether there are unpaid invoices
- pay the invoice if needed
- confirm domain and webspace are not suspended

Do not touch DNS aggressively before this is confirmed.

### 2. Inventory The Current DNS State

Inside Alfahosting DNS management, record the current entries for:

- `@`
- `www`
- `web`
- mail-related records: `MX`, `SPF`, `DKIM`, `DMARC`

Save the old values before changing anything.

Minimum snapshot to capture:

- record type
- host/name
- target/value
- TTL

### 3. Decide The First Cutover Shape

Pick one of these two paths explicitly:

#### Path A: Deploy new static site to `web.asfmedia.org` first

Best when:

- existing root domain must stay stable for now
- you want a safer soft relaunch
- you want to test the git-based deploy flow first

Recommended first DNS intent:

- `web.asfmedia.org` -> new git-based host
- keep root domain unchanged temporarily

#### Path B: Deploy new static site to `asfmedia.org` directly

Best when:

- you want the main brand surface replaced immediately
- the old stack is no longer worth keeping
- you have control over mail and DNS records already

Recommended first DNS intent:

- root domain -> new git-based host
- `www` -> redirect or alias to root

### 4. Prepare The Git Deploy Target

Before changing DNS, the new host must exist.

Checklist:

- create or connect the git remote
- push the repo
- configure the chosen static host
- confirm the preview/build works
- verify the generated site loads before DNS change

### 5. Lower DNS TTL Before The Switch

If possible, lower TTL a few hours before the cutover for the records you plan to change.

Example target:

- TTL from high/default down to something like `300`

That makes rollback and propagation less painful.

### 6. Update Only The Needed Records

Touch only the records required for the chosen cutover.

Likely cases:

- `A` record for root
- `CNAME` for `www`
- `CNAME` for `web`

Do **not** accidentally break:

- mail routing
- SPF/DKIM/DMARC
- unrelated subdomains

### 7. Verify After DNS Change

Check:

- `asfmedia.org`
- `www.asfmedia.org`
- `web.asfmedia.org`
- email still works
- SSL certificate is issued correctly on the new host

### 8. Clean Up Later

Only after the new site is stable:

- remove obsolete DNS entries
- archive old hosting/site paths
- document the final source of truth

## Recommended First Move

Given the current state, the sanest first move is:

1. pay/fix the Alfahosting invoice
2. snapshot all DNS records
3. deploy the new git-based static version to `web.asfmedia.org` first
4. keep `asfmedia.org` untouched until the new flow is proven

That gives you the lowest-risk rollout.

## Fast Data To Collect Next

When you sit down with the Alfahosting panel, collect these exact facts:

- account active: yes / no
- invoice status: paid / unpaid
- nameserver mode: Alfahosting or external
- current `A` record for `@`
- current `CNAME`/`A` for `www`
- current `CNAME`/`A` for `web`
- current mail records still needed: yes / no

Once those are known, the deploy path stops being vague and becomes mechanical.
