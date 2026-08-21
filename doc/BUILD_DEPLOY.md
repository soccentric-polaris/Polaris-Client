# Polaris-Client Build and Deployment

## Purpose and scope

Polaris-Client is a CMake/Ninja C++ test application suite. The Makefile supports native host builds, ARM64 Docker/QEMU cross-builds, test execution, static analysis, Doxygen, and copying cross-built `lib/` and `bin/` trees to two NVIDIA devices.

## 1. Prerequisites

- Linux with GNU Make, CMake, Ninja, a C++ toolchain, `nproc`, and `ctest`.
- The sibling `Polaris-HAL` checkout must be present because CMake resolves shared protobuf/HAL paths through the parent `Polaris/` directory.
- Native quality targets require `clang-format`, `cmake-format`, `cppcheck`, `doxygen`, and Graphviz as applicable.
- Docker targets require Docker and ARM64 emulation support. `make docker-install` may use `sudo apt` when Docker is absent.
- Deployment additionally requires `sshpass`, network access to the target, and valid credentials. The Makefile defaults are `nvidia` / `nvidia`; override them rather than committing credentials.

## 2. Setup

From this leaf directory, inspect available targets with `make help`. For a native build, no separate configure step is needed: `make configure` creates `build/native` and enables `BUILD_TESTS=ON`.

## 3. Build and compile

```bash
make configure
make build                 # native CMake/Ninja build
make docker-image          # build the polaris cross-build image if absent
make docker-configure
make docker-build          # ARM64 build in build/cross
make JOBS=4 docker-build   # optional parallelism override
```

`make` defaults to `build`; the cross-build path is selected explicitly with `docker-build`. Docker mounts the parent Polaris directory and uses the `polaris` image.

## 4. Test and lint

```bash
make test                  # native CTest: serializer_tests and runner_tests
make docker-test           # all CTest tests under ARM64/QEMU
make format                # ninja format and cmake-format in place
make cppcheck              # static analysis target from the generated build
make doc                   # Doxygen target
```

The native `test` target is intentionally limited to `serializer_tests` and `runner_tests`; Docker runs the complete CTest set. Formatting and analysis require their external tools.

## 5. Packaging and artifacts

This Makefile does not declare a packaging target. Native outputs are under `build/native/{bin,lib}`; Docker outputs are under `build/cross/{bin,lib}`. Use `make list` to inspect discovered binaries.

## 6. Deployment, install, and run

```bash
HAL_SSH_USER=nvidia HAL_SSH_PASS='REDACTED' make deploy-a
HAL_SSH_USER=nvidia HAL_SSH_PASS='REDACTED' make deploy-b
make IP_A=192.168.1.3 IP_B=192.168.1.13 deploy
make run-a [app] [args...]
make run-b [app] [args...]
make run [app] [args...]   # run native binaries locally
```

`deploy-a` and `deploy-b` first run `docker-build`, then replace `/tmp/acu-test` on the selected device and copy `build/cross/lib` and `build/cross/bin`. These are conditional, destructive-to-that-temp-directory, hardware/network operations; verify IPs, SSH access, and target architecture first.

## 7. Cleanup

```bash
make clean                 # remove native and cross build directories
make docker-clean          # remove only the cross build inside the image
```

`docker-clean` requires the Docker image and leaves the image and ccache volume in place.

## 8. Verification caveats

No cloud deployment is defined. Native and Docker builds are compile checks only. Manual verification must cover target boot/runtime compatibility, remote execution, application behavior, and device permissions. `run-a`/`run-b` require the deployed hardware and credentials.