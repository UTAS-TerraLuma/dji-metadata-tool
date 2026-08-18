> [!CAUTION]
> This has been archived. Use the [NatureScan CLI](https://github.com/UTAS-TerraLuma/naturescan-cli) instead.

# DJI Metadata Tool

A Python tool for extracting and managing metadata from DJI WPML (Waypoint Mission Language) KMZ files. Parses flight mission parameters from DJI drone mission files and writes user-provided metadata to each survey's metadata folder.

## TODO

- [ ] Add tests

## Requirements

- [pixi](https://pixi.sh)

## Install

```sh
git clone https://github.com/UTAS-TerraLuma/dji-metadata-tool
cd dji-metadata-tool
pixi install
```

## Commands

### `kmz` — extract from a file or directory

Extract metadata from a single KMZ file or recursively from a directory:

```sh
pixi run metadata-tool kmz <input.kmz>
pixi run metadata-tool kmz <directory>
```

The tool parses the KMZ (platform, sensor, flight height, speed, overlaps, terrain follow, etc.) and writes a GeoJSON `.json` file next to the input KMZ.

**Example:**

```sh
pixi run metadata-tool kmz kmz_test_files/1_simple/20250724-SAABHC0005-m3m-50mAGL.kmz
```

---

### `survey` — extract for a NatureScan survey

Locate the KMZ for a specific survey in the standard NatureScan directory layout, rename it to `<survey_uid>.kmz` if needed, and extract its metadata:

```sh
pixi run metadata-tool survey <SITE_DATE_TAG>
```

The survey UID must follow the `SITE_DATE_TAG` format, e.g. `NTAFIN_20240925_m3m`.

The tool looks for the KMZ in:

```
NatureScan_drone / SITE / DATE / survey_TAG / metadata /
```

If the JSON already exists it will skip and notify. Use `--overwrite` to force re-extraction.

**Examples:**

```sh
pixi run metadata-tool survey NTAFIN_20240925_m3m
pixi run metadata-tool survey NTAFIN_20240925_m3m --overwrite
```

Pass `all` to process every survey in the NatureScan drone directory:

```sh
pixi run metadata-tool survey all
pixi run metadata-tool survey all --overwrite
```

When using `all`, surveys with no KMZ or multiple KMZs produce a warning and are skipped rather than aborting. A summary of written / skipped / warnings is printed at the end.

---

### `user-metadata` — sync user metadata from Excel

Read the shared `user_metadata.xlsx` spreadsheet and write a `<survey_uid>_user.json` file into each survey's metadata folder:

```sh
pixi run metadata-tool user-metadata
pixi run metadata-tool user-metadata --overwrite
```

The Excel file is read from:

```
NatureScan_drone / user_metadata.xlsx
```

If the JSON already exists it will skip and notify. Use `--overwrite` to force rewrite. A summary of written / skipped / warnings is printed at the end.
