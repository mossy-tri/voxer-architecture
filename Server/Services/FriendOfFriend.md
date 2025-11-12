# Friend-of-Friend Service (FOF)

**Repository**: `https://github.com/voxer/server`
**Location**: `fof/`

## Overview

The Friend-of-Friend (FOF) Service analyzes the social graph to provide friend recommendations, discover mutual connections, and support social features based on relationship networks. The service is built on the VoxerService base class.

## Friend Recommendations

The service suggests users based on mutual friends and calculates relationship strength between users. Recommendations are ranked by relevance and filtered to exclude already-connected users. Scoring considers the number of mutual friends, recency of mutual connections, interaction frequency with mutual connections, geographic proximity, and shared group memberships.

## Social Graph Analysis

The service builds and maintains a social graph representation of user connections. It calculates network metrics, finds shortest paths between users, and identifies communities and clusters. The graph consists of user nodes with attributes for activity, engagement, and network size. Edges represent friendships with attributes for strength, recency, and interaction count. Graph operations include adding and removing connections, updating edge weights, shortest path queries, and subgraph extraction.

## Mutual Connection Discovery

The service displays mutual friends between users and connection paths. It suggests conversation participants and provides group composition insights based on shared connections.

## Network Insights

The service calculates user network size, identifies influential users, measures network density, and detects communities within the social graph.

## Privacy Filters

The service respects user privacy settings, enforces block lists, handles hidden profiles, and supports opt-out preferences. Data minimization limits collection to necessary connection data, uses aggregation where possible, and implements time-limited retention. Users can opt out of recommendations and control their visibility in suggestions.

## Pool Client

The pool client in `fof/pool.js` connects to the FOF service with load balancing across instances. It is used by the Growth Service and Node Router.

## Data Sources

The service integrates with the Contact Service for imported contacts, the Header Store for conversation participants, the Growth Service for invite graphs, and activity logs for interaction patterns.

## Performance

The service performs graph traversal computations which can be compute-intensive. Results are cached with TTL-based invalidation and lazy recomputation. Recommendations are updated through periodic batch processing, off-peak graph analysis, and incremental updates. The graph can be sharded by user regions with locality-aware partitioning for distributed processing.

## API Surface

The service provides HTTP endpoints for retrieving friend recommendations, finding mutual connections, calculating relationship paths, querying network statistics, and executing social graph queries.

## Scaling Characteristics

The service is stateful and maintains graph state. The graph can be partitioned for distributed computation. The service uses a database or graph database backend for storage.

## Dependencies

The service integrates with the Contact Service for connection data, the Header Store for user and conversation data, the Growth Service for invitation data, and optionally a graph database for social graph storage.
