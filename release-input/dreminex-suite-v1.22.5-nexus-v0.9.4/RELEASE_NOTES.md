# Dreminex Suite 1.22.5 · Nexus 0.9.4

- Fixed the built-in supplier profile PHP parse error that could stop the Suite loader before Supplier Monitor and dashboard classes loaded.
- Preserved automatic ShopBase BD platform slug, URL pattern and price selector defaults.
- Supplier account setup remains idempotent and uses `suppliers:read` / `suppliers:write`; it does not require `approvals:write`.
- Supplier Monitor remains available to WooCommerce managers and administrators, with the existing tabs and security checks.
- Nexus 0.9.4 is the existing signed Android build; no signing identity or application package changed.
- Windows distribution remains disabled.
