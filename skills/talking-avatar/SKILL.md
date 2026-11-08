---
name: talking-avatar
description: A presenter on a green screen who speaks your script. Uses the Shotboard MCP tools; the Shotboard Mac app must be open.
---

Make a talking avatar on a chroma-key green background.

From the request, work out what the avatar looks like and the script they say. If the look isn't given, use: friendly presenter in their 30s, studio portrait, looking straight at the camera, shoulders up, soft light. If there's no script, ask for one.

1. Add the look as a prompt node and the script as a script node (add_text_node).
2. Avatar: generate_media, kind image, task text-to-image, prompt {"$node": look}, aspectRatio 9:16.
3. Green screen: generate_media, kind image, task image-edit, input reference = the avatar, prompt: "Keep the person exactly as they are: same face, hair, clothes, pose and framing. Replace the entire background with a flat, evenly lit chroma-key green (#00FF00). No shadows, gradients or green spill on the person."
4. Voice-over: generate_media, kind audio, task text-to-speech, prompt {"$node": script}.
5. Talking head: generate_media, kind video, task talking-head, inputs reference = the green-screen image and audio = the voice-over.

Queue every step at once (steps wait for their inputs), then wait_for_nodes on the talking head and arrange_nodes.
