# albanian-macos-dictionary - Fjalori Shqip for Apple Dictionary

An offline, monolingual Albanian explanatory dictionary for the Dictionary app on macOS. It contains 35,617 entries with Albanian descriptions, based on the open FjaloriShqip project. It is not an English–Albanian translator.

## Features

- 35,617 Albanian headwords and descriptions.
- Lookup aliases for included inflected and dialect forms, plus unambiguous unaccented aliases for words containing **ë** and **ç**. Displayed headwords retain their Albanian spelling.
- Definitions are Albanian explanatory text. English glosses from the source API are not used as definitions.
- Offline lookup after installation.

Some entries use shorter Albanian descriptions when a fuller description is unavailable. The source describes the Albanian text as written with AI assistance; it has not been independently human-reviewed as a complete lexicographic work. See the [data quality report](reports/data-quality-report.md) for counts, examples, and limitations.

## Install the dictionary

1. Obtain the compiled `Fjalori Shqip.dictionary` bundle from a project release, or build it using the instructions below.
2. In Finder, choose **Go → Go to Folder…**, enter `~/Library/Dictionaries`, and copy `Fjalori Shqip.dictionary` into that folder.
3. Open the **Dictionary** app. Choose **Dictionary → Settings…** and enable **Fjalori Shqip**.
4. Search for an Albanian word, or select a word in an app and choose **Look Up** from its context menu.

The bundle is a macOS folder package ending in `.dictionary`. When sharing it directly, compress the package into a ZIP first.

## Build from source

Requirements:

- macOS
- Python 3.10 or later (the project scripts use only the standard library)
- Apple's Dictionary Development Kit (DDK), included in **Additional Tools for Xcode**

Build the normalized data, Apple Dictionary XML, and compiled bundle:

```sh
DDK_PATH="/path/to/Dictionary Development Kit" scripts/build.sh
```

To also install the validated bundle for the current macOS user:

```sh
DDK_PATH="/path/to/Dictionary Development Kit" scripts/build.sh --install
```

The install command writes to `~/Library/Dictionaries/Fjalori Shqip.dictionary`; it does not install into a system-wide dictionary folder. The project validates its source data and compiled bundle structure. On the target Mac, enable the source in Dictionary settings and try representative entries such as `punë`, `gjuhë`, and `shqip`.

To keep generated data and compiler output on another volume, set `FJALORI_BUILD_DIR` to a writable directory there:

```sh
FJALORI_BUILD_DIR="/Volumes/External-Drive/FjaloriShqip-build" \
DDK_PATH="/path/to/Dictionary Development Kit" \
scripts/build.sh
```

The source snapshots remain in the repository; generated normalized data, DDK XML, build objects, and the compiled bundle go under `FJALORI_BUILD_DIR`.

To regenerate and validate the cached source without compiling a bundle:

```sh
python3 scripts/normalize.py
python3 scripts/generate_dictionary_xml.py
python3 scripts/validate.py
```

## Data sources, credits, and licenses

The dictionary adapts Albanian descriptions and associated lookup data from [FjaloriShqip](https://fjalorishqip.org/), [its source and license notes](https://fjalorishqip.org/burimet/), and [its GitHub repository](https://github.com/EndritRushiti/FjaloriShqip). The upstream project credits multiple sources, including Wiktionary, Wikidata Lexemes, Wikifjalor, and historical Albanian dictionaries. Sources vary by entry; the available API snapshot does not provide complete per-entry source arrays.

The exported dictionary data is adapted from material distributed under **CC BY-SA 4.0**. Redistributed adaptations must retain attribution, identify changes, and use the same license. The export excludes eight headwords marked GPL-2.0-or-later by the upstream API. See [ATTRIBUTION.md](ATTRIBUTION.md) for detailed credits, modifications, exclusions, and source terms, and [LICENSE](LICENSE) for the code and data license notices.

The build scripts in this repository are offered under the MIT License. Apple's Dictionary Development Kit is Apple's software and is not included here.

## Project files

- `data/source/` — cached FjaloriShqip API and description data used to reproduce the export.
- `data/excluded-gpl-headwords.txt` — GPL-marked source headwords omitted from the dictionary export.
- `scripts/` — normalization, XML generation, build, and validation scripts.
- `reports/data-quality-report.md` — data counts and limitations.
- `ATTRIBUTION.md` — detailed attribution and license scope.

## Refreshing the data

The included snapshot was retrieved on 2026-10-23; the API reports a dataset build date of 2026-10-15. To update it, refresh the cached API files and Albanian descriptions from the official FjaloriShqip sources, review license exceptions and attribution, then rebuild and validate the dictionary. Do not assume the current counts or license exceptions apply to a future data release.
