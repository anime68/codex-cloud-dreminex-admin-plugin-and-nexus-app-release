# Dreminex Suite 1.22.6 · Nexus 0.9.4

- Added the additive supplier account schema migration for existing installations missing `setup_request_id`.
- Supplier setup retries are now idempotent on upgraded staging sites and do not create duplicate platform or account records.
- Fixed the built-in supplier profile PHP parse error that could stop the Suite loader before Supplier Monitor and dashboard classes loaded.
- Preserved automatic ShopBase BD platform slug, URL pattern and price selector defaults.
- Supplier account setup uses `suppliers:read` / `suppliers:write` and never requires `approvals:write`.
- Supplier Monitor remains available to WooCommerce managers and administrators with existing tabs and security checks.
- Nexus 0.9.4 remains the existing signed Android build; no signing identity or application package changed.
- Windows distribution remains disabled.
