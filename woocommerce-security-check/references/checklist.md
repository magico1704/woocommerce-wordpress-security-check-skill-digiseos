# Checklist by phase

Skip any line the owner's answers made irrelevant. Mark each item PASS / FAIL / N/A /
NEEDS-OWNER. Keep N/A items in the report as a record.

## Phase 1 — Outside checks (no login)

**Free scanners**
- SSL Labs (target A), securityheaders.com, Sucuri SiteCheck (malware, blacklists),
  internet.nl (website + email), SPF/DKIM/DMARC lookup, CAA and DNSSEC lookup.

**URL tests on every language version**
- `http://` redirects to `https://`.
- `/wp-login.php` and `/wp-admin/` (hidden or protected?), `/xmlrpc.php` (blocked?).
- `/?author=1` and `/wp-json/wp/v2/users` must not reveal usernames.
- `/readme.html`, `/license.txt`, `/.env`, `/.git/`, `debug.log`, and any
  `.zip` / `.sql` / `.bak` files in the web root must not be downloadable.
- `/wp-content/uploads/` must not list files.
- `/wp-content/uploads/wc-logs/` and `/wp-content/uploads/woocommerce_uploads/` must
  not be reachable (logs can hold personal data; the second holds paid downloads).
- Other apps in the same web folder must not open through the main domain
  (e.g. `shop.com/otherapp/admin/`).

**Page source**
- WordPress version, visible plugins and theme; known vulnerabilities.

**Consent**
- As a new visitor: nothing from tracking, pixels or session recording is sent before
  "Accept"; it starts after "Accept". Look for the same tool loaded twice.
- Banner text in every shop language.

**Checkout**
- Which domain receives the card number (provider vs shop)? Card fields built by a
  plugin and posted to the shop's server = Critical.

## Phase 2 — Inside checks (owner logged in, read-only)

**WordPress**
- Users and roles: no shared logins, no leftover staff, Shop Manager instead of
  Administrator where possible, 2FA for every admin and shop manager.
- Plugins: delete inactive, duplicate (two login limiters, two caches, a tool installed
  both as its own plugin and through another plugin) and abandoned ones (no update in
  12 months). Auto-updates on for security and minor releases.
- WordPress and PHP versions supported; `WP_DEBUG` off; file editor disabled;
  security plugin (e.g. Wordfence) firewall on and last scan clean.
- Multilingual: every enabled language has its pages; buttons inside pages point to
  the right language and not to old `/index.php/...` links.

**WooCommerce**
- Settings → Advanced → REST API: remove unused keys; read-only where possible.
- Settings → Advanced → Webhooks: remove unused; delivery URLs are known.
- Settings → Accounts & Privacy: guest checkout and account creation as intended;
  data retention set for inactive accounts, cancelled/failed orders.
- Settings → Products → Downloadable products: protected download method; files not
  in a public folder.
- Payment plugins: live keys only on the live site, test mode off, webhook signing
  secret set.
- Marketing → Coupons: every coupon has an expiry and usage limits.
- CAPTCHA (e.g. Cloudflare Turnstile) on checkout, registration, login and
  "add payment method".

**Hosting panel**
- 2FA on the hosting account (often the host's own login, not cPanel).
- File permissions: folders 755, files 644, `wp-config.php` and other config/database
  files 600. Never 777.
- Stray files in the web root (old backups, zips, `phpinfo.php`).
- Backups: daily, off-server, includes the database; one restore tested.
- SSL certificates: none expired; no manually installed certificates that block the
  host's automatic renewal (AutoSSL).

**Cloudflare (if used)**
- SSL/TLS mode Full (strict) once every origin certificate is valid; minimum TLS 1.2.
- HSTS on (if the button fails, use a Response Header Transform Rule:
  `Strict-Transport-Security: max-age=15552000; includeSubDomains`).
- Security headers (Managed Transforms), "Always Use HTTPS" (only with Full/Full strict).
- Bot protection on; one rate-limiting rule on login and checkout.
- Cache rules never cache cart, checkout, my-account or `?wc-ajax=` requests.
- After deleting a public file, purge that URL from the cache.

**Payment provider dashboards**
- 2FA for every user; least-privilege roles; no shared logins.
- Restricted API keys; old keys revoked.
- Fraud rules / 3-D Secure on; alerts for payout-account changes and disputes.

**Email and domain**
- SPF, DKIM, DMARC (start `p=none` with reports, move to `p=quarantine` after
  2–4 weeks once real senders pass). CAA records list only the certificate issuers in
  use. Domain transfer lock on; 2FA at the registrar and on the reset email.

**SMS (if used)**
- Provider limited to the countries actually served; CAPTCHA before sending; daily
  and monthly spend alerts; small prepaid balance with auto-recharge off or capped.

## Phase 3 — Report

See the format in `SKILL.md`. Include what passed.

## Phase 4 — Fix

High items first, one at a time. Backup confirmed → owner approves → steps → re-test →
mark Fixed or Accepted risk (with the reason).
