<!--
SPDX-FileCopyrightText: 2025 Thomas Mathys
SPDX-License-Identifier: MIT
-->

# Modules
## Adding directories to `CMAKE_MODULE_PATH`
Example, based on [this](https://discourse.cmake.org/t/how-to-properly-import-cmake-files-modules/2816/2):

```cmake
list(INSERT CMAKE_MODULE_PATH 0 "${CMAKE_CURRENT_LIST_DIR}/cmake")
```

Note two things:

1. We use a variable such as `CMAKE_CURRENT_LIST_DIR` to ensure the correct
   directory is added even when the project is used as a subproject.
   Do not use variables such as `CMAKE_SOURCE_DIR`.
2. We *prepend* our directory to `CMAKE_MODULE_PATH`. If our directory
   contains module 'foo', and other projects have added directories that also
   contain modules called 'foo', then our module 'foo' is found first, which
   is most likely what we want.

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

We do not use the `CTest` module, but we can call `enable_testing()` ourselves.

## Dealing with tests when being used as a subproject

Assume we're working on project A, which uses project B as a subproject.
In this case it may be undesired if enabling tests in A also enables B's tests.

[Quote from Craig Scott](https://discourse.cmake.org/t/how-to-use-fetchcontent-correctly-for-building-external-dependencies/3686/2):

> A well-structured project should ideally provide a project-specific CMake
> variable that can be passed down to turn tests on or off, and it should
> default to true if the dependency is being built on its own and false if it
> is not the top level project.

[Related quote from Ben Boeckel](https://discourse.cmake.org/t/fetchcontent-vs-build-testing/4477/4):

> Basically I would try and namespace as much as possible. For example,
> offer `MyProject_BUILD_TESTING` then set `BUILD_TESTING` internally based on it.
> It helps to keep your relevant options grouped in the cache file and editors
> as well as offering useful knobs when embedded.

Providing a project specific build testing variable and grouping things together
by prefixing it with the project name is actually very good advice.

## Example

Minimal example that puts together the ideas from above:

```cmake
project(myproject)

# Whether to enable and build tests. Default to true if main project, false otherwise.
option(myproject_BUILD_TESTING "Build myproject tests." ${PROJECT_IS_TOP_LEVEL})

# Build production code
add_subdirectory(src)

# Set up testing and build tests
if(myproject_BUILD_TESTING)
  # enable_testing() should be called from the top-level source directory,
  # that is, the file that contains the project command.
  enable_testing()
  # ...
  # More test setup. Maybe detect the unit test framework using find_package.
  # ...
  add_subdirectory(test)
endif()
```
