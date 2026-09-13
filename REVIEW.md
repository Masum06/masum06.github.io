# Homepage loading fixes — ready for local review

Branch: `codex/site-loading-fixes`. Base: `9a86112` (Headshot reduced in size).
No push, PR, deployment, DNS change, or new commit was performed.

## Changes

- Host the existing Bootstrap 3.3.6, jQuery 1.12.0, Font Awesome 4.7.0, Philosopher fonts, and ORCID icon locally. Include transitive font assets and upstream license notices. Remove unused Oswald. Defer jQuery and Bootstrap in dependency order.
- Use an optimized progressive JPEG copy: 148,737 bytes versus the current original's 261,094 bytes (43% smaller), retaining its 854 × 981 dimensions, color profile, portrait and alt text. The original JPEG is unchanged. The handoff's 2,924,941-byte original was already reduced by the base commit before this retry.
- Remove the dead Projects navigation entry. History shows the old content was commented out and then removed in `aa60c3c`; it was not restored as current content.
- Remove Teaching from the navigation as requested; retain the existing teaching page file. Preserve Home, Publications, Blogs and CV destinations. Offset the Publications anchor below the fixed navbar.
- Move the highlights script inside the body, expose expanded states, and show navigation and all highlights when scripting is disabled.

## Verification — September 13, 2026

- Actual Chrome rendering: desktop and mobile viewport (390 × 844); portrait, text, local fonts and icons visually inspected. Mobile viewport is emulation, not a physical phone test.
- Mobile navigation opens and closes. Highlights expand and collapse. Publications heading lands 70px below the top. Teaching was tested before its navigation entry was subsequently removed at the user's request.
- Controlled local server blocked external assets through Content Security Policy and delayed local asset responses by 150ms. Page and controls remained functional; no captured console errors in that test. This delay is not a full slow-network bandwidth simulation.
- A separate CSP sandbox test disabled scripting: navigation and all highlights were visible, and inactive toggle controls were hidden.
- Verified 20 local asset paths, including every CSS font reference, return HTTP 200, the expected bytes, and non-HTML content types. All local navigation targets exist; CV returns a valid PDF. `git diff --check` passes.
- All four public URL variants (HTTP/HTTPS, apex/www) finish at `https://masumhasan.com/` with HTTP 200. These are checks of the unchanged live site.
- Google and Cloudflare DNS-over-HTTPS queries return no apex AAAA answers.
- No build system or repository test suite is present. Firefox, Safari/WebKit and Edge were not tested. No new independent certificate scan or native IPv6 connectivity test was performed.

## Pending approval and access

Review these local changes before committing/pushing and creating a PR. After deployment, repeat the live smoke tests.

IPv6 requires the DNS provider account and approval. GitHub's [current DNS documentation](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site) still lists these four AAAA values for host `@`:

```
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

Before applying, record the current provider entries for rollback and confirm the GitHub Pages domain configuration. Preserve existing A records, www CNAME, mail records and nameservers. Recheck authoritative and recursive DNS after propagation, then test from a native IPv6-capable network.

The original browser-specific failure is not proven resolved. Its exact error, browser/version, device and network are still needed to identify the cause. TLS policy and browser security settings were not changed.
