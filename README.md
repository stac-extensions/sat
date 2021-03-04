# Template Extension Specification


- **Title:** Satellite
- **Identifier:** <https://stac-extensions.github.io/sat/v1.0.0/schema.json>
- **Field Name Prefix:** sat
- **Scope:** Item
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @emmanuelmathot

This document explains the Satellite Extension to the [SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.
It adds metadata related to a satellite that carries an instrument for collecting data. It will often be combined with other 
extensions that describe the actual data, such as the [EO](https://github.com/radiantearth/stac-spec/tree/master/extensions/eo) or 
[SAR](https://github.com/stac-extensions/sar) extensions.

- Examples:
  - [Item example](examples/item.json): Shows the basic usage of the extension in a STAC Item
  - [Collection example](examples/collection.json): Shows the basic usage of the extension in a STAC Collection
- [JSON Schema](json-schema/schema.json)
- [Changelog](./CHANGELOG.md)

## Item Properties

| Field Name       | Type                     | Description |
| ---------------- | ------------------------ | ----------- |
| sat:platform_international_designator | string | The International Designator, also known as COSPAR ID, and NSSDCA ID |
| sat:orbit_state        | string        | The state of the orbit. Either `ascending` or `descending` for polar orbiting satellites, or `geostationary` for geosynchronous satellites |
| sat:absolute_orbit     | integer       | The obsolute orbit number at the time of acquisition. |
| sat:relative_orbit     | integer       | The relative orbit number at the time of acquisition. |
| sat:anx_datetime      | string       | The [Ascending Node](https://en.wikipedia.org/wiki/Orbital_node) Crossing (ANX) time, in UTC. It is formatted according to [RFC 3339, section 5.6](https://tools.ietf.org/html/rfc3339#section-5.6). |

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
