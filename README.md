# LevelDB Integration for EpicChain

**LevelDB** is a high-performance key-value storage library originally developed by Google. It provides an efficient, ordered mapping from string keys to string values, making it a crucial component for EpicChain’s data management.

**Note:** This repository is currently receiving very limited maintenance. We will only review changes that address critical bugs, such as data loss or memory corruption, or necessary updates for internally supported clients.

![CI Badge](https://github.com/google/leveldb/actions/workflows/build.yml/badge.svg)


## Overview

In the EpicChain blockchain network, LevelDB is utilized to handle the key-value data essential for the blockchain’s operation. The following sections detail how LevelDB is integrated into EpicChain, its features, and its usage.

### Key Features for EpicChain

- **Arbitrary Byte Arrays:** LevelDB supports keys and values as arbitrary byte arrays, which is vital for storing diverse types of blockchain data in EpicChain.
- **Sorted Data Storage:** Data is stored sorted by key, allowing for efficient retrieval and organization of blockchain records.
- **Custom Comparison Function:** Users can provide a custom comparison function to override the default sorting order, enabling EpicChain to handle specialized key ordering as needed.
- **Atomic Batches:** LevelDB supports making multiple changes in one atomic batch. EpicChain uses this feature to ensure that multiple blockchain transactions or updates are processed atomically, preserving data integrity.
- **Snapshot Creation:** Users can create transient snapshots to get a consistent view of the data. EpicChain utilizes snapshots for reliable state management and rollback capabilities.
- **Forward and Backward Iteration:** LevelDB supports iteration over data in both directions. EpicChain leverages this for comprehensive data exploration and analysis.
- **Data Compression:** Data is automatically compressed using the Snappy compression library, with support for Zstd compression as well. This helps reduce storage costs and improve access times for EpicChain.
- **Customizable External Activity:** File system operations are managed through a virtual interface, allowing EpicChain to customize OS interactions as required.

### Installation and Integration for EpicChain

#### Prerequisites

- **Operating Systems:** LevelDB is supported on Windows and Linux. For Mac users, a virtual machine is recommended. Note that Ubuntu versions 14 and 16 are supported; Ubuntu 17 is not.
- **.NET Core:** Install [.NET Core](https://www.microsoft.com/net/download/core) as LevelDB for EpicChain relies on it.

#### On Linux

Install necessary development packages:

```sh
sudo apt-get install libleveldb-dev sqlite3 libsqlite3-dev libunwind8-dev
```

#### On Windows

Use the [EpicChain version of LevelDB](https://github.com/xmoohad/leveldb) for Windows-specific integration. 

```sh
dotnet epicchain-cli.dll
```

### Building LevelDB for EpicChain

#### Clone the Repository

Clone the LevelDB repository:

```sh
git clone --recurse-submodules https://github.com/google/leveldb.git
```

#### Building on POSIX Systems

Quick start for POSIX systems:

```sh
mkdir -p build && cd build
cmake -DCMAKE_BUILD_TYPE=Release .. && cmake --build .
```

#### Building on Windows

Generate Visual Studio project/solution files:

```cmd
mkdir build
cd build
cmake -G "Visual Studio 15" ..
```

For 64-bit builds:

```cmd
cmake -G "Visual Studio 15 Win64" ..
```

Compile the solution:

```cmd
devenv /build Debug leveldb.sln
```

Alternatively, open `leveldb.sln` in Visual Studio and build from within.

### Usage in EpicChain

#### Basic Operations

- **Put:** `Put(key, value)` - Stores a key-value pair in the database.
- **Get:** `Get(key)` - Retrieves the value associated with a key.
- **Delete:** `Delete(key)` - Removes a key-value pair from the database.

#### Commands

- **Show State:** Use `show state` to display the current state of the blockchain.
- **Create Wallet:** Use `create wallet wallet.db3` to create a new wallet database.

#### Advanced Operations

- **Batch Operations:** Perform multiple database changes in one atomic batch for efficiency.
- **Snapshot Management:** Create and manage snapshots to obtain a consistent view of the blockchain data.

### Limitations

- **No SQL Support:** LevelDB is not a SQL database; it does not support relational data models, SQL queries, or indexing.
- **Single Process Access:** Only a single process (multi-threaded) can access a particular database at a time.
- **No Client-Server Support:** LevelDB does not include built-in client-server support; applications requiring this must implement their own server.

### Contributing to LevelDB

**Contribution Requirements:**

1. **Tested Platforms Only:** Contributions are generally accepted only for platforms that are tested, such as POSIX systems (Linux and macOS) or Windows.
2. **Stable API:** Changes must maintain a stable API; breaking changes may be rejected.
3. **Tests:** All changes should be accompanied by tests or an explanation why new tests are not required.
4. **Consistent Style:** Conform to the [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html). Use `clang-format` for formatting.

**Submitting a Pull Request:**

- Sign a Contributor License Agreement (CLA) at https://cla.developers.google.com/.
- Squash changes into a single commit and rebase on `google/leveldb/main` for a linear commit history.

### Performance Overview

- **Write Performance:** Benchmarks show high throughput for write operations, with sequential fills achieving 62.7 MB/s and random writes at 45.0 MB/s.
- **Read Performance:** Reads are efficient, with random reads at approximately 60,000 reads per second and sequential reads at 232.3 MB/s.

### Repository Contents

- **Documentation:** Refer to [doc/index.md](doc/index.md) for more detailed documentation and [doc/impl.md](doc/impl.md) for implementation overviews.
- **Public Interface:** Key headers include:
  - `include/leveldb/db.h`: Main interface to the database.
  - `include/leveldb/options.h`: Database configuration.
  - `include/leveldb/comparator.h`: Custom comparison functions.
  - `include/leveldb/iterator.h`: Data iteration.
  - `include/leveldb/write_batch.h`: Atomic batch operations.
  - `include/leveldb/slice.h`: Byte array management.
  - `include/leveldb/status.h`: Status reporting.

**Note:** Avoid relying on internal headers not listed above, as they may change without notice.

**For EpicChain Integration:**

LevelDB's role in EpicChain is pivotal for managing and optimizing blockchain data. Its features ensure robust performance and reliability, making it an ideal choice for EpicChain’s key-value data storage needs.
