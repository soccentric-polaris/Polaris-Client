# Polaris-Client

## Project Summary

Polaris-Client is a C++17 embedded-Linux daemon that runs hardware-validation work on a target board and exchanges results with Polaris-Server. The executable is assembled from configuration, stream, run, registry, spool, logging, diagnostics, OTA, and HAL-service components under `src/`, with `src/main.cpp` and `src/app.cpp` providing the application entry path. The registry and adapter tree expose module-specific test execution for compute, thermal, storage, networking, cameras, sensors, safety, and other board capabilities. Unsupported or incomplete module paths are represented by the existing stub behavior, so the adapter catalog should not be read as proof that every hardware test is functional.

The client has two gRPC roles. Its outbound stream connects to the server for commands, heartbeats, results, and logs; its inbound HAL service lets the Python SDK or another integration client request module operations. Result serialization bridges HAL data into protobuf messages, while the SQLite spool preserves results and logs when the network is unavailable. Configuration examples live in `conf/`, systemd packaging is under `systemd/`, and Docker helpers support cross-build workflows. The repository also contains design notes in `client_update/` that document implementation history and remaining work.

Automated evidence is narrow but concrete: `tests/test_result_serializer.cc` covers serialization and `tests/test_run_orchestrator.cc` covers orchestration. The Makefile exposes `configure`, `build`, `test`, `format`, `cppcheck`, `doc`, and Docker targets; the CMake test binaries and `ctest --output-on-failure` are useful validation entry points. Network, real-device, service-manager, and hardware-peripheral behavior still requires manual verification.

## Feature Inventory

| # | Name / Description | Implemented? | Test Coverage? | Manual Test |
|---:|---|---|---|---|
| 1 | C++17 daemon entry point and application lifecycle (`src/main.cpp`, `src/app.cpp`) | Implemented | Build target exists; no dedicated lifecycle test found | [ ] Manual verification required |
| 2 | TOML configuration and sample files in `conf/` | Implemented | No focused config test found | [ ] Manual verification required |
| 3 | Outbound gRPC command/result stream and reconnect path (`src/stream`) | Implemented | No focused stream test found | [ ] Manual verification required |
| 4 | Heartbeat and target communication | Implemented | No focused heartbeat test found | [ ] Manual verification required |
| 5 | Run orchestration and batching (`src/run`) | Implemented | `tests/test_run_orchestrator.cc` | [ ] Manual verification required |
| 6 | HAL-to-protobuf result serialization (`src/hal_bridge`) | Implemented | `tests/test_result_serializer.cc` | [ ] Manual verification required |
| 7 | SQLite result and log spool (`src/spool`) | Implemented | No focused spool test found | [ ] Manual verification required |
| 8 | Module registry and adapter dispatch (`src/registry`) | Implemented | No broad adapter matrix test found | [ ] Manual verification required |
| 9 | Hardware adapter catalog | Partial | Orchestration tests only; per-adapter coverage not found | [ ] Manual verification required |
| 10 | Inbound HalService gRPC endpoint (`src/hal_service`) | Implemented | No focused service test found | [ ] Manual verification required |
| 11 | Log tailing and forwarding (`src/log`) | Implemented | No focused log test found | [ ] Manual verification required |
| 12 | Janitor cleanup and diagnostic socket (`src/janitor`, `src/diag`) | Implemented | No focused tests found | [ ] Manual verification required |
| 13 | OTA client path (`src/ota`) | Partial | No focused OTA test found | [ ] Manual verification required |
| 14 | Native and Docker cross-build workflows | Implemented | `make build`, Docker targets declared | [ ] Manual verification required |

## Validation

```bash
make configure
make build
make test
cd build/native && ctest --output-on-failure
```

The repository also contains Docker build/test targets for cross-compilation. Hardware access, server interoperability, systemd installation, and recovery from a full spool are not verified by the repository tests.
