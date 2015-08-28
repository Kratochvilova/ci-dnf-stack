# richdeps-docker
Docker image for testing rich dependencies in DNF/RPM using the Behave framework

## Overview
Each test runs in it's own container making it possible to run multiple tests
in parallel without interfering with each other. These tests are meant to
verify that both DNF and RPM (if relevant) interpret the rich dependency semantics
correctly. In overall the system is written in such a way that it is package-manager
agnostic, so plugging in tests for `PackageKit` is just about providing correct
CLI arguments.

## Binaries

### test-launcher.py
Launches the `test-suite` container (mounting, result collection ...).

### launch-test
Executes the test case specified in first parameter with Behave.

## Describing a test

Here's an example configuration from the first ported test:

```
Feature: Richdeps/Behave test-1
 TestA requires (TestB OR TestC), TestA recommends TestC

Scenario: Install TestA from repository "test-1"
 Given I use the repository "test-1"
 When I "install" a package "TestA" with "dnf"
 Then package "TestA, TestC" should be "installed"
 And package "TestB" should be "absent"

```

Possible actions: install, remove
Possible package managers: dnf, rpm (pkcon soonish)
Possible states: installed, removed, absent
