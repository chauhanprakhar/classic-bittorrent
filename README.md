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

## Features

- Decoding and encoding of Bencode strings
- Connection management with torrent peers
- Piece downloading with verification against SHA-1 hashes
- Support for magnet links to initiate downloads

## Installation

To install the required Go packages, run:

```bash
go mod tidy
```

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
