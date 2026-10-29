# Dreminex Suite 1.22.10 · Nexus 0.9.4

- Native MCP now reads the live WordPress Abilities Registry and keeps registered, internal, globally exposed, client-discoverable and executable states separate.
- Added sandbox, read-only, auto-review and full-access policy profiles with explicit OAuth reauthorization for missing scopes.
- High-risk scopes remain explicit opt-in; destructive and high-impact actions stay behind the approval queue.
- Added provider, category and individual ability controls with stable diagnostics and actionable reason codes.
- Added owner-managed reviewed metadata policies and bounded skills for registered abilities; no PHP upload, eval or arbitrary code execution is allowed.
- Internal transport helpers remain hidden from public discovery.
- The plugin no longer bundles an APK, Windows installer or app manifest. App distribution reads the verified public release manifest.
- Nexus 0.9.4 is unchanged in this release and retains its existing signing identity.
