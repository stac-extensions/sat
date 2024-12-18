# Satellite Extension Specification

- **Title:** Satellite
- **Identifier:** <https://stac-extensions.github.io/sat/v1.0.0/schema.json>
- **Field Name Prefix:** sat
- **Scope:** Item, Collection
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Candidate
- **Owner**: @emmanuelmathot
- **History**: [Prior to March 2, 2021](https://github.com/radiantearth/stac-spec/commits/v1.0.0-rc.1/extensions/sat)

This document explains the Satellite Extension to the [SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.
It adds metadata related to a satellite that carries an instrument for collecting data.
It will often be combined with other extensions that describe the actual data, such as the [EO](https://github.com/radiantearth/stac-spec/tree/master/extensions/eo) or 
[SAR](https://github.com/stac-extensions/sar) extensions.

- Examples:
  - [example-landsat8.json](examples/example-landsat8.json): Shows the basic usage of the extension in a STAC Item
  - [example-sentinel1.json](examples/example-sentinel1.json): Shows the more advanced usage of the extension in a STAC Collection
- [JSON Schema](json-schema/schema.json)
- [Changelog](./CHANGELOG.md)

## Item Properties

| Field Name                            | Type                  | Description                                                                                                                                                                                             |
| ------------------------------------- | --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| sat:platform_international_designator | string                | The International Designator, also known as COSPAR ID, and NSSDCA ID                                                                                                                                    |
| sat:orbit_state                       | string                | The state of the orbit. Either `ascending` or `descending` for polar orbiting satellites, or `geostationary` for geosynchronous satellites                                                              |
| sat:absolute_orbit                    | integer               | The absolute orbit number at the time of acquisition.                                                                                                                                                   |
| sat:relative_orbit                    | integer               | The relative orbit number at the time of acquisition.                                                                                                                                                   |
| sat:orbit_cycle                       | integer               | The number of repeat cycle done by the satellite at the time of the acquisition. [Repeat cycle](https://ltb.itc.utwente.nl/page/498/concept/81577) is the time between two successive identical orbits. |
| sat:orbit_state_vectors               | Map<string, \[number]> | The state vectors of the satellite at the time of acquisition.                                                                                                                                         |
| sat:anx_datetime                      | string                | The [Ascending Node](https://en.wikipedia.org/wiki/Orbital_node) Crossing (ANX) time, in UTC. It is formatted according to [RFC 3339, section 5.6](https://tools.ietf.org/html/rfc3339#section-5.6).    |

*At least one of the fields must be specified.*

### Additional Field Information

#### sat:platform_international_designator

The [International Designator](https://en.wikipedia.org/wiki/International_Designator), also known as COSPAR ID, and NSSDCA ID 
and is an international identifier assigned to artificial objects in space.

#### sat:orbit_state

Indicates the type and current state of orbit. Satellites are either geosynchronous in which case they have one state: 
`geostationary`, or they are sun synchronous (i.e., polar orbiting satellites) in which case they are either `ascending` or 
`descending`. For sun synchronous satellites it is daytime during one of these states, and nighttime during the other.

#### sat:absolute_orbit

A count of orbits from 1 to the number of orbits made in the total satellite lifecycle. In mission planning and tasking, the 
absolute orbit may be used as a reference in the non systematic acquisition missions. The resulting Item can be tagged with the 
absolute orbit and thus searchable as such. In the case of orbital changes during the mission modifying the ground track and 
thus the repeat cycle and thus the number or relative orbits, the combination of cycle and relative orbit is not sufficient to 
derive an absolute orbit.

#### sat:relative_orbit

A count of orbits from 1 to the number of orbits contained in a repeat cycle, where relative orbit 1 starts from a specific 
reference location of the sub-satellite point (the point on the earth directly below the satellite). It resets to 1 when the 
sub-satellite point revisits the reference location.

#### sat:anx_datetime

The UTC time when the satellite crosses the [Ascending Node](https://en.wikipedia.org/wiki/Orbital_node). For geocentric and 
heliocentric orbits, the ascending node (or north node) is where the orbiting object moves north through the plane of 
reference. Used to quickly compute orbital parameters without having to download the product. For instance to compute on the 
fly a baseline between 2 satellite acquisition, to find the best candidate in the archive from a post-disaster event (e.g 
earthquake) scene acquisition for a DInSAR processing.

#### sat:orbit_cycle

The repeat cycle of the satellite. The repeat cycle is the number of orbits required for the satellite to return to the same
position in its orbit. It is used to determine the relative orbit number. For instance, a satellite with a 16-day repeat cycle
will have 16 relative orbits.

#### sat:orbit_state_vectors

The state vectors of the satellite at the time of acquisition. The state vectors are a set of parameters that describe the
position and velocity of the satellite at a given time. The state vectors are used to compute the position of the satellite
at any time during the acquisition. 
The state vectors are usually provided in the [ECI](https://en.wikipedia.org/wiki/Earth-centered_inertial) frame.
The field is a map where the key is the time of the state vector and the value is an array of 6 elements (optional 9).
The first 3 elements are the position of the satellite in meters and the last 3 elements are the velocity of the satellite in meters per second.
The optional 3 additional elements are the acceleration of the satellite in meters per second squared.

Example:

```json
{
  "sat:orbit_state_vectors": {
    "2015-03-05T05:19:40.103Z": [
      5082939.4189831074,
      1595651.7369050409,
      4648033.5588545678,
      5173.8779329387489,
      -269.5432365485845,
      -5550.246440359806
    ],
    "2015-03-05T05:19:45.449Z": [
      5110519.0376543682,
      1594174.328112772,
      4618284.7202579351,
      5143.1200754073143,
      -283.12343661734587,
      -5578.2160610647188
    ],
    "2015-03-05T05:19:50.449Z": [
      5136162.3887853986,
      1592727.024757518,
      4590328.5904560406,
      5114.1961953273549,
      -295.79384304587683,
      -5604.2108611458289
    ]
  }
}
```

It is recommended that the state vectors are provided in a separate file and referenced as a link with relationship type `sat:osv`.
The file should correspond to the example above.

## Relation types

The following types should be used as applicable `rel` types in the
[Link Object](https://github.com/radiantearth/stac-spec/tree/master/item-spec/item-spec.md#link-object).

| Type    | Description                                        |
| ------- | -------------------------------------------------- |
| sat:osv | Link to a file containing the orbit state vectors. |

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
