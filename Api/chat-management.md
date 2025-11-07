# Chat Management API

Chat thread creation, participant management, and chat settings.

## Endpoints

- `2/cs/start_thread` - Create new chat thread
- `2/cs/chats_list` - Get list of user's chat threads
- `2/cs/get_thread_details` - Get detailed information about a thread
- `chat/add_participants/1` - Add participants to a chat
- `chat/remove_participants/1` - Remove participants from a chat
- `2/cs/leave_chat` - Leave a chat (group only)
- `chat/delete` - Delete a chat
- `chat/modify_name/1` - Change chat title/name
- `chat/controlled_chat/1` - Set chat as controlled (admin-only posting)
- `chat/suppress_notifications/1` - Mute notifications for a chat
- `chat/enable_notifications/1` - Unmute notifications for a chat
- `chat/enable_extreme_notifications/1` - Enable extreme notifications for a chat
- `2/cs/mod_thread_tags` - Modify thread tags
- `chat/summarize/1` - Generate AI summary of chat conversation
