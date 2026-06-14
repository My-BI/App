# MyBI — Languages

Translation files for the [MyBI](https://github.com/My-BI) app. The app **ships only English (UK)**
in its bundle; every other language is **downloaded the first time a user selects it**, from this
repo.

> Translations are **community-contributed and may contain inaccuracies** — only `en-GB` is
> maintained by MyBI. **Contributions to improve them are welcome** (see below).

## How the app uses these files

```
https://raw.githubusercontent.com/My-BI/app/main/languages/manifest.json
https://raw.githubusercontent.com/My-BI/app/main/languages/<code>.json
```

1. On launch (and when the language changes) the app reads `manifest.json` and compares the selected
   locale's hash to what it cached. **If it changed → download; else → don't.** Only the **selected**
   language is fetched.
2. Fetched strings are cached locally. Missing keys fall back to `en-GB`, then the raw key.
3. Under **Cyber lockdown** the app never reaches the network and uses bundled/cached strings.

## `manifest.json`

```json
{ "version": 1, "locales": { "en-GB": "<sha256-12>", "es": "<sha256-12>", "…": "…" } }
```

Each value is the first 12 hex of the SHA-256 of that locale file. **Bump it whenever you change a
locale file** so clients pick up the update.

## Files

- One `<code>.json` per language (BCP-47 code), a flat `key → string` map.
- **95 languages** have a file; `en-GB` is the source of truth. A handful of languages (e.g. Navajo,
  Quechua, Guarani, Aymara) are only partially translated and fall back to `en-GB` for missing keys.
- Keys must match `en-GB.json`. The current set is the starter UI vocabulary (`app.*`, `nav.*`,
  `action.*`, `common.*`); it grows as the app migrates strings to `t(...)`.

## Contributing a translation

1. Open the `<code>.json` for your language (or copy `en-GB.json` to a new code).
2. Translate the **values** only — keep the keys, the `…` ellipsis, and `MyBI` (brand) unchanged.
3. Recompute that locale's hash in `manifest.json` (first 12 hex of its SHA-256).
4. Open a pull request. Native-speaker reviews are especially welcome.

The full language list (names, country, flag, continent, RTL) lives in the app's
`src/i18n/languages.ts`.
