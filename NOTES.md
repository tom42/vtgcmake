<!--
SPDX-FileCopyrightText: 2025 Thomas Mathys
SPDX-License-Identifier: MIT
-->

# Testing
## `include(CTest)` is *not* required
To quote [Craig Scott](https://discourse.cmake.org/t/project-structure-for-unit-testing-and-coverage/9038/4):
> Unless you’re running a CDash Dashboard script, there’s little benefit to
> doing `include(CTest)`. That adds a bunch of noise for things intended for
> nightly builds, but it just tends to get in the way for developers.
> I don’t typically include that module in any of my projects these days.
