<!--
SPDX-FileCopyrightText: 2025 Thomas Mathys
SPDX-License-Identifier: MIT
-->

# Testing
## `include(CTest)` is not required unless CDash is used
[Quote from Craig Scott](https://discourse.cmake.org/t/project-structure-for-unit-testing-and-coverage/9038/4):

> Unless you’re running a CDash Dashboard script, there’s little benefit to
> doing `include(CTest)`. That adds a bunch of noise for things intended for
> nightly builds, but it just tends to get in the way for developers.
> I don’t typically include that module in any of my projects these days.

We do not use CDash, so we do not need the `CTest` module.

## `BUILD_TESTING` is not required unless `include(CTest)` is used
[Quote from the CMake manual](https://cmake.org/cmake/help/latest/variable/BUILD_TESTING.html):

> Control whether the `CTest` module invokes `enable_testing()`.

We do not use CDash and therefore we do not use the `CTest` module, so there
is little point in ever setting `BUILD_TESTING`.
