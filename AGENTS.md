# Agent guide

This repository is a Jekyll website. Keep the published site clean and minimal.

## File placement rules

- Put files that are not part of the website into `_local/`.
- Do not reference `_local/` from templates, pages, posts, CSS, or scripts.
- Keep only files needed for the actual site in the project root or standard Jekyll folders.
- If a file is just downloaded research, reference material, temporary branding, or local scratch work, move it to `_local/`.

## Typical examples for `_local/`

- downloaded logo packs
- press kit assets
- screenshots used only for local exploration
- test or draft images not published
- random downloaded files not used in the website

## Keep published content clean

- Do not commit generated or temporary build artifacts unless explicitly required.
- Prefer source files that are actually used by Jekyll.
- If something is not part of the live site, it belongs in `_local/`.
