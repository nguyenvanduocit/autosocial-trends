---
source: hackernews
title: Spherical Cow
url: https://lib.rs/crates/spherical-cow
date: 2025-12-29
---

[Search](/#search_q "Search")

# [Lib.rs](/)

›
[Algorithms](/algorithms "Core algorithms such as hashing, sorting and searching.
")

[#sphere](/keywords/sphere)
[#obj](/keywords/obj)
[#geometry](/keywords/geometry)
[#packing](/keywords/packing)
#spherepacking

## spherical-cow

Spherical Cow: High volume fraction sphere packing in arbitrary geometries

by
[Tim DuBois](/~Libbum "owner: Libbum")
and
[2 contributors](https://github.com/libbum/spherical-cow/graphs/contributors)

* [Install](/install/spherical-cow)
* [API reference](https://docs.rs/spherical-cow)
* [GitHub (libbum)](https://github.com/libbum/spherical-cow)

### [5 releases](/crates/spherical-cow/versions)

|  |  |
| --- | --- |
| 0.1.4 | Feb 12, 2021 |
| 0.1.3 | Feb 11, 2021 |
| 0.1.2 | Oct 29, 2018 |
| 0.1.1 | Jan 22, 2018 |
| 0.1.0 | Jan 21, 2018 |

#**2553** in [Algorithms](/algorithms "Rust implementations of core algorithms such as hashing, sorting, searching, and more.")

**29** downloads per month

Used in [sphere\_pack\_from\_json](/crates/sphere_pack_from_json)

**MIT/Apache**

49KB

764 lines

# Spherical Cow

A high volume fraction sphere packing library

[![Crates.io](https://img.shields.io/crates/v/spherical-cow.svg)](https://crates.io/crates/spherical-cow)
│
[![Docs.rs](https://img.shields.io/badge/api-documentation-blue.svg)](https://docs.rs/spherical-cow/)
│
[![Travis-ci](https://img.gs/czjpqfbdkz/full/https://travis-ci.org/Libbum/spherical-cow.svg?branch=master)](https://travis-ci.org/Libbum/spherical-cow)
│
[![Codecov](https://img.gs/czjpqfbdkz/full/https://codecov.io/gh/Libbum/spherical-cow/branch/master/graph/badge.svg)](https://codecov.io/gh/Libbum/spherical-cow)
│
[![FOSSA Status](https://img.gs/czjpqfbdkz/full/https://app.fossa.io/api/projects/git%2Bgithub.com%2FLibbum%2Fspherical-cow.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com/Libbum/spherical-cow?ref=badge_shield)

Based on the advancing fronts algorithm outlined in Valera *et al.*, [Computational Particle Mechanics 2, 161 (2015)](https://doi.org/10.1007/s40571-015-0045-8).

> Milk production at a dairy farm was low, so the farmer wrote to the local university, asking for help from academia.
> A multidisciplinary team of professors was assembled, headed by a theoretical physicist, and two weeks of intensive on-site investigation took place.
> The scholars then returned to the university, notebooks crammed with data, where the task of writing the report was left to the team leader.
> Shortly thereafter the physicist returned to the farm, saying to the farmer, "I have the solution, but it works only in the case of spherical cows in a vacuum".

# Usage

Complete documentation can be found at [docs.rs](https://docs.rs/spherical-cow/).

A simple example to get you packing spheres of radii (0.1..0.2) into a container sphere of radius 2.

```
use spherical_cow::shapes::Sphere;
use rand::distributions::Uniform;
use nalgebra::Point3;

fn main() {
    let boundary = Sphere::new(Point3::origin(), 2.0).unwrap();
    let mut sizes = Uniform::new(0.1, 0.2);

    let spheres = spherical_cow::pack_spheres(boundary, &mut sizes).unwrap();

    println!("Number of spheres: {}", spheres.len());
}
```

More elaborate examples can be found in the [examples](https://github.com/libbum/spherical-cow/blob/HEAD/examples/) directory.

# Output

True to its name, it is indeed possible to build a spherical cow:

![spherical cow in vacuum](https://img.gs/czjpqfbdkz/full/https://raw.githubusercontent.com/Libbum/spherical-cow/master/examples/objects/cow_output.jpg?raw=true)

You can run this example yourself from [show\_in\_cow](https://github.com/libbum/spherical-cow/blob/HEAD/examples/show_in_cow.rs).

# Example use cases

The paper which this algorithm comes from gives two examples of real world use cases:

1. Sphere packing a skull model to study fractures due to shocks and penetrating objects.
2. Sphere packing a cutting tool to identify the failure / breaking points when the tool is placed under load.

The reason this library was initially written was to optimise the layout of inflatable [space habitats](https://github.com/Libbum/space-habitats) which may one day be constructed on the Moon and Mars.

# License

Licensed under the Apache License, [Version 2.0](http://www.apache.org/licenses/LICENSE-2.0) or the [MIT license](http://opensource.org/licenses/MIT), at your option.
These files may not be copied, modified, or distributed except according to those terms.

[![FOSSA Status](https://img.gs/czjpqfbdkz/full/https://app.fossa.io/api/projects/git%2Bgithub.com%2FLibbum%2Fspherical-cow.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com/Libbum/spherical-cow?ref=badge_large)

## Deeply linked dependencies

The [wayland-protocols](https://github.com/Smithay/wayland-rs/tree/master/wayland-protocols) library (released under an MIT license) is used by [kiss3d](https://github.com/sebcrozet/kiss3d) and [obj](https://github.com/Smithay/wayland-rs/tree/master/wayland-protocols), both of which are only extant in the examples directory of this project (and thus are NOT a part of the library).
Content therein: the file `misc/server-decoration.xml`, is Copyright (C) 2015 Martin Gräßlin and licensed under the GNU Lesser General Public Library, version 2.1. You can find a copy of this license at <https://www.gnu.org/licenses/lgpl-2.1.en.html>

#### Dependencies

~6MB

~114K SLoC

* [float-cmp](/crates/float-cmp "obsolete") 0.8
* [itertools](/crates/itertools "outdated") 0.10
* [nalgebra](/crates/nalgebra "obsolete") 0.24
* [rand](/crates/rand "outdated") 0.8
* [rand\_distr](/crates/rand_distr "outdated") 0.4
* [serde-1?](/crates/spherical-cow/features#feature-serde-1 "optional feature")
  [serde](/crates/serde "1.0")

* dev
  [criterion](/crates/criterion "obsolete") 0.3
* dev
  [kiss3d](/crates/kiss3d "obsolete") 0.29
* dev
  [obj](/crates/obj "0.10")
* dev
  [serde\_json](/crates/serde_json "1.0")

See also:
[unit-sphere](/crates/unit-sphere),
[mesh-loader](/crates/mesh-loader),
[gltfgen](/crates/gltfgen),
[hexasphere](/crates/hexasphere),
[packed\_struct](/crates/packed_struct),
[rs-math3d](/crates/rs-math3d),
[preserves](/crates/preserves),
[rune\_c\_compiler](/crates/rune_c_compiler),
[geo-aid](/crates/geo-aid),
[centerline](/crates/centerline),
[geodatafusion](/crates/geodatafusion)

[**Lib.rs**](/) is an unofficial list of Rust/Cargo crates, created by [kornelski](https://github.com/kornelski). It contains data from [multiple sources, including heuristics, and manually curated data](/data-processing). Content of this page is not necessarily endorsed by the authors of the crate. This site is not affiliated with nor endorsed by the Rust Project.
If something is missing or incorrect,
please [file a bug](https://gitlab.com/lib.rs/main/issues/new).
