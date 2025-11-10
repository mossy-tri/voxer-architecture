# Friend-of-Friend Service (FOF)

## Overview

The Friend-of-Friend (FOF) Service analyzes the social graph to provide friend recommendations, discover mutual connections, and enable social features based on relationship networks.

## Entry Point

- **Main File**: `fof/fof.js`
- **Service Type**: `fof`
- **Base Class**: Extends `VoxerService`

## Key Responsibilities

1. **Friend Recommendations**
   - Suggest users based on mutual friends
   - Calculate relationship strength
   - Rank recommendations by relevance
   - Filter already-connected users

2. **Social Graph Analysis**
   - Build and maintain social graph
   - Calculate network metrics
   - Find shortest paths between users
   - Identify communities and clusters

3. **Mutual Connection Discovery**
   - Show mutual friends between users
   - Display connection paths
   - Suggest conversation participants
   - Group composition insights

4. **Network Insights**
   - User's network size
   - Influential users
   - Network density metrics
   - Community detection

## Algorithm Features

### Friend-of-Friend Matching
- Find friends of friends not yet connected
- Weight by number of mutual connections
- Consider interaction frequency
- Prioritize recent connections

### Recommendation Scoring
Factors considered:
- Number of mutual friends
- Recency of mutual connections
- Interaction frequency with mutuals
- Geographic proximity
- Shared group memberships

### Privacy Filters
- Respect user privacy settings
- Block list enforcement
- Hidden profile handling
- Opt-out support

## Social Graph Structure

### Nodes
- Users
- Attributes: activity, engagement, network size

### Edges
- Friendships/connections
- Attributes: strength, recency, interaction count

### Graph Operations
- Add/remove connections
- Update edge weights
- Shortest path queries
- Subgraph extraction

## Use Cases

### User Discovery
- "People you may know"
- Contact suggestions
- New user onboarding

### Group Formation
- Suggest group members
- Find common connections
- Identify natural communities

### Viral Growth
- Network effects measurement
- Invitation targeting
- Influence analysis

## API Surface

HTTP endpoints for:
- Get friend recommendations
- Find mutual connections
- Calculate relationship path
- Network statistics
- Social graph queries

## Pool Client

- **`fof/pool.js`** - Pool client for connecting to FOF service
- Used by Growth Service and Node Router
- Load balanced across instances

## Data Sources

- Contact Service (imported contacts)
- Header Store (conversation participants)
- Growth Service (invite graphs)
- Activity logs (interaction patterns)

## Performance Characteristics

- Graph traversal computations
- Can be compute-intensive
- Results can be cached
- Batch processing for recommendations
- Pre-computed recommendations

## Optimization Strategies

1. **Caching**
   - Cache recommendations per user
   - TTL-based invalidation
   - Lazy recomputation

2. **Batch Processing**
   - Periodic recommendation updates
   - Off-peak graph analysis
   - Incremental updates

3. **Graph Partitioning**
   - Shard graph by user regions
   - Locality-aware partitioning
   - Distributed graph processing

## Privacy Considerations

1. **Data Minimization**
   - Only use necessary connection data
   - Aggregate where possible
   - Time-limited retention

2. **User Control**
   - Opt-out of recommendations
   - Control visibility in suggestions
   - Privacy-preserving algorithms

## Scaling Characteristics

- Stateful (maintains graph)
- Can partition graph
- Computation can be distributed
- Database or graph database backend

## Dependencies

- **Contact Service** - Connection data
- **Header Store** - User and conversation data
- **Growth Service** - Invitation data
- **Graph Database** - Social graph storage (optional)

## Code Location

**Repository**: `https://github.com/voxer/server`
**Directory**: `fof/`
**Main Service**: `fof/fof.js`
**Pool Client**: `fof/pool.js`
