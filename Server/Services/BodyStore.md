# Body Store (BS)

**Repository**: `https://github.com/voxer/server`
**Location**: `body_server.js` and `bs/`

## Overview

The Body Store stores, retrieves, and serves media files including audio messages, images, videos, and profile pictures. The service handles media transcoding, caching, and streaming.

## Media Storage

The service stores audio messages, images, videos, profile pictures, and file attachments. Media is stored using a multi-tier architecture with local disk cache and distributed storage in Riak. The disk cache uses an LRU eviction policy with configurable size limits. Riak provides redundant storage across multiple nodes using an eventual consistency model.

## Media Processing

The service transcodes audio between formats including Voxer Silk, Speex, Opus, and MP3. Common transcoding pipelines include Speex to Voxer Silk and Voxer Silk to MP3, with transcoding performed on cache miss. Image processing includes resizing, cropping, format conversion, thumbnail generation, and EXIF data handling. Video processing is also supported.

## Content Delivery

The service streams media to clients and supports range requests for partial downloads and progressive download. It integrates with CDN infrastructure for content distribution.

## Caching

The service implements multi-tier caching with disk and distributed storage layers. Cache invalidation strategies are applied and popular content can be pre-warmed.

## File Format

The Body Store uses a custom disk file format with a 16-byte fixed header containing version number, header and JSON length, completion flag, and reserved bytes. The header is followed by original HTTP headers in JSON format and raw media bytes.

## API Surface

The service provides HTTP endpoints for media upload, media download, range requests for streaming, media metadata queries, and cache management.

## Configuration

Key configuration parameters include maximum upload size, write buffer size, wait time for incomplete uploads, disk cache paths and size limits, and Riak cluster configuration.

## Performance Characteristics

The service provides high throughput for media streaming. Traffic is write-heavy during peak messaging times and read-heavy from cache. The service is optimized for sequential access patterns and handles large files with chunked transfer.

## Scaling Characteristics

The service is horizontally scalable with multiple instances. Each instance is stateless and can serve any content. The storage layer scales independently with disk cache local to each instance.

## Dependencies

The service depends on the Riak distributed storage cluster, the Relay client for Riak communication, local filesystem for disk cache, and FFmpeg for media transcoding.
