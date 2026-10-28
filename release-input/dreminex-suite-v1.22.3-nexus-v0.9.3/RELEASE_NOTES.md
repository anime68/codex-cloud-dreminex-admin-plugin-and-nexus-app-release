# Dreminex Suite 1.22.3 · Nexus 0.9.3

- Supplier account setup now requires `suppliers:write` and no longer incorrectly requires `approvals:write`.
- Successful account creation immediately opens the protected HTTPS login flow.
- Failed login launch keeps the account visible as **Login needed** with a **Continue login** action.
- Setup requests are idempotent, preventing duplicate accounts from repeated taps or retries.
- Supplier Monitor is again configurable from the Dreminex Suite module manager.
- Supplier Monitor now uses the universal supplier account architecture without importing retired configuration.
- Existing navigation, website data, product workflows, approval workflows, and signing identity are unchanged.
