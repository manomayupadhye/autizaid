# Pause Point · Wearable Speech Simulation (prototype)

A single-file WebXR simulation of an assistive facial wearable for children aged 3–8 with autism and/or ADHD. It listens to everyday speech in a school setting and shows **3–5 simple keywords with picture cues** instead of full subtitles.

> **Note:** This is a prototype for demonstration only. It has not been clinically validated and is not a diagnostic or therapeutic tool.

## How the speech-to-keyword pipeline works

`Person speaks → wearable hears → speech to text → split into sentences → filler removed → keywords picked out → keywords and icons shown`

1. **Hear / text.** Live input comes from the browser's Web Speech API. Demo input comes from the characters in the scene, or from sentences you type in Conversations → Demo input.
2. **Split.** The text is split into sentences at punctuation and at spoken joiners such as "and then" or "but".
3. **Simplify.** Filler words are removed ("hey", "um", "please", "just"). Negations such as "don't" are never removed.
4. **Keywords.** Ordered phrase rules sort words into the categories ACTION, OBJECT, LOCATION, QUESTION, WARNING, SOCIAL and INFO. "Don't run" becomes **DON'T RUN**, shown with a crossed-out icon.
5. **Meaning.** Each sentence gets a type: safety, instruction, question, invitation, greeting, feeling or information. A polite request such as "could you…?" counts as an instruction, not a question. A feeling is shown only when the speaker says it in words.
6. **Show.** The display keeps the highest-priority keywords: safety first, then instructions, questions, social, places and things. Safety keywords appear at the top and the rest follow in spoken order. If recognition confidence is low or nothing useful was heard, the display shows **NOT CLEAR** rather than guessing.

All three input sources go through the same `processSpeech()` function and the same wearable display. The analyser can be swapped out: see `RemoteAnalyzerPlaceholder` in the `wearable-core` script. Never put API keys in the page. Call your own server instead, which keeps the key.

## Controls

| | |
|---|---|
| Move | W A S D or arrow keys · Shift to move faster |
| Look | Mouse (click to lock the pointer) or drag · Scroll to zoom · F switches between first and third person |
| Talk | Walk up to a person and press E · R repeats · X dismisses |
| Other keys | M turns the microphone on or off · P pauses · H hides the help |
| VR | Left stick moves · right stick snap-turns 30° · trigger talks · A repeats · B dismisses · X pauses · Y toggles the mic |

## Host on GitHub Pages and open it on a Meta Quest

1. Create a new **public** GitHub repository, for example `pausepoint-vr`.
2. Upload **`index.html`** to the root of the repository. Keep that exact file name. Add this README too if you like.
3. Open **Settings → Pages**. Under *Build and deployment*, set *Source* to **Deploy from a branch**, then choose **main** and **/(root)**, and click **Save**.
4. Wait about one to two minutes. The site will be at `https://<your-username>.github.io/pausepoint-vr/`. It uses HTTPS automatically, and both the microphone and WebXR need HTTPS.
5. On the Quest, open the **Meta Quest Browser** and go to that URL. You can bookmark it, or send the link from your phone with the Meta Horizon app.
6. Set up the experience *before* entering VR:
   - Press **Start**.
   - Adjust **Settings** if needed.
   - Optionally turn on the **Mic**.
7. Press **Enter VR** (bottom right) and allow the immersive-session prompt. Stand in a clear play space.

To test on a computer, open the same URL in desktop Chrome or Edge. To run it locally, use `python3 -m http.server`, because `localhost` also counts as a secure context.

## Browser support for speech recognition

- **Chrome and Edge on desktop:** live recognition works. Chrome's built-in recognition may send audio to the browser maker's speech service to convert it to text. The page itself does not record, store or upload audio.
- **Safari:** support is partial and depends on the version.
- **Firefox:** no live recognition, so the page runs in a clearly labelled **DEMO MODE**.
- **Meta Quest Browser:** speech recognition support is limited and unreliable. If the recogniser fails or is missing, the status badge switches to DEMO MODE, and the character conversations and typed demo input still work through the same pipeline. Text-to-speech voices also vary between devices. For a reliable live-mic demonstration, use desktop Chrome or Edge.

## Implemented now vs. future work

| Implemented in this prototype | Needs a future AI backend or dedicated hardware |
|---|---|
| Desktop and WebXR (Quest) school scene: classroom with a Pause Point corner, corridor, playground and gate | Real semantic understanding: paraphrases, idioms, sarcasm, context across sentences |
| Microphone capture with a visible indicator and level meter, on/off control, no audio stored | Speaker separation (who is talking) and direction-of-voice cues |
| Live Web Speech recognition, with a DEMO MODE fallback | Robust recognition in noisy classrooms, Indian English accents and code-mixed speech, other Indian languages |
| Local rule-based segmentation, filler removal, keyword extraction, categories, negation, priority, NOT CLEAR | On-device, offline and private speech processing on the wearable itself |
| One wearable display shared by desktop and VR (the same canvas): icons, category tags, high contrast | Real optics: display in the field of view, eye-safe brightness, battery, comfort and fit for children |
| Six talking characters with predefined lines, plus a scenario selector and typed demo input | Personalised vocabulary and symbol sets (for example matched to a child's AAC symbols) |
| In-world cue rings on objects that are mentioned (bag, notebook, ball, wet-floor sign…) | Object recognition through a camera, to point at real-world items |
| Sensory settings: audio, text size, contrast, animation, display time, keyword count, recognition, view height; always-available pause | Clinical co-design, validation and safeguarding with therapists, teachers and families |
| A replaceable analyser interface (`RemoteAnalyzerPlaceholder`) | A secure server proxy for any external AI API (keys never in the client) |
