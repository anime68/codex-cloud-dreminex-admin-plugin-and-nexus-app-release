# Dreminex Suite 1.22.8 · Nexus 0.9.4

- Fixed built-in supplier profile synchronization so SHA-256 configuration hashes remain full strings and revisions remain numeric.
- Existing supplier platform/account records are preserved; the sync repairs the stored built-in profile metadata on the next staging/admin load.
- Supplier account setup includes the explicit idempotency schema migration and uses `suppliers:read` / `suppliers:write` without `approvals:write`.
- Supplier Monitor remains available to WooCommerce managers and administrators with the existing tabs and security checks.
- Nexus 0.9.4 remains the existing signed Android build; no signing identity or application package changed.
- Windows distribution remains disabled.
