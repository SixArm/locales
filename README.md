# Locales

A table of 305 locales with metadata, language names, and IANA time zone names, each translated into the locale's own language using Unicode CLDR.

The same data is published in three formats:

- `locales.tsv`: tab-separated values (source of truth).
- `locales.csv`: comma-separated values, converted from the TSV.
- `locales.json`: an array of objects, converted from the CSV. Empty cells are `null`, and `L1`, `L2`, and `L1+L2` are integers.

Each row is one locale. Each file has 769 columns in this order.

## Metadata columns (13)

| Column           | Meaning                                               | Example |
| ---------------- | ----------------------------------------------------- | ------- |
| `lowercase_slug` | Lowercase locale identifier, unique per row           | `cy-gb` |
| `bcp47`          | BCP 47 language tag                                   | `cy-GB` |
| `iso639_1`       | ISO 639-1 language code                               | `cy`    |
| `iso639_3`       | ISO 639-3 language code                               | `cym`   |
| `script`         | ISO 15924 script                                      | `Latn`  |
| `region`         | ISO 3166-1 alpha-2 or UN M.49 region (`001` is world) | `GB`    |
| `direction`      | Text direction, `ltr` or `rtl`                        | `ltr`   |
| `L1`             | Estimated first-language speakers                     |         |
| `L2`             | Estimated second-language speakers                    |         |
| `L1+L2`          | Sum of `L1` and `L2`                                  |         |
| `endonym`        | The locale's name in its own language                 |         |
| `t_language`     | The word for "language" in the locale                 | `Iaith` |
| `t_languages`    | The word for "languages" (plural) in the locale       | `Ieithoedd` |

`t_language` and `t_languages` sources:

- `t_language` comes from the CLDR `codePatterns` "language" label for 270 locales. For the rest, and for `ky` and `xh`, where CLDR gives a possessive form or untranslated English, it was written by hand.
- `t_languages` is not in CLDR, so every plural was written by hand (by Claude), matching the casing of `t_language`. Treat these as less authoritative than CLDR cells.
- Empty cells mean no confident translation: `t_language` for `arn`, `kl`, `moh` and `sms`, and `t_languages` for those and `kok`.

## Exonym columns (305)

`exonym-<slug>` holds the locale's name as written in the locale `<slug>`.

For example some of the locale codes for one of the IANA time zones:

| Locale code | America/New_York          |
| ----------- | ------------------------- |
| en-us       | United States/New York    |
| es-es       | Estados Unidos/Nueva York |
| hi-in       | संयुक्त राज्य/न्यूयॉर्क   |
| zh-cn       | 美国/纽约                 |

Some cells are empty where no translation is available.

## Time zone columns (451)

One column per IANA time zone ID, such as `Europe/Paris` or `America/New_York`. Each cell is the time zone's country and city translated into the row's locale, joined with `/`, for example `Ffrainc/Paris` in `cy-gb`.

- Time zone IDs come from CLDR `bcp47/timezone.json`, which uses CLDR's canonical IDs and so includes legacy names such as `Europe/Kiev` and `Asia/Calcutta`.
- Country names come from CLDR `territories` and city names from CLDR `timeZoneNames` `exemplarCity`, from CLDR 49.0.0.
- Lookup tries the locale's `bcp47`, then language plus script, then language alone. A missing country falls back to English, and a missing city falls back to the last part of the ID with underscores as spaces. About 47% of city cells use that fallback, usually because the name equals the ID, so they are not always true translations.
- Zones with no country, such as `Etc/UTC`, show the city part only.

## Regenerating

Edit `locales.tsv`, then regenerate `locales.csv` and `locales.json` from it.
