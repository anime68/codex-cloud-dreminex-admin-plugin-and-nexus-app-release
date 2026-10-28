# Dreminex Suite 1.22.7 · Nexus 0.9.4

- Added an explicit additive migration for existing supplier account tables: `setup_request_id` and its unique idempotency index are created when absent.
- Supplier account setup now succeeds on upgraded installations while preserving existing accounts and platform data.
- Fixed the built-in supplier profile PHP parse error that could stop the Suite loader before Supplier Monitor and dashboard classes loaded.
- Preserved automatic ShopBase BD platform slug, URL pattern and price selector defaults.
- Supplier account setup uses `suppliers:read` / `suppliers:write` and never requires `approvals:write`.
- Supplier Monitor remains available to WooCommerce managers and administrators with existing tabs and security checks.
- Nexus 0.9.4 remains the existing signed Android build; no signing identity or application package changed.
- Windows distribution remains disabled.
