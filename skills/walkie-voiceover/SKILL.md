---
name: walkie-voiceover
description: Give a video a voice with Walkie. Turns a film's script into a voiceover audio file by asking the user's own Walkie app to narrate it, then hands the file to the video step. Use with brand-film, narrated-product-film, or any video skill when the user asks for a voiceover, narration, VO, "give it a voice", or a different voice or read for an existing cut.
---

# Walkie voiceover

Walkie is a speech app that runs on the user's computer. When the user has connected it to their assistant, the assistant can ask it to read a script aloud and save the result as an audio file. This skill uses that to voice a video: script in, voiceover file out, picture timed to the voice.

It works alongside a video skill and does not replace one. Let `brand-film` or `narrated-product-film` decide the story and the picture; use this skill for the voice.

## Check that Walkie is reachable

Look for the Walkie tools `list_narration_voices` and `narrate`. If they are there, carry on.

If they are missing, stop and tell the user plainly what to switch on. Do not swap in another voice service without asking.

1. Install Walkie from https://trywalkie.com, open it, and sign in. Connecting an assistant needs the Walkie Work plan or above.
2. Connect Walkie to this assistant on this computer. The steps for each assistant are at https://trywalkie.com/mcp/setup. Narration only works through the connection on the user's own computer, not the cloud one.
3. In Walkie, open Settings, then MCP, and turn on "Let your assistant add recordings and narrate". It is off by default.
4. Restart the assistant or start a new chat so the new tools appear.

If the tools are there but a call fails, read the message. The usual causes are that the Walkie app is closed or signed out.

Tell the user before the first narration that it counts toward their Read Aloud time in Walkie. Each new take counts again, so settle the script before recording it.

## Settle the script first

Get the words right before making audio. Write short, speakable lines, one claim per line, and read them aloud. Spell out anything that could be misread: numbers, abbreviations, product names, and unusual names.

Mark the delivery:

- **Tone** applies to a whole take: excited, happy, warm, calm, playful, serious, dramatic, deadpan, whispering, or sad.
- **Speed** runs from 0.7 to 1.3, with 1.0 as normal. Change it to fit a duration only in small steps.
- **A single moment** takes an English tag written into the text, such as `[pause]` before a turn, or `[whispering]` in front of one phrase. Tags are performed, not spoken. A `[pause]` is about one second.

Tone, speed, and tags apply to Walkie's cloud voices. If the voice list reports that Walkie is in On-Device Mode, one on-device voice speaks instead, the file is WAV, and tone and speed are ignored.

## Choose a voice

Call `list_narration_voices`. It returns each voice's name, gender, and accent, the languages available, and the default voice. Any voice can speak any listed language; the accent is the voice's own.

Do not pick silently. Offer two or three voices that fit the brand and the audience, say why, and let the user choose. They can hear voices first at https://trywalkie.com/mcp/voices or in the Voice Lab inside the Walkie app. If the user has no preference, use the default and say which voice it was.

Keep one voice for the whole film unless the script has more than one speaker.

## Narrate

Create the output folder first; `narrate` will not create it, and it will not overwrite an existing file. Put the audio where the video project keeps its sound, and number each take (`voiceover-take-01.wav`, `voiceover-take-02.wav`) so earlier takes survive.

Call `narrate` with the script as `text`, plus `voice`, `tone`, `speed`, `language`, `format`, and `output_path`. Ask for `wav` when the file goes into an edit or a render, and `mp3` only for a quick listen. One call takes about as long as the audio it makes, and it returns the file's path and its length in seconds.

Choose how to split the script:

- **One take for the whole script** gives the most natural read. Use it for short films where the picture can follow the voice.
- **One take per beat** gives a separate file and an exact length for every line, so each beat can be retimed or re-read without touching the others. Use it when picture has to cut on the lines, or when the script will keep changing.

A single call takes up to 5,000 characters, about five minutes. Split longer scripts at paragraph breaks and join the files in order.

## Hand the audio to the picture

The voice sets the timing. Before building or re-cutting the timeline:

1. Read the length of every take from the `narrate` result, or measure the file.
2. Find where each line starts. With one take per beat, the starts are the running total of the lengths plus the gaps you choose. With one long take, measure the pauses (for example with ffmpeg's `silencedetect`) or transcribe the file with word timings.
3. Give the video skill a beat table: line, audio file, start, and length. Cut picture to those times rather than stretching the voice to fit the picture. If a line runs long, shorten the line and narrate it again.

Leave about half a second of picture before the first word and enough after the last one for the ending to land. Mix music well below the voice.

For Remotion, put the file in `public/` and play it with an audio element at the frame the line starts. For an editor, import the WAV on its own track. To lay a finished take under an existing cut without re-rendering picture:

```bash
ffmpeg -i cut.mp4 -i voiceover-take-01.wav -filter_complex "[1:a]adelay=500|500,apad[a]" -map 0:v -map "[a]" -c:v copy -c:a aac -shortest with-voice.mp4
```

## Check and deliver

Listen to the whole take before using it. Then transcribe the finished video's audio and compare it with the script: every word present, names said correctly, nothing clipped at the start or end. Watch once for sync, with each action landing on the words that name it.

The voice is AI-generated. Say so wherever the video is published, and tell the user that this is their responsibility when they post it. Report which voice, tone, and speed were used so the next take or the next film can match.
