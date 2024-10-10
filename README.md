# Forecast Extension Specification

- **Title:** Forecast
- **Identifier:** <https://stac-extensions.github.io/forecast/v0.1.0/schema.json>
- **Field Name Prefix:** forecast
- **Scope:** Item, Collection
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @m-mohr

This document explains the Forecast Extension to the [SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.
It defines some high-level fields to get a basic understanding of **weather** forecast data.
Some fields may also be applicable for climate forecast data, but it hasn't been written specifically for that domain.

- Examples:
  - [Item example for an specific time](examples/item.json): An example STAC Item for a forecast covering a specific instance in time
- [JSON Schema](json-schema/schema.json)
- [Changelog](./CHANGELOG.md)

## Fields

The fields in the table below can be used in these parts of STAC documents:
- [ ] Catalogs
- [x] Collections
- [x] Item Properties (incl. Summaries in Collections)
- [x] Assets (for both Collections and Items, incl. Item Asset Definitions in Collections)
- [ ] Links

| Field Name                   | Type   | Description |
| ---------------------------- | ------ | ----------- |
| forecast:reference_datetime  | string | **REQUIRED.** The *reference* datetime: i.e. predictions for times after this point occur in the future. Predictions prior to this time represent 'hindcasts', predicting states that have already occurred. This must be in UTC. It is formatted according to [RFC 3339, section 5.6](https://tools.ietf.org/html/rfc3339#section-5.6). |
| forecast:horizon             | string | The time between the reference datetime and the forecast datetime. Formatted as [ISO 8601 duration](https://en.wikipedia.org/wiki/ISO_8601#Durations), e.g. `PT6H` for a 6-hour forecast. |
| forecast:duration            | string | If the forecast is not only for a specific instance in time but instead is for a certain period, you can specify the length here. Formatted as [ISO 8601 duration](https://en.wikipedia.org/wiki/ISO_8601#Durations), e.g. `PT3H` for a 3-hour accumulation. If not given, assumes that the forecast is for an instance in time as if this was set to `PT0S` (0 seconds). |
| forecast:param               | string | Name of the model parameter that corresponds to the data, e.g. `T` (temperature), `P` (pressure), `U`/`V`/`W` (windspeed in three directions). |
| forecast:mode                | string | Denotes whether the data corresponds to the control run or perturbed runs. Allowed values are `ctrl` or `perturb`. |
| forecast:member              | integer | Specifies the member (sample number) of perturbed runs, e.g. `1`. |
| forecast:level               | integer | Index of the vertical level of the height coordinate system used in the forecast model, e.g. `4`. |

### Additional Fields from other extensions

| Field Name        | Type   | Description |
| ----------------- | ------ | ----------- |
| datetime          | string | **REQUIRED.** The forecast datetime. It follows the definition in the [STAC Common Metdata](https://github.com/radiantearth/stac-spec/blob/master/item-spec/common-metadata.md#date-and-time). If the forecast is not only for a specific instance in time but instead is for a certain period, you should use `start_datetime` and `end_datetime` and set `datetime` either to reflect the `start_datetime` (recommended) or to `null`. |
| start_datetime / end_datetime | string | The forecast start and end datetime. It follows the definition in the [STAC Common Metdata](https://github.com/radiantearth/stac-spec/blob/master/item-spec/common-metadata.md#date-and-time). Only use these fields if the forecast is for a period (and as such `forecast:duration` is not `PT0S`). |
| expires           | string | The datetime until the forecast is valid or gets superseded by a new forecast. It follows the definition in the [Timestamps Extension](https://github.com/stac-extensions/timestamps#item-properties-fields). |
| deprecated        | string | Set this to `true` if a newer version of the forecast is available. It follows the definition in the [Version Extension](https://github.com/stac-extensions/timestamps#item-properties-fields). |

**Note:** The fields mentioned above don't use the `forecast:` prefix!

It is also recommended to implement the [Version Extension](https://github.com/stac-extensions/version)
and use it to "deprecate" old forecasts and link between forecasts with the same forecast datetime using the given
[relation types](https://github.com/stac-extensions/version#relation-types).

## Media Types

You can use any file format, but here is a list of common media types for forecast assets that have not been listed yet in the list of [common media types in STAC](https://github.com/radiantearth/stac-spec/blob/master/best-practices.md#common-media-types-in-stac):

| Type                    | Description |
| ----------------------- | ----------- |
| `application/wmo-GRIB2` | GRIB2 file (usually with the file extension `.grb2` or `.grib2`) |
| `application/x-netcdf`  | NetCDF file (usually with the file extension `.nc`) |

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
