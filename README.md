# BitStorm

An experimental BitTorrent client implemented in Python 3 using `asyncio`.

BitStorm is not a practical BitTorrent client—it lacks many features needed for real-world use. Instead, it was created as a fun project to explore the BitTorrent protocol and Python's `asyncio` library.

### Current Features
✅ Download pieces (leeching)  
✅ Contact tracker periodically  
❌ Seed (upload) pieces  
❌ Support multi-file torrents  
❌ Resume a download  

Even though BitStorm isn't practical yet, you're welcome to learn from it, improve it, steal code, laugh at it, or just ignore it.

### Known Issues
- Sometimes, the client hangs at startup. This appears to be related to the number of concurrent peer connections.

---
## Getting Started

Install the required dependencies and run the unit tests:
```sh
make init
make test
```

To download a torrent file, run:
```sh
python bitstorm.py -v tests/data/bootfloppy-utils.img.torrent
```

If successful, your torrent will be downloaded, and the program will terminate. To stop the client manually, use `Ctrl + C`.

---
## Design Considerations

The goal of BitStorm was to gain hands-on experience with `asyncio` and Python 3.5 features like type hinting, while scratching the long-standing itch of implementing the BitTorrent protocol.

The code prioritizes clarity and simplicity over efficiency or performance. For example:
- Pieces are requested sequentially, without implementing a _rarest-first_ algorithm.
- All downloaded pieces are kept in memory until the entire torrent is complete.

---
## Code Walkthrough

### Core Components
#### `bitstorm.client.TorrentClient`
- Connects to the tracker to receive peer lists.
- Manages a queue of available peers.
- Determines the order of piece requests.
- Shuts down once the download is complete.

#### `bitstorm.client.PieceManager`
- Implements the logic for selecting the next piece to request.
- Handles piece assembly (currently using a basic strategy).
- Note: File writing is currently synchronous and could be improved.

#### `bitstorm.protocol`
- Implements BitTorrent-specific logic.
- `bitstorm.protocol.PeerConnection` handles peer-to-peer communication.
- `bitstorm.protocol.PeerStreamIterator` is an `async` iterator that continuously reads and parses messages from a peer connection.
- Individual BitTorrent messages are implemented as classes with `encode` and `decode` methods.
  - Since BitStorm does not yet support seeding, some messages are only implemented for downloading.
---
## License
BitStorm is released under the Apache v2 license. See `LICENSE` for details.

