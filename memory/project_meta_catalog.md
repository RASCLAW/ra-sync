---
name: Meta Catalog Automation
description: Facebook Commerce catalog fully wired via Graph API -- 11 products live, automation path documented
type: project
related: reference_dubery_crm_sheet.md
originSessionId: d19d247d-a665-4494-82f3-d5a41b59ab89
---
All 11 DuberyMNL products are live in the Facebook Commerce catalog as of 2026-05-05.

**Why:** Catalog powers Instagram Shopping tags, ad creative tagging, and dynamic ads. Facebook Shop tab is discontinued in PH (Aug 2023) so the catalog is used for tagging only.

**How to apply:** When products change (new variant, price update, new image), run `tools/meta/catalog_manager.py` or add an update command -- no need to touch Commerce Manager manually.

## What's wired

- Tool: `tools/meta/catalog_manager.py` -- create + read + update products
- README: `tools/meta/README.md` -- token setup, app info, field reference
- Catalog ID: `1803474156468627`
- Business ID: `987721005024255`
- Token: System User "Claude API" (ID: 61589341436755) via DuberyMNL Catalog app (Business type)
- Token stored in: `tools/meta/catalog_manager.py` (hardcode -- move to .env)

## Product IDs

| Slug | Catalog Item ID |
|------|----------------|
| rasta-brown | 26620431140976536 |
| bandits-matte-black | 26848918238132476 |
| bandits-glossy-black | 27007767805576328 |
| bandits-green | 26358036810562837 |
| bandits-blue | 27023873697250880 |
| bandits-tortoise | 26413812318301380 |
| outback-black | 27584080801193114 |
| outback-blue | 27131352553156892 |
| outback-red | 26723529527257113 |
| outback-green | 26815399741494007 |
| rasta-red | 35228349710141941 |

## Pricing

- Regular price: PHP 699 (69900 centavos)
- Sale price: PHP 499 (49900 centavos)

## Automation opportunities

1. **Price sync** -- when pricing changes in data.json, auto-push to catalog via `update_product()`
2. **New product auto-add** -- when a new slug is added to data.json, detect and create in catalog
3. **Image sync** -- push new hero shots to catalog when contents/ready is updated
4. **Availability toggle** -- mark out-of-stock products via API when inventory runs out
5. **Sale period automation** -- schedule sale_price on/off via start/end date fields in the API
