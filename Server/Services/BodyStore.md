# Body Store (BS)

## Overview

The Body Store is responsible for storing, retrieving, and serving all media files including audio messages, images, videos, and profile pictures. It handles media transcoding, caching, and streaming.

## Entry Point

- **Main File**: `body_server.js`
- **Service Type**: `bs`
- **Base Class**: Extends `VoxerService`
- **File Size**: 78KB

## Key Responsibilities

1. **Media Storage**
   - Audio messages (primary Voxer content type)
   - Images and photos
   - Video messages
   - Profile pictures
   - File attachments

2. **Media Processing**
   - Audio transcoding (Speex, Opus, MP3, Voxer Silk)
   - Image resizing and optimization
   - Video processing
   - Format conversion

3. **Content Delivery**
   - Streaming media to clients
   - Range request support for partial downloads
   - Progressive download for large files
   - CDN integration

4. **Caching**
   - Multi-tier caching (disk + distributed)
   - Cache invalidation strategies
   - Pre-warming for popular content

## Storage Architecture

### Disk Cache
- Local disk cache for fast access
- LRU eviction policy
- Configurable size limits
- Custom file format with metadata headers

### Distributed Storage
- **Primary**: Riak distributed key-value store
- Redundant storage across multiple nodes
- Eventual consistency model
- Large capacity for long-term storage

### File Format
The Body Store uses a custom disk file format:
```
16 byte fixed header:
  - byte 0: version number
  - bytes 1-4: total header + JSON length (int32BE)
  - byte 5: completion flag (0=incomplete, 1=complete)
  - bytes 6-15: reserved for future expansion
Original HTTP headers (JSON)
Raw media bytes
```

## Media Processing

### Audio Codecs Supported
- Voxer Silk (proprietary)
- Speex
- Opus
- MP3

### Transcoding Pipelines
- Speex → Voxer Silk
- Voxer Silk → MP3
- Live transcoding on cache miss

### Image Processing
- Resize and crop
- Format conversion
- Thumbnail generation
- EXIF data handling

## Configuration

Key parameters:
- `max_body_size` - Maximum upload size (default: 30MB)
- `max_buffered_cache_writes` - Write buffer size
- `notfound_max_wait` - Wait time for incomplete uploads
- Disk cache paths and size limits
- Riak cluster configuration

## Performance Characteristics

- Very high throughput for media streaming
- Write-heavy during peak messaging times
- Read-heavy from cache (disk or distributed)
- Optimized for sequential access patterns
- Large file handling with chunked transfer

## Scaling Characteristics

- Horizontally scalable (multiple BS instances)
- Stateless (any instance can serve any content)
- Storage layer scales independently
- Disk cache local to each instance

## Dependencies

- Riak distributed storage cluster
- Relay client for Riak communication
- Local filesystem for disk cache
- FFmpeg or similar for media transcoding

## API Surface

HTTP endpoints for:
- Media upload (POST)
- Media download (GET)
- Range requests for streaming
- Media metadata queries
- Cache management

## Code Location

**Repository**: `https://github.com/voxer/server`
**Main File**: `body_server.js`
**Directory**: `bs/` (supporting modules)
