# TLI Login Pages

TLI's login pages: static pages that send people on to the real sign-in for a
service, each way in a button. They hold no accounts and do no
authentication themselves (TLI's sign-in plumbing is cmutli-shib-proxy). Built
on CMU's web template and the
[Carnegie Mellon Web Kilt](https://webkilt.andrew.cmu.edu/)'s Sign-In Page.
Static HTML, no build step.

| Page              | Address                                 | Ways in                                                                                                              |
| ----------------- | --------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| `login/learning/` | https://www.tli.cmu.edu/login/learning/ | Google and LinkedIn (through Cirrus Identity), an Andrew account and a Canvas-specific account, for learning.cmu.edu |

Page copy says "Sign In"; addresses say `login`. Link a page by its folder
(`/login/learning/`), never `index.html`.

## Preview

    python3 -m http.server 4200          # http://127.0.0.1:4200/login/learning/

To see an unreleased Web Kilt change, put the Web Kilt's preview proxy in
front of it. From a Web Kilt checkout:

    npm run preview -- --proxy http://127.0.0.1:4200    # http://127.0.0.1:4100/login/learning/

The pages load the Web Kilt 0.8.17 from jsDelivr, pinned with integrity
hashes. For a newer release, change the version in each page's two Kilt
`<link>`s and replace their hashes with the release's (the Web Kilt's
`package.json` lists them).

## Google Tag

Each page has the Google tag for the TLI account's Sign-In Pages property
(Tag Manager container `GTM-5T9BMGB5`). A page with a Google tag keeps the
Legal Info link in its footer.

## Deploying

The TLI fleet serves `/login/` on www.tli.cmu.edu from a tag of this
repository (`profiles/www.tli.cmu.edu.nix` in cmutli-fleet; the site's root
isn't this repository's). Tag a release, bump the fleet's `cmutli-login-pages`
flake input to it and deploy www-01.

## Moving learning.cmu.edu's Sign-In Page Here

The page used to be a copy at
`https://www.eberly.cmu.edu/login/learning.html`. To switch over:

1. In learning.cmu.edu's Canvas authentication settings, change the
   discovery URL to `https://www.tli.cmu.edu/login/learning/`.
2. On the Eberly web server (not in the fleet), redirect
   `/login/learning.html` to `https://www.tli.cmu.edu/login/learning/` for old
   links and bookmarks.
