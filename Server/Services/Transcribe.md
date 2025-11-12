# Transcription Service

**Repository**: `https://github.com/voxer/server`
**Location**: `transcribe/transcribe.js`

## Overview

The Transcription Service converts audio messages to text using speech-to-text APIs. The service extends the VoxerService base class and provides automated transcription of voice messages.

## Audio Transcription

The service converts audio messages to text with support for multiple languages. Transcription results include quality scores and confidence metrics.

## Transcription Management

The service queues transcription jobs, tracks their status, and stores results linked to the original messages. A pool client in `transcribe/pool.js` provides load-balanced connections across instances.

## Speech Recognition

The service integrates with speech-to-text APIs including Google Cloud Speech-to-Text, AWS Transcribe, Azure Speech Services, and custom models. Audio undergoes format conversion and quality preprocessing before submission. Results include confidence scoring.

## Search Integration

The service indexes transcribed text for search functionality. Voice messages become searchable through their transcription text with term highlighting in results.

## Transcription Flow

Audio messages are received and fetched from the Body Store. Jobs are queued and processed by downloading audio files, converting to required formats, and submitting to speech-to-text APIs. Results are parsed, stored with confidence scores, linked to messages, and indexed for search. Clients receive completion notifications and transcripts appear in the message UI.

## Language Support

The service supports English, Spanish, French, and other major languages. Language auto-detection is available.

## API Surface

The service provides HTTP endpoints for job submission, status checks, result retrieval, batch transcription, and language configuration.

## Performance Characteristics

Processing is asynchronous and queue-based. Jobs take several seconds per message due to third-party API latency. Batch processing improves efficiency. The service is horizontally scalable with rate limiting per API provider.

## Quality Considerations

The service uses confidence thresholds and collects user feedback on transcriptions. Audio preprocessing includes noise reduction and format optimization. Poor quality audio is handled gracefully.

## Configuration

Key configuration parameters include speech-to-text API credentials, supported languages, confidence thresholds, queue settings, and cost optimization rules.

## Cost Management

Transcription incurs per-minute API costs. The service implements selective transcription, batch discounting, result caching, and avoidance of re-transcription.

## Dependencies

The service integrates with the Body Store for audio files, the Header Store for transcription metadata, the Message Indexer for text indexing, the Node Router for result delivery, and external speech-to-text APIs. It uses the Relay Client for Riak access.

## Privacy & Security

The service handles audio data and transcription data with retention policies. Privacy regulations are addressed in the context of end-to-end encryption.
