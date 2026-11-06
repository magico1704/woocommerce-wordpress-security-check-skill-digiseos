# WooCommerce Security Check — a Claude skill

A guided security check for small WordPress + WooCommerce shops, written for owners who
are not developers. Claude asks a few questions first, shows a short plan, and works
through the Critical and High items with you, one at a time, stopping before every
change.

Built from a real audit; the pitfalls we hit are in
[`woocommerce-security-check/references/lessons-learned.md`](woocommerce-security-check/references/lessons-learned.md).

## Install

**Claude app (web or desktop)**
1. Download this repository as a ZIP (green **Code** button → **Download ZIP**) and
   unzip it.
2. Zip only the `woocommerce-security-check` folder.
3. In Claude, open **Settings → Capabilities → Skills** and upload that ZIP.

**Claude Code**
```bash
git clone https://github.com/magico1704/woocommerce-wordpress-security-check-skill-digiseos.git
cp -r woocommerce-wordpress-security-check-skill-digiseos/woocommerce-security-check ~/.claude/skills/
```

## Use

Ask Claude something like: *"Check the security of my WooCommerce shop
https://my-shop.com"*. Have a fresh backup and your logins ready. Never paste passwords
or API keys into the chat.

## What's inside

```
woocommerce-security-check/
├── SKILL.md                     # rules, first questions, phases, report format
└── references/
    ├── checklist.md             # what to check in each phase
    └── lessons-learned.md       # real pitfalls and the rule for each
```

## Disclaimer

This is guidance, not a guarantee. Always take a backup before changes, and for card
payments follow your payment provider's own security requirements.

## License

MIT — see [LICENSE](LICENSE).
