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

## So what is required to run tests?
1. `enable_testing()` needs to be called in the top-level source directory.
2. Then, tests can be added with the `add_test()` command.

The [CMake manual on `enable_testing()`](https://cmake.org/cmake/help/latest/command/enable_testing.html):

> Enables testing for this directory and below.
>
> This command should be in the top-level source directory because `ctest(1)`
> expects to find a test file in the top-level build directory.

And the [CMake manual on `add_test()`](https://cmake.org/cmake/help/latest/command/add_test.html):

> Adds a test [...].
>
> CMake only generates tests if the `enable_testing()` command has been invoked.
> The `CTest` module invokes `enable_testing()` automatically unless `BUILD_TESTING`
> is set to `OFF`.

We do not use the `CTest` module, but we can and are allowed to call
`enable_testing()` ourselves.
