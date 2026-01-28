# Shopify Developer Skill

A Claude Code skill for Shopify development—themes, Hydrogen, Functions, metafields, and checkout extensibility.

## Install

```bash
npx skills add cathrynlavery/shopify-developer-skill
```

**Recommended:** Also install Context7 for real-time API docs:
```bash
npx skills add intellectronica/agent-skills --skill context7
```

## What It Does

- **Verifies current APIs** before coding via Context7 (prevents deprecated patterns)
- **Workflow decision trees** for theme vs headless vs Functions
- **DO/DON'T patterns** for common Shopify mistakes
- **Progressive disclosure** - loads references only when needed

## The API Drift Test

Before implementing any Shopify feature, the skill verifies current patterns:

| Common Drift | Old Pattern | Current Pattern |
|--------------|-------------|-----------------|
| Checkout | `checkout.liquid` | Checkout UI Extensions |
| Delete metafield | `metafieldDelete` | `metafieldsDelete` |
| Metafield access | `product.metafields.ns.key` | `product.metafields.ns.key.value` |

## What's Included

```
├── SKILL.md                     # Main skill (~200 lines)
└── references/
    ├── liquid-patterns.md       # Metafields, loops, performance
    ├── apis-functions.md        # Storefront API, Functions, Extensions
    ├── section-settings.md      # All 20+ setting types
    └── platform-limits.md       # Limits and breaking changes
```

## Coverage

- Online Store 2.0 themes & Liquid
- Hydrogen 2.0 & Oxygen hosting
- Shopify Functions (discounts, delivery, payments)
- Metafields & Metaobjects
- Checkout Extensibility
- B2B Wholesale features
- Storefront & Admin APIs

## Platform Limits (2024-2025)

| Limit | Value |
|-------|-------|
| Variants per product | 100 |
| Sections per template | 100 |
| Metafields per resource | 200 |
| Functions execution | <5ms |

## License

MIT

## Author

[@cathrynlavery](https://twitter.com/cathrynlavery) • [BestSelf.co](https://bestself.co)
