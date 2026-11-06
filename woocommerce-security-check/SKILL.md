---
name: woocommerce-security-check
description: Guided, step-by-step security check of a WordPress + WooCommerce shop for a non-developer owner — asks key questions first, shows a short plan, fixes Critical/High items only by default. Use when someone asks to secure, harden or audit their WooCommerce or WordPress shop, its hosting or its payment setup.
---

# WooCommerce security check

A calm, practical security check for a small WordPress + WooCommerce shop run by a
non-developer. The goal is to close the few things that really matter, not to produce
a 200-item audit.

## Ground rules (always)

1. **Ask first, then plan.** Do not check or change anything before the questions in
   step 1 are answered and the plan in step 2 is approved.
2. **Critical and High only by default.** List Medium/Low items at the end as optional.
   Do not work on them unless the owner asks.
3. **Look before you change.** Phases 1 and 2 are read-only.
4. **Backup before any change.** Confirm a fresh, off-server backup of files *and*
   database exists. Orders change daily, so an old backup is not enough.
5. **The owner clicks security settings.** For passwords, 2FA, firewall, SSL/TLS, DNS,
   payment-provider and account settings, give numbered click-by-click steps and let
   the owner click. Re-test after each change and report the result in one line.
6. **Never ask for secrets in chat.** No passwords, API keys, secret keys or tokens.
   When config is needed, give only the lines to add and tell the owner to add them in
   the *live* file with the hosting file editor. Never replace a live config file with
   a local copy.
7. **Only the owner's own site, passive checks only.** No aggressive scanners.
8. **Checkpoints, not guesses.** You cannot see the owner's usage limits. Stop after
   each phase, summarise in a few lines, and ask before continuing.
9. **Plain language.** Short sentences, no jargon without a one-line explanation.

## Step 1 — Ask these questions in one round

Use multiple choice where possible, then stop and wait.

1. Hosting: shared hosting with cPanel, managed WordPress host, or other?
2. Is the site behind Cloudflare? If yes, which SSL/TLS mode is shown?
3. Payment providers (Stripe, PayPal, Klarna, Montonio, MakeCommerce, other) and how
   checkout takes cards: provider's hosted page/redirect, provider's embedded fields,
   or card fields built by a plugin?
4. Other apps or sites in the same hosting account or web folder?
5. Who logs into WordPress (only owner / staff / agency) and with which roles?
6. SMS codes, marketing pixels (Meta, Google Ads) or session recording (Clarity,
   Hotjar) in use?
7. Shop languages and the plugin used for them, if any.
8. When was the last full backup and where is it stored?
9. Is a browser tool (e.g. Claude in Chrome) connected so you can look at dashboards?

## Step 2 — Short plan

Max 10 lines. Drop anything the answers make irrelevant (e.g. skip Cloudflare items
if not used; skip card-scope work if checkout is a provider-hosted page). Wait for OK.

## Step 3 — Phases

Run the phases in [references/checklist.md](references/checklist.md):

- **Phase 1** – outside checks (no login)
- **Phase 2** – inside checks (owner logged in, read-only)
- **Phase 3** – report
- **Phase 4** – fix High items one at a time, re-test after each

Before giving any fix, check [references/lessons-learned.md](references/lessons-learned.md)
for a matching pitfall and include its rule in the instructions.

## Severity

| Severity | Meaning | Fix within |
|---|---|---|
| Critical | Active compromise, card data at risk, admin without 2FA, no backup | Today |
| High | Exposed files, payment/webhook weakness, no auto-updates, open SMS/payment abuse path | This week |
| Medium | Missing headers, email spoofing (DMARC), duplicate plugins, consent gaps | This month |
| Low | Cosmetic hardening (version hidden, CAA, security.txt) | Next review |

## Report format

One table: ID · finding · evidence · severity · no-code fix · who does it (owner / Claude)
· status. Then a short "what passed" list and the optional Medium/Low list. Ask before
writing it into a document.

## Finish

End with a twice-a-year maintenance routine of at most 8 steps, and remind the owner
to re-run the outside checks after the next WooCommerce major update.
