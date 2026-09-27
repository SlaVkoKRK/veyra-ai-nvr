# VEYRA 1.2.111

## Camera UI continuity + compact Risk Zone classes + GitHub API update checks

- Risk Zone class selection now uses the same compact add/remove workflow as Objects instead of permanently rendering every model class.
- Risk Zone selector/canvas labels show the notification repeat interval immediately, e.g. `Drzwi wejĹ›ciowe Â· co 10 s`.
- Switching cameras in Camera Settings preserves the active section. Masks additionally preserve the selected mask type (Motion/Object/Manual GLARE/Risk Zone).
- Switching cameras while in Debug preserves Debug mode and the selected image pipeline stage.
- Update checks now prefer the GitHub Contents API for `VERSION`, avoiding raw/CDN propagation delay. The existing raw GitHub URL remains an automatic fallback for API errors or rate limits.
