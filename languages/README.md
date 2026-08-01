# MyBI — Languages

Translation files for the [MyBI](https://github.com/My-BI) app. The app **ships only English (UK)**
in its bundle; every other language is **downloaded the first time a user selects it**, from this
folder.

> Translations are **community-contributed and may contain inaccuracies** — only `en-GB` is
> maintained by MyBI. **Contributions to improve them are welcome** (see below).

## The keys ARE the English text

There are no invented key names. The string the app passes to `t()` **is** the en-GB source text, so
a locale file maps English → your language:

```json
{
  "Colour palettes": "Palettes de couleurs",
  "Create custom column": "Créer une colonne personnalisée"
}
```

That is why **`en-GB` has no file** — it is the source. A locale file only needs the strings that
differ from English; anything missing falls back to en-GB, so a partial translation is useful
immediately and never shows a blank or a raw key.

## How the app uses these files

```
https://raw.githubusercontent.com/My-BI/app/main/languages/manifest.json
https://raw.githubusercontent.com/My-BI/app/main/languages/<code>.json
```

1. The app reads `manifest.json` and **lists only the languages marked `enabled`**. Everything else
   stays hidden and the app runs in the language it shipped with.
2. For the selected language it compares that locale's `hash` to what it cached. **Changed →
   download; unchanged → skip.** Only the selected language is ever fetched.
3. Fetched strings are cached locally. Missing keys fall back to en-GB.
4. Under **Cyber lockdown** the app never reaches the network and uses the cached strings.

## `manifest.json`

```json
{
  "version": 2,
  "source": "en-GB",
  "locales": {
    "hi": { "hash": "fa827bbf47d0", "enabled": true,  "keys": 1789, "english": "Hindi",   "native": "हिन्दी" },
    "es": { "hash": "bf21a9e8fbc5", "enabled": false, "keys": 0,    "english": "Spanish", "native": "Español" }
  }
}
```

| Field | Meaning |
| --- | --- |
| `hash` | First 12 hex of the SHA-256 of that locale file. **Bump it whenever you change the file**, or clients keep their cached copy. |
| `enabled` | Whether the app offers this language at all. `false` = the file exists but has no translations yet. |
| `keys` | How many strings are translated — the honest measure of how far along it is. |
| `english` / `native` | The language's name, mirroring the app's own registry. |

A language is only worth listing once it actually translates something, which is what `enabled`
decides. Listing a name that then silently renders English is worse than not listing it at all.

## Files

- One `<code>.json` per language (BCP-47 code) — a flat *English string → translation* map.
  **105 files**; most are still `{}` and therefore disabled.
- `_template.src.json` — the full English vocabulary, grouped into `areas` / `buttons` / `labels` /
  `sentences`, with every value blank. This is the **worksheet to translate from**.

## Contributing a translation

1. Copy `_template.src.json` and fill in the **values** — keep the keys, the `…` ellipsis, and
   `MyBI` (the brand name) exactly as they are.
2. Flatten your filled worksheet into `<code>.json` (one flat object, sections merged). Leave a
   string out rather than shipping an empty value — omitted strings fall back to English.
3. Update that locale in `manifest.json`: new `hash`, new `keys` count, and `enabled: true` once
   there is enough there to be worth offering.
4. Open a pull request. Native-speaker reviews are especially welcome.

The full language list (names, country, flag, continent, RTL) lives in the app's
`src/i18n/languages.ts`.
