# Self-built caller: Twilio + xAI Grok realtime voice — Advanced (technical users)

Read only when the owner's Calling method is the self-built caller. All rules in "Phone calls" in the RV trip planning skill still apply. For Twilio building blocks, see the twilio-voice-conversation-relay plugin skill.

Only if the owner wants it, can run code, and sets up their own accounts. Everyone else: use a hosted service or call sheets.
- **Needs:** the owner's own Twilio account and a voice-capable number, plus an xAI API key with credit. An X Premium subscription does not include API access. Store keys as described under Secrets.
- **Trial accounts:** a Twilio trial account can only call verified numbers and plays a trial notice. The test call to the owner's own phone works on a trial. Calling parks needs an upgraded (funded) account.
- **How it works:**
  - Twilio places the outbound call with a `TimeLimit` of about 6 minutes (360 seconds). The TwiML uses `<Connect><Stream>`, a bidirectional Media Stream (μ-law, 8 kHz).
  - A small websocket bridge on the bot's computer relays the audio to xAI's realtime voice model. Use the current model from xAI's docs (the `grok-voice-latest` alias tracks the newest). Set μ-law 8 kHz for both input and output if the API supports it; otherwise transcode.
  - The bridge relays audio only. It never saves audio. The transcript comes from the model's transcription events.
  - Opening: the model waits for the office, then gives the disclosure line from Call style.
  - Phone menus: the model waits for the whole menu, then presses the key for reservations or the office. Keys are sent as in-band DTMF tones on the media stream. If that fails, end the call and make a call sheet.
  - Answering machine detection (Twilio async AMD) is a hint only. Business greetings and menus often score as "machine". The model decides from a clear beep or "leave a message" prompt.
  - Time limit: the bridge starts a polite wrap-up at about 5:15. Twilio's `TimeLimit` ends the call at about 6:00 as a backstop.
  - The model gets tools: `wait_silently` (stay quiet while staff look things up), `send_dtmf` (press keys), `end_call` (hang up through the Twilio API), and `voicemail_detected`.
  - A public HTTPS tunnel exposes the bridge. Some sandboxes only allow outbound port 443, so Cloudflare quick tunnels (port 7844) may fail. An SSH-over-443 tunnel such as Pinggy works. Free tunnels may expire after about an hour, which is fine for one call.
  - After the call, a text model pulls the structured answers (availability, prices, discounts, other answers) out of the transcript.
  - Put the website facts in a per-call facts file, so the caller never asks what's posted.
- **Security:**
  - Validate the `X-Twilio-Signature` header on every webhook.
  - Give each call a random token on its callback and stream URLs, and expire it when the call ends.
  - Use a Twilio API key and secret, not the main account auth token, where possible.
  - The tunnel provider ends the encryption at its end, so it can see the audio. Use a provider you trust.
- **Cost** at US pay-as-you-go list prices, as of 2026-10-18 (check current pricing at https://www.twilio.com/en-us/voice/pricing/us and https://docs.x.ai/developers/pricing):
  - Twilio outbound voice: $0.014/min
  - Twilio Media Streams: $0.0044/min
  - xAI realtime voice: $0.08/min, plus $0.004 per text input sent to the model
  - Answering machine detection: $0.0075 per call
  - Answer extraction: a fraction of a cent per call
  - Total: about $0.10 a minute. A typical 2–3 minute call is about $0.21–$0.31. The 6-minute cap is about $0.61 at most. Show this maximum before each call.
  - Plus the Twilio number fee (about $1.15 a month for a US local number).
- **Caller ID:** calls from an individual Twilio account are usually signed with B attestation (SHAKEN/STIR). A attestation and Voice Integrity (spam-label cleanup with the carriers) need a registered Twilio business profile. Spam labels come mostly from carrier analytics, so a new number may still show "Spam Likely". Keep call volume low and expect some unanswered calls.
- **First run:** always do a test call to the owner's own phone first, with their OK. The owner plays the park office.

Re-check prices before quoting a cost if the "as of" date is older than 90 days. Store keys per "Secrets" in the RV trip planning skill; never print them.
