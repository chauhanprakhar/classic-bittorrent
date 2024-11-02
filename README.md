# Bencode Torrent Client

A lightweight BitTorrent client implementation in Go with support for Bencode encoding/decoding and magnet links.

## Overview

This project implements a Bencode-based torrent client in Go, capable of decoding Bencode strings, connecting to peers, downloading pieces of data, and handling magnet links. The client supports basic torrent operations, including handshake with peers, piece downloading, and data verification through hash checks.

## Table of Contents

- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
- [Key Functions and Algorithms](#key-functions-and-algorithms)
- [License](#license)
- [Contributing](#contributing)
- [Acknowledgement](#acknowledgement)

## Features

- Decoding and encoding of Bencode strings
- Connection management with torrent peers
- Piece downloading with verification against SHA-1 hashes
- Support for magnet links to initiate downloads

## Installation

### Prerequisites

* Go 1.18 or higher installed on your machine
* A terminal or command prompt to execute commands

### Setup

1. **Clone the Repository**
```bash
git clone https://github.com/yourusername/your-repo-name.git
cd your-repo-name
```

2. **Install Dependencies**
```bash
go mod init your-repo-name
```

### File Structure

* `bencode.go` - Implements the Bencode encoding and decoding
* `command.go` - Handles commands for downloading and connecting to peers
* `decoder.go` - Provides utilities for decoding Bencode strings
* `main.go` - Entry point of the application
* `utils.go` - Contains utility functions for various operations

### Usage

### Running the Tool

The basic syntax for running the program is:
```bash
go run main.go <command> [options]
```

### Available Commands

1. **decode** - Decode a Bencode string
```bash
go run main.go decode <bencodedString>
```

2. **info** - Show information about a torrent file or magnet link
```bash
go run main.go info <path_to_torrent_file>
```

3. **peers** - Get the list of peers from a tracker
```bash
go run main.go peers <path_to_torrent_file>
```

4. **handshake** - Perform a handshake with a peer
```bash
go run main.go handshake <path_to_torrent_file> <peer_address>
```

5. **download** - Download a file from peers
```bash
go run main.go download <path_to_torrent_file> <output_file>
```

6. **magnet_parse** - Parse a magnet link and extract tracker URL and info hash
```bash
go run main.go magnet_parse <magnet_link>
```

7. **magnet_download** - Download a file from a magnet link
```bash
go run main.go magnet_download <magnet_link> <output_file>
```

### Example Commands

1. Decode a Bencode string:
```bash
go run main.go decode "d3:foo3:bare"
```

2. Get info from a torrent file:
```bash
go run main.go info "example.torrent"
```

3. Download a file using a magnet link:
```bash
go run main.go magnet_download "magnet:?xt=urn:btih:EXAMPLEHASH" "output.file"
```

### Implementation Details

### Bencode Encoding/Decoding
The package implements full support for Bencode format including:
- Strings
- Integers
- Lists
- Dictionaries

### BitTorrent Protocol Support
- Peer wire protocol implementation
- Tracker communication
- Peer discovery
- Piece verification
- Magnet link parsing

### Notes

- Replace placeholders (`yourusername`, `your-repo-name`, `path_to_torrent_file`, `magnet_link`) with actual values
- Output files are created in the current directory unless specified otherwise
- The tool follows BitTorrent specification v1.0

## Usage

Compile the application and run it with the desired command. The main commands include:

- `decode <bencoded_string>`: Decode a given Bencode string.
- `info <torrent_file>`: Retrieve and display information about the torrent.
- `download <torrent_file> <output_file>`: Download the entire torrent and save it to a specified output file.
- `magnet_download <magnet_link> <output_file>`: Download a torrent from a magnet link.

## Key Functions and Algorithms

### Bencode Encoding/Decoding
- `decodeBencode(bencodedString string) (interface{}, error)`: Main function to decode Bencode strings. It identifies the type (string, integer, list, dictionary) and delegates to specific decoding functions.
- `decodeBencodeString(bencodedString string, firstIndex int) (interface{}, int, error)`: Decodes Bencoded strings, reading the length and the string value.
- `decodeBencodeInteger(bencodedString string, firstIndex int) (interface{}, int, error)`: Extracts integers from Bencoded format.
- `decodeBencodeList(bencodedString string, i int) ([]interface{}, int, error)`: Decodes lists, recursively calling itself for nested structures.
- `decodeBencodeDict(bencodedString string, i int) (interface{}, int, error)`: Converts Bencoded dictionaries into Go maps, ensuring keys are strings.

### Networking and Peer Management
- `handshake(peer string) (net.Conn, bool, error)`: Establishes a connection with a peer, performing the necessary handshake to negotiate protocol support and retrieve the peer's ID.
- `getUnchokedPeer(peer string) (net.Conn, error)`: Connects to a peer and checks if it is unchoked (allowed to download).
- `download(peersList []string)`: Manages multiple goroutines to download pieces concurrently from a list of peers, using a queue to track available pieces.

### Piece Handling
- `downloadPiece(peerList []string, pieceId, pieceCount int, actualPieceHash string) ([]byte, error)`: Downloads a specific piece of data from the peers, ensuring the downloaded data matches the expected hash.
- `getPieceData(conn net.Conn, pieceSize, pieceId int, actualPieceHash string) ([]byte, error)`: Requests and receives data blocks for a piece, reassembling them and verifying the integrity of the piece with a hash check.

### Magnet Link Support
- `magnetInfo(link string) error`: Extracts and verifies metadata from a magnet link, including the tracker URL and info hash.
- `magnetHandshake(link string) (net.Conn, int, error)`: Performs a handshake with the first peer retrieved from the magnet link, ensuring metadata extensions are supported.

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## Acknowledgments

- BitTorrent protocol specification
- Bencode encoding specification
- The Go community

---

**Note**: This tool is for educational purposes. Please ensure compliance with local laws and regulations when using BitTorrent technology.
