# Message Queue Update (Nov 16, Lea)

See `system_diagram.png` (blue parts = new).

**What I added (only `main.c`):**
- A message queue `xEventQueue` (10 events). The sensor task and Wi-Fi task will use it to send events to the state machine.
- An event format: `Event = { type, value1, value2 }`. Types: `EVT_PHONE_PLACED`, `EVT_PHONE_REMOVED`, `EVT_CHEAT_DETECTED`, and phone commands `EVT_CMD_START / PAUSE / RESUME / RESET / SET_TIME`.
- `EventToFlag()`: turns a queue event into a flag, e.g. `EVT_CMD_START` → `SHORT_PRESS_FLAG`.
- `StateMachineTask`: if there is no notification, it reads one event from the queue. **Your state logic is not changed.**

**How to send an event:**
```c
Event evt = { .type = EVT_PHONE_REMOVED };
xQueueSend(xEventQueue, &evt, 0);
```

**Test:** I made the button send events through the queue. Short press → STUDYING / PAUSE, long press → IDLE. All passed. Then I changed `ButtonTask` back.

**Not done yet:**
- The state machine does not handle the phone / cheat flags yet.
- Next for me: Wi-Fi task.
