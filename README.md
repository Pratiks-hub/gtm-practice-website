# Northstar / Lab — GTM Practice Store

A free static e-commerce test website. All products and orders are simulated. No payment processing or backend.

## Run locally

Open `index.html` in a browser, or run `python3 -m http.server 8000` from this directory and open http://localhost:8000.

## Install Google Tag Manager

1. Create a **Web** container at https://tagmanager.google.com.
2. Open `index.html` and find the commented GTM script in `<head>`.
3. Replace `GTM-XXXXXXX` with your real container ID.
4. Remove `<!--` and `-->` around that GTM script block. Do not remove the opening dataLayer initialization script.
5. Optionally add the GTM noscript iframe immediately after `<body>` as provided by GTM.
6. Publish your changes, then test with GTM Preview / Tag Assistant.

**Note:** Do not paste GA4 Measurement IDs (G-...) where a GTM container ID (GTM-...) is required.

## Publish free with GitHub Pages

1. Create a public GitHub repository, e.g. `gtm-practice-website`.
2. Upload the contents of this folder to the repository root (`index.html`, `style.css`, `script.js`, `README.md`).
3. Go to Settings > Pages > Build and deployment > Deploy from a branch, select `main` and `/ (root)`, then Save.
4. Your site will be available at `https://YOUR-USERNAME.github.io/gtm-practice-website/` once deployed.

## dataLayer events

| Event | Trigger | Important fields |
| --- | --- | --- |
| `site_init` | Initial load | `page_type`, `site_section` |
| `page_view_custom` | Initial load | `page_title`, `page_location` |
| `view_item_list` | Product grid renders/filters/searches | `ecommerce.items` |
| `select_item` | Open product | `ecommerce.items` |
| `view_item` | Open product | `ecommerce.currency`, `ecommerce.items` |
| `add_to_cart` | Add to bag | `ecommerce.value`, `ecommerce.items` |
| `view_cart` | Open bag | `ecommerce.items` |
| `remove_from_cart` | Remove one item | `ecommerce.items` |
| `begin_checkout` | Checkout | `ecommerce.value`, `ecommerce.items` |
| `add_shipping_info` | Submit checkout | `ecommerce.shipping_tier` |
| `purchase` | Submit checkout | `ecommerce.transaction_id`, `ecommerce.value`, `ecommerce.items` |
| `search` | Search (debounced) | `search_term` |
| `generate_lead` | Newsletter form | `form_id`, `form_name` |
| `scroll_depth` | 25/50/75/90% | `percent_scrolled` |
| `cta_click` | Hero CTA | `cta_name`, `cta_location` |
| `navigation_click` | Header navigation | `navigation_item` |
| `select_content` | Category filter | `content_type`, `item_id` |

## Suggested GTM setup

1. Create a GA4 Google tag with your GA4 Measurement ID and Initialization or All Pages trigger (according to your setup).
2. Create a Custom Event trigger for `add_to_cart` and a GA4 Event tag named `add_to_cart`.
3. Create a Data Layer Variable with name `ecommerce.items` (Data Layer Version 2) and use it as an event parameter named `items`.
4. Add `currency` and `value` variables using `ecommerce.currency` and `ecommerce.value`.
5. Repeat for `view_item`, `begin_checkout`, and `purchase`, using the corresponding event parameters.
6. Inspect browser DevTools `window.dataLayer` and GTM Preview to validate the event payloads.

This example is designed for **web GTM**. To practice server-side GTM, create a separate Server container and provision a tagging server. Configure a GA4 web tag to send hits to your server endpoint and inspect incoming requests in Server Preview. A server container alone is not a deployed tagging server; hosting may cost money.

## Important safety and data notes

- Never enter real payment details or sensitive personal data; form fields are intentionally not sent anywhere.
- This site has no consent management platform. Add a proper consent implementation before using it with real visitors or advertising tags.
- The product visuals are emoji placeholders to keep the demo lightweight and self-contained.
- The event monitor logs only events pushed through this site's `track()` helper, not every event GTM itself adds to `dataLayer`.
