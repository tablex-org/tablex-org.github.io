# tablex website

The site at https://tablex-org.github.io: a one-page landing page (`index.html`) and the user docs (`docs/`).

- `index.html`, `assets/`: the landing page, plain HTML and CSS.
- `docs/`: built from `source/docs` with [mdBook](https://rust-lang.github.io/mdBook/): `cd source/docs && mdbook build` (it writes `../../docs`; set `build-dir` accordingly).

The app lives at https://github.com/tablex-org/tablex.
