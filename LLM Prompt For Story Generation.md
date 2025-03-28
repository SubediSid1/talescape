You are tasked with generating a JSON file for an interactive story in the format used by my app, using the `Scene`, `Chapter`, `Character`, and `Choice` models. The story features a dynamic conversation between "Alex" (the user, `isUser: true`) and "Bob" (the second character), with dialogue alternating to drive the narrative. Keep exchanges concise and natural, avoiding long monologues. The story spans multiple scenes, each advancing the plot via dialogue and ending with one choice. If the story involves mature themes (e.g., category "Hot"), focus on suggestive, playful interactions without explicit adult content, ensuring compliance with general content guidelines.

**Instructions:**

- **Task**: Generate JSON for one scene based on `scene_id`:
  - "scene1": Start fresh with metadata.
  - Higher `scene_id`: Continue from "{prior_scene_summary}" or infer from "{story_title}", "{category}", and `{scene_id}` if none provided.
- **Scene Structure**:
  - `story_id`: "{story_id}" (e.g., "star-voyage")
  - `scene_id`: "{scene_id}" (e.g., "scene1")
  - `background`: Unique Unsplash URL matching the scene and "{story_title}".
  - `chapters`: Exactly {chapters_per_scene} chapters, alternating "Alex" and "Bob":
    - `chapter_id`: Sequential (e.g., "ch1" for scene1, "ch7" for scene2 if scene1 has 6 chapters).
    - `text`: Concise dialogue (2-3 sentences max) advancing plot or character; for mature themes, use flirty or suggestive tones without explicit details.
    - `content_image`: Optional URL for 1-2 chapters if relevant (e.g., a party scene).
    - `character`: "Alex" (`isUser: true`, image: "https://ix-marketing.imgix.net/focalpoint.png") or "Bob" (`isUser: false`, image: "https://ix-marketing.imgix.net/footer-image.png").
    - `isUser`: `true` for "Alex", `false` for "Bob", `null` only if Narrator (avoid unless essential).
  - `choices`: 1 option with:
    - `text`: Descriptive choice text.
    - For non-last scenes: `next_scene` set to the next sequential scene (e.g., "scene2" for scene1).
    - For the last scene (e.g., `scene_id` = "scene{total_scenes}"): `next_scene` set to `null`, and add `next_story` set to "{story_id}\_next" (e.g., "star-voyage-next").
- **Metadata** (scene1 only):
  - `id`: "{story_id}" (e.g., "star-voyage")
  - `title`: "{story_title}" (e.g., "Star Voyage")
  - `cover_image`: Scene1’s `background` URL.
  - `total_scenes`: {total_scenes} (e.g., 3)
  - `description`: Short, engaging, based on "{story_title}" and "{category}"; for mature themes, keep it suggestive but subtle (e.g., "A night of playful intrigue.").
  - `category`: "{category}" (e.g., "Hot")
- **Next Request** (all scenes except the last):
  - A plain text string combining the request and summary for the next scene (e.g., "Generate scene2 of 'Star Voyage', 'star-voyage', Hot, 3 scenes, 150 chapters, story1, 50 chapters per scene. Prior scene summary: 'In scene1, Alex and Bob began a flirty adventure.'").
- **Output**:
  - Return two JSON objects and one plain text line, each on its own line with a label:
    - `// METADATA`: Metadata JSON (`{}` if not scene1).
    - `// SCENE`: Scene JSON.
    - `Next Request`: Plain text request for the next scene, including the summary (`<empty>` if last scene).
  - The two JSON blocks must be valid JSON, structured and copiable. Do not output unformatted strings within the JSON; ensure the JSON response is parseable with no additional text beyond the `//` labels and the `Next Request` line.

**Variables**:

- `{story_title}`, `{story_id}`, `{category}`, `{total_scenes}`, `{total_chapters}` (story scope, e.g., 150), `{chapters_per_scene}`, `{prior_scene_summary}`

**Example Output**:
// METADATA
{
"id": "star-voyage",
"title": "Star Voyage",
"cover_image": "https://images.unsplash.com/photo-1441974231531-c6227db76b6e",
"total_scenes": 3,
"description": "A steamy journey among the stars.",
"category": "Hot"
}
// SCENE
{
"story_id": "star-voyage",
"scene_id": "scene1",
"background": "https://images.unsplash.com/photo-1441974231531-c6227db76b6e",
"chapters": [
{
"chapter_id": "ch1",
"text": "Hey, this ship’s vibe is wild! Where are we headed?",
"content_image": null,
"character": {"name": "Alex", "image": "https://ix-marketing.imgix.net/focalpoint.png"},
"isUser": true
},
{
"chapter_id": "ch2",
"text": "Somewhere hot, Alex. Ready to flirt with the stars?",
"content_image": null,
"character": {"name": "Bob", "image": "https://ix-marketing.imgix.net/footer-image.png"},
"isUser": false
}
],
"choices": [{"text": "Flirt with Bob among the stars", "next_scene": "scene2"}]
}

For first scene:
Generate scene1 of 'Flames of Desire', 'flames-of-desire', Hot, 4 scenes, 120 chapters, 30 chapters per scene.
