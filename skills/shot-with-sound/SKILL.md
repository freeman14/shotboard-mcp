---
name: shot-with-sound
description: A still, animated, with matching sound effects, merged into one clip. Uses the Shotboard MCP tools; the Shotboard Mac app must be open.
---

Turn a shot description into one video clip with sound.

From the request, work out the shot and the motion. If the motion isn't given, use a slow cinematic push-in.

1. Add the shot description as a prompt node (add_text_node).
2. Still: generate_media, kind image, task text-to-image, prompt {"$node": shot}, aspectRatio 16:9.
3. Clip: generate_media, kind video, task image-to-video, prompt = the motion, input first_frame = the still.
4. Sound: generate_media, kind audio, task video-to-audio, prompt {"$node": shot}, input video = the clip.
5. Final: compose_video with the clip and the sound as its audio.

Queue the generation steps at once, wait_for_nodes on the sound, then compose and arrange_nodes.
