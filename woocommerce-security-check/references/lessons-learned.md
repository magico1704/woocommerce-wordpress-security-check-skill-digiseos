# Lessons learned

From a real audit of a small WordPress site and two hand-built apps in the same
hosting account. When a fix touches one of these areas, include the rule.

| What happened | Rule |
|---|---|
| Pasting code copied the code box's language label (`php`) into a config file; the app went down with a server error | Copy only the code lines, never the label above the box. Reload the site right after saving |
| A secret key was left as a placeholder; every signup would have failed | After editing config, check the file for leftover placeholders |
| An upload reset a config file from 600 to 644 | Re-check permissions after every upload or edit |
| All app files were 777 (writable by anyone on the server) | Folders 755, files 644, config and database files 600 |
| Deleted backup zips stayed downloadable for hours from the CDN cache | After deleting a public file, purge that URL in the CDN |
| A tracking tool was installed twice (own plugin + consent tool), so one copy ran before consent | One way in per tool; test as a new visitor before and after "Accept" |
| A cache plugin was installed but switched off; a login limiter duplicated the security plugin | Delete plugins that do nothing or duplicate another |
| A language had no pages; browser language detection sent visitors to an empty page | Every enabled language needs its pages, or remove the language until it does |
| Buttons inside pages used old `/index.php/...` links and sent other languages to English pages | Check buttons inside pages, not only the menu |
| A public form sent paid SMS codes to ~60 countries with no CAPTCHA | Lock SMS countries, add CAPTCHA, set spend alerts, keep a small prepaid balance |
| Finnish phones showed the SMS sender as "Roskaposti" (Spam) | Since 2 Nov 2026 Finnish operators relabel unverified name senders; use a number sender or get it approved |
| Cloudflare's HSTS button failed to save | Add the header with a Response Header Transform Rule |
| A manually installed certificate expired and never renewed | Remove it so automatic renewal (AutoSSL) takes over; then use Cloudflare Full (strict) |
| Side apps also opened as `main-domain.com/app/`, including an admin page | Block those paths on the main domain, or keep apps outside the main web folder |
| cPanel said 2FA was unavailable | On many hosts 2FA is on the hosting account login, not inside cPanel |
| The security plugin firewall showed 64% | That is the free version's ceiling, not a fault |
| The browser assistant could not see a logged-in tab | It only sees its own tab group; drag the tab into it |
