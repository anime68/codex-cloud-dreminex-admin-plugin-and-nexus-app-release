# Dreminex Suite 1.22.4 · Nexus 0.9.4

- Supplier platform setup now derives a safe platform ID and product URL pattern from the supplier domain.
- ShopBase BD defaults use the HTTPS store pattern and price selectors; advanced URL patterns remain optional.
- Supplier account creation is idempotent, uses `suppliers:write`, and never requires `approvals:write` or sends credentials.
- Successful setup returns a `login_needed` account and opens protected login; retry and Continue login remain available after a launch failure.
- Supplier Monitor now accepts authorized WooCommerce managers and WordPress administrators through one capability mapping, while unauthorized users remain denied.
- Supplier Monitor jobs, scan results, Nexus devices and review actions are available as dashboard tabs.
- Plugin update checks are visible in the Suite dashboard and use the verified public manifest and checksum.
- Windows installer distribution remains excluded from this release.
