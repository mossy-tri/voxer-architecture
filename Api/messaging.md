# Messaging API

Message sending, reading, reactions, search, and AI-powered features.

## Endpoints

- `3/cs/post_message` - Send text message, file, or audio
- `3/cs/post_message_live` - Send live streaming audio message
- `message/revox/1` - Forward/revox a message to another thread
- `2/cs/recall_messages` - Recall/delete messages from a thread
- `3/cs/consume_messages` - Mark messages as read
- `3/cs/like_messages` - Like or unlike messages
- `1/chat/search_messages` - Search messages globally across all threads
- `1/cs/share_message` - Share a message publicly
- `message/transcribe/3` - Transcribe audio message to text
- `message/summarize/1` - Generate AI summary of message
- `message/translate/1` - Translate message to target language
- `get_offsets/1` - Get message offsets for synchronization
