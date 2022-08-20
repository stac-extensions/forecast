# Forecast Extension Specification

- **Title:** Forecast
- **Identifier:** <https://stac-extensions.github.io/forecast/v1.0.0/schema.json>
- **Field Name Prefix:** forecast
- **Scope:** Item, Collection
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @m-mohr

This document explains the Forecast Extension to the [SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.

- Examples:
  - [Item example](examples/item.json): Shows the basic usage of the extension in a STAC Item (todo)
  - [Collection example](examples/collection.json): Shows the basic usage of the extension in a STAC Collection (todo)
- [JSON Schema](json-schema/schema.json) (todo)
- [Changelog](./CHANGELOG.md) (todo)

## Fields

The fields in the table below can be used in these parts of STAC documents:
- [ ] Catalogs
- [x] Collections
- [x] Item Properties (incl. Summaries in Collections)
- [x] Assets (for both Collections and Items, incl. Item Asset Definitions in Collections)
- [ ] Links

| Field Name                   | Type   | Description |
| ---------------------------- | ------ | ----------- |
| forecast:datetime            | string | The forecast datetime, which must be in UTC. It is formatted according to [RFC 3339, section 5.6](https://tools.ietf.org/html/rfc3339#section-5.6). |
| forecast:horizon             | string | The time between the reference datetime and the forecast datetime. Formatted as [ISO 8601 duration](https://en.wikipedia.org/wiki/ISO_8601#Durations), e.g. `PT6H` for a 6-hour forecast. |
| forecast:accumulation_period | string | If the forecast is not only for a specific instance in time but instead is for an accumulation over a certain period you can specify the length here.Formatted as [ISO 8601 duration](https://en.wikipedia.org/wiki/ISO_8601#Durations), e.g. `PT3H` for a 3-hour accumulation. If not given, assumes that the forecast is for an instance in time as if this was set to `P0TS` (0 seconds). |

One of the fields `forecast:datetime` or `forecast:step` is **REQUIRED**!

### Additional Fields from other extensions

| Field Name        | Type   | Description |
| ----------------- | ------ | ----------- |
| datetime          | string | **REQUIRED.** The reference datetime. It follows the definition in the [STAC Common Metdata](https://github.com/radiantearth/stac-spec/blob/master/item-spec/common-metadata.md#date-and-time). Alternatively, `start_datetime` and `end_datetime` can also be used. |
| expires           | string | The datetime until the forecast is valid or gets superseded by a new forecast. It follows the definition in the [Timestamps Extension](https://github.com/stac-extensions/timestamps#item-properties-fields). |
| deprecated        | string | Set this to `true` if a newer version of the forecast is available. It follows the definition in the [Version Extension](https://github.com/stac-extensions/timestamps#item-properties-fields). |

It is also recommended to implement the [Version Extension](https://github.com/stac-extensions/version)
and use it to "deprecate" old forecasts and link between forecasts with the same forecast datetime using the given
[relation types](https://github.com/stac-extensions/version#relation-types).

## Media Types

You can use any file format, but here is a list of common media types for forecast assets that have not been listed yet in the list of [common media types in STAC](https://github.com/radiantearth/stac-spec/blob/master/best-practices.md#common-media-types-in-stac):

| Type                    | Description |
| ----------------------- | ----------- |
| `application/wmo-GRIB2` | GRIB2 file (usually with the file extension `.grb2` or `.grib2`) |
| `application/netcdf`    | NetCDF file (usually with the file extension `.nc`) |

## Contributing

All contributions are subject to the
[STAC Specification Code of Conduct](https://github.com/radiantearth/stac-spec/blob/master/CODE_OF_CONDUCT.md).
For contributions, please follow the
[STAC specification contributing guide](https://github.com/radiantearth/stac-spec/blob/master/CONTRIBUTING.md) Instructions
for running tests are copied here for convenience.

### Running tests

The same checks that run as checks on PR's are part of the repository and can be run locally to verify that changes are valid. 
To run tests locally, you'll need `npm`, which is a standard part of any [node.js installation](https://nodejs.org/en/download/).

First you'll need to install everything with npm once. Just navigate to the root of this repository and on 
your command line run:
```bash
npm install
```

Then to check markdown formatting and test the examples against the JSON schema, you can run:
```bash
npm test
```

This will spit out the same texts that you see online, and you can then go and fix your markdown or examples.

If the tests reveal formatting problems with the examples, you can fix them with:
```bash
npm run format-examples
```
