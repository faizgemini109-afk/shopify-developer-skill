# Shopify Platform Limits & Updates

## Table of Contents
- [Current Limits](#current-limits)
- [API Changes 2024-2025](#api-changes-2024-2025)
- [Plan-Specific Features](#plan-specific-features)
- [Debugging Tips](#debugging-tips)
- [Official Resources](#official-resources)

## Current Limits

### Products & Variants
| Limit | Value |
|-------|-------|
| Variants per product | 100 (increased from 50) |
| Options per product | 3 |
| Images per product | 250 |
| Tags per product | 250 |

### Theme Limits
| Limit | Value |
|-------|-------|
| Total theme size | 50MB |
| Single file size | 2MB |
| Sections per template | 100 |
| Blocks per section | 50 |
| Settings per section | 25 |

### Metafields
| Limit | Value |
|-------|-------|
| Metafields per resource type | 200 |
| Metafield value size | 512KB |
| Namespace length | 20 chars |
| Key length | 30 chars |

### API Rate Limits
| API | Limit |
|-----|-------|
| Storefront API | 60 requests/second |
| Admin API (REST) | 40 requests/second |
| Admin API (GraphQL) | 50 points/second |
| Shopify Functions | <5ms execution |

### Cart & Checkout
| Limit | Value |
|-------|-------|
| Line items per cart | 500 |
| Discount codes per order | 1 |
| Discount codes stacking | Plus only |

## API Changes 2024-2025

### Breaking Changes

**August 2024**
- `checkout.liquid` deprecated → use Checkout Extensibility
- All stores must migrate checkout customizations

**2025 API Updates**
- `metafieldDelete` deprecated → use `metafieldsDelete`
- Private/public metafield permissions removed
- Metaobject webhook support for third-party apps
- List-type metafields support multiple values

### New Features

**2024**
- Shopify Functions expanded (cart transforms, validation)
- Hydrogen 2.0 stable release
- Oxygen hosting for all plan tiers
- B2B features expanded to all plans
- 100 variant limit (up from 50)

**2025**
- Enhanced semantic search (Plus)
- Improved checkout extensibility
- New metaobject patterns

## Plan-Specific Features

### Basic / Shopify / Advanced
- Online Store 2.0 themes
- Storefront API access
- Oxygen hosting
- B2B basic features
- Standard checkout

### Shopify Plus Only
- Shopify Functions
- Semantic Search
- Checkout customization (full)
- Launchpad
- Script Editor
- Multi-store management
- Advanced Flow workflows
- Discount stacking

### B2B (All Plans)
- Company accounts
- Volume pricing
- Net payment terms
- Up to 10K ship-to locations per company
- B2B checkout flows

## Debugging Tips

### Performance Issues
1. Check Liquid loops - use `limit:` and `where:` filters
2. Avoid nested loops when possible
3. Use Shopify's Web Performance dashboard
4. Test with Theme Inspector Chrome extension
5. Monitor Core Web Vitals

### Common Errors

**"Theme has too many sections"**
- Max 100 sections per template
- Split into multiple templates or reduce sections

**"Metafield not found"**
- Check namespace/key spelling
- Verify metafield definition exists
- Use `.value` accessor: `{{ product.metafields.custom.key.value }}`

**"Rate limit exceeded"**
- Implement exponential backoff
- Cache API responses
- Batch operations where possible

**"Function execution timeout"**
- Functions must complete in <5ms
- Simplify logic, reduce iterations
- Pre-compute values in metafields

### Testing Checklist
- [ ] Test on development store first
- [ ] Create theme duplicates for major changes
- [ ] Test across product types and variants
- [ ] Test with various customer groups (guest, logged in, B2B)
- [ ] Verify mobile responsiveness on actual devices
- [ ] Check Core Web Vitals before/after

## Official Resources

### Documentation
- **Liquid Reference**: https://shopify.dev/docs/api/liquid
- **Theme Dev Guide**: https://shopify.dev/docs/themes
- **Storefront API**: https://shopify.dev/docs/api/storefront
- **Admin API**: https://shopify.dev/docs/api/admin-graphql
- **Functions**: https://shopify.dev/docs/api/functions
- **Hydrogen**: https://shopify.dev/docs/custom-storefronts/hydrogen

### Updates & News
- **Changelog**: https://changelog.shopify.com
- **Editions**: https://www.shopify.com/editions
- **Developer Blog**: https://shopify.engineering

### Tools
- **Shopify CLI**: `npm install -g @shopify/cli`
- **Theme Inspector**: Chrome extension
- **GraphiQL Explorer**: In Partner Dashboard
- **Polaris**: https://polaris.shopify.com

### Support
- **Partner Slack**: Join via Partner Dashboard
- **Community Forums**: https://community.shopify.com
- **Partner Support**: Via Partner Dashboard
