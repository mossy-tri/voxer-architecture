# Voxer Architecture Overview

## Code

- [iOS Client](Code/ios.md)
- [Server](Code/server.md)
- [Unison](Code/unison.md)
- [Web Client](Code/webclient.md)

## Reference

- [Server Architecture, Design Principles, and Coding Style](Reference/Server%20Architecture%2C%20Design%20Principles%2C%20and%20Coding%20Style.md)
  Documents the production operation for the Voxer service, including scalability principles, data storage strategies (Riak, Data Store, Redis), service-oriented architecture, and detailed coding standards for the Node.js codebase. Emphasizes building a telecommunications infrastructure designed for 24/7 availability with tolerance for single machine/process failures.

- [How_Voxer_Works.pptx.pdf](Reference/How_Voxer_Works.pptx.pdf)
  Explains Voxer's core innovation as a communication protocol combining synchronous (live) and store-and-forward messaging without their respective drawbacks. Details how the system enables live message delivery without requiring end-to-end connections, using a store-and-stream protocol with conversation-based routing.

- [Server Architecture.pdf](Reference/Server%20Architecture.pdf)
  Presents the high-level technology choices (Node.js, Riak, Redis, SmartOS) and visual diagrams of the live messaging flow, consistent hashing implementation, and component interactions between NR, HS, BS, and NS services. Illustrates how the system achieves scalability through horizontal scaling and distributed architecture.

- [Voxer architecture and feature list.docx.pdf](Reference/Voxer%20architecture%20and%20feature%20list.docx.pdf)
  Comprehensive feature list describing Voxer's capabilities including live/messaged voice integration, offline operation, multi-device support, and advanced incoming call management (screen, join, catch-up-to-live). Details the ring-based cluster architecture and data storage across Router Ring, Header Store Ring, Body Store Ring, and Riak clusters.

- [Voxer Architecture Discovery.pdf](Reference/Voxer%20Architecture%20Discovery.pdf)
  Architecture discovery report from Oct 2021 providing detailed analysis of the current system and recommendations for migration to Google Cloud. Identifies scalability issues with JSON-based configuration and Redis compaction, proposing solutions using etcd/Redis for configuration management, Spanner/BigTable for database replacement, and GCS optimization for media storage.
