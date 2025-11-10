# Transcription Service

## Overview

The Transcription Service converts audio messages to text using speech-to-text APIs. It provides automated transcription of voice messages to improve accessibility and searchability.

## Entry Point

- **Main File**: `transcribe/transcribe.js`
- **Service Type**: `transcribe`
- **Base Class**: Extends `VoxerService`

## Key Responsibilities

1. **Audio Transcription**
   - Convert audio messages to text
   - Multiple language support
   - Accuracy optimization
   - Quality scoring

2. **Transcription Management**
   - Queue transcription jobs
   - Track transcription status
   - Store transcription results
   - Link transcriptions to messages

3. **Speech Recognition**
   - Integration with speech-to-text APIs
   - Audio format conversion
   - Quality preprocessing
   - Confidence scoring

4. **Search Integration**
   - Index transcribed text
   - Enable text search of voice messages
   - Highlight search terms in transcripts
   - Conversation search

## Transcription Flow

1. **Job Submission**
   - Receive audio message
   - Fetch audio from Body Store
   - Queue for transcription
   - Return job ID

2. **Processing**
   - Download audio file
   - Convert to required format
   - Send to speech-to-text API
   - Parse results

3. **Storage**
   - Save transcription text
   - Store confidence scores
   - Link to original message
   - Update search index

4. **Delivery**
   - Notify client of completion
   - Deliver transcript
   - Show in message UI

## Speech-to-Text Providers

Potential integrations:
- Google Cloud Speech-to-Text
- AWS Transcribe
- Azure Speech Services
- Custom models

## Language Support

- English (primary)
- Spanish
- French
- Other major languages
- Language auto-detection

## API Surface

HTTP endpoints for:
- Submit transcription job
- Check transcription status
- Get transcription result
- Batch transcription
- Language configuration

## Pool Client

- **`transcribe/pool.js`** - Pool client for connecting to Transcribe service
- Used by Node Router and Header Store
- Load balanced across instances

## Performance Characteristics

- Asynchronous processing (not real-time)
- Queue-based job processing
- Third-party API latency
- May take several seconds per message
- Batch processing for efficiency

## Quality Considerations

1. **Accuracy**
   - Confidence thresholds
   - Human review for low confidence
   - User feedback on transcriptions
   - Model improvements over time

2. **Audio Quality**
   - Noise reduction preprocessing
   - Format optimization
   - Sampling rate requirements
   - Handle poor quality gracefully

## Configuration

Key settings:
- Speech-to-text API credentials
- Supported languages
- Confidence thresholds
- Queue settings
- Cost optimization (when to transcribe)

## Cost Management

Transcription has per-minute costs:
- Selective transcription (premium feature?)
- Batch discounts
- Cache results
- Don't re-transcribe

## Integration Points

- **Body Store** - Fetch audio files
- **Header Store** - Store transcription metadata
- **Message Indexer** - Index transcribed text
- **Node Router** - Deliver results to clients
- **Speech-to-Text APIs** - External transcription

## Privacy & Security

- Audio data handling
- Transcription data retention
- End-to-end encryption considerations
- Compliance with privacy regulations

## Scaling Characteristics

- Horizontally scalable workers
- Queue-based processing
- Rate limiting per API provider
- Cost-aware scaling

## Code Location

**Repository**: `https://github.com/voxer/server`
**Directory**: `transcribe/`
**Main Service**: `transcribe/transcribe.js`
**Pool Client**: `transcribe/pool.js`
