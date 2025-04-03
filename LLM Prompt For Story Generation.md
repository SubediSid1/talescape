# Interactive Story Generation Prompt

You are tasked with generating a JSON file for an interactive story in the format used by my app, using the Scene, Chapter, Character, and Choice models. The story features a dynamic conversation between two characters, with random usernames generated for each character at the start. The user is one of the characters (isUser: true), and the second character will be generated with a random name as well.

The narrative should be enticing, playful, and filled with chemistry, subtly teasing sexual tension and excitement without being explicit. The dialogue should be suggestive and flirty, keeping the exchanges quick, natural, and enticing. Avoid long monologues, and let the conversation flow smoothly, making the reader feel immersed in the moment.

The story will span several scenes, each advancing the plot with dialogue that escalates in excitement, ultimately leading to an intriguing choice in the final scene that shapes the conclusion of the story. The arc should have a clear progression with a satisfying resolution at the end.

Make sure the characters' personalities come through strongly in their dialogue and actions. The tone should be confident, playful, and engaging, and the content should be best fitting for an adult audience.

## Instructions

- **Task**: Generate JSON for one scene based on `scene_id`:
  - "scene1": Start fresh with metadata and establish the story’s premise, introducing the main conflict or goal.
  - Higher `scene_id`: Continue from "{prior_scene_summary}" or infer from "{story_title}", "{category}", and `{scene_id}` if none provided, building toward the story’s resolution.
- **Story Arc**:
  - Plan the story across `{total_scenes}` scenes to have a complete narrative arc:
    - Scene 1: Introduce the setting, characters, and main conflict or goal (e.g., a mission, a mystery, or a personal challenge).
    - Middle scenes: Develop the conflict through dialogue and choices, escalating tension or stakes.
    - Last scene (`scene_id` = `scene{total_scenes}`): Resolve the main conflict or goal, providing a satisfying conclusion (e.g., the mission succeeds, the mystery is solved, or the characters reach an emotional resolution). The story must not end on a cliffhanger or imply a sequel.
- **Scene Structure**:
  - `story_id`: "{story_id}" (e.g., "star-voyage")
  - `scene_id`: "{scene_id}" (e.g., "scene1")
  - `background`: Unique Unsplash URL matching the scene and "{story_title}".
  - `chapters`: Exactly {chapters_per_scene} chapters, alternating "Alex" and "Bob":
    - `chapter_id`: Sequential (e.g., "ch1" for scene1, "ch31" for scene2 if scene1 has 30 chapters).
    - `text`: Concise dialogue (2-3 sentences max) advancing plot or character; for mature themes, use flirty or suggestive tones without explicit details.
    - `content_image`: Optional URL for 1-2 chapters if relevant (e.g., a party scene).
    - `character`: "Alex" (`isUser: true`, image: "https://ix-marketing.imgix.net/focalpoint.png") or "Bob" (`isUser: false`, image: "https://ix-marketing.imgix.net/footer-image.png").
    - `isUser`: `true` for "Alex", `false` for "Bob", `null` only if Narrator (avoid unless essential).
  - `choices`: 1 option with:
    - `text`: Descriptive choice text reflecting the story’s progression.
    - For non-last scenes: `next_scene` set to the next sequential scene (e.g., "scene2" for scene1).
    - For the last scene (`scene_id` = `scene{total_scenes}`): `next_scene` set to `null`. The choice should reflect a conclusive action that wraps up the story (e.g., "Celebrate your success"), ensuring the narrative feels complete. Additionally, the `text` should suggest a different story to read next (e.g., "Celebrate your success and try 'Moonlit Secrets' next"), and include:
      - `next_story`: A suggested story ID for a different story (not a sequel), e.g., "moonlit-secrets".
- **Metadata** (scene1 only):
  - `id`: "{story_id}" (e.g., "star-voyage")
  - `title`: "{story_title}" (e.g., "Star Voyage")
  - `cover_image`: Scene1’s `background` URL.
  - `total_scenes`: {total_scenes} (e.g., 3)
  - `description`: Short, engaging, based on "{story_title}" and "{category}"; for mature themes, keep it suggestive but subtle (e.g., "A night of playful intrigue.").
  - `category`: "{category}" (e.g., "Sci-Fi")
  - `category_id`: "{category_id}" (e.g., "scifi")
- **Next Request** (all scenes except the last):
  - A plain text string combining the request and summary for the next scene (e.g., "Generate scene2 of 'Star Voyage', 'star-voyage', Sci-Fi, scifi, 3 scenes, 90 chapters, 30 chapters per scene. Prior scene summary: 'In scene1, Alex and Bob began a thrilling space mission.'").
- **Output**:
  - Return two JSON objects and one plain text line, each on its own line with a label:
    - `// METADATA`: Metadata JSON (`{}` if not scene1).
    - `// SCENE`: Scene JSON.
    - `Next Request`: Plain text request for the next scene, including the summary (`<empty>` if last scene).
  - The two JSON blocks must be valid JSON, structured and copiable. Do not output unformatted strings within the JSON; ensure the JSON response is parseable with no additional text beyond the `//` labels and the `Next Request` line.

## Variables

- `{story_title}`, `{story_id}`, `{category}`, `{category_id}`, `{total_scenes}`, `{total_chapters}` (story scope, e.g., 90), `{chapters_per_scene}`, `{prior_scene_summary}`

## Example Output for Scene 1

// METADATA
{
"id": "star-voyage",
"title": "Star Voyage",
"cover_image": "https://images.unsplash.com/photo-1451187580459-43490279c0fa",
"total_scenes": 3,
"description": "A thrilling journey to find a lost artifact and save the colony.",
"category": "Sci-Fi",
"category_id": "scifi"
}
// SCENE
{
"story_id": "star-voyage",
"scene_id": "scene1",
"background": "https://images.unsplash.com/photo-1451187580459-43490279c0fa",
"chapters": [
{
"chapter_id": "ch1",
"text": "This ship is amazing, Bob! Where are we headed?",
"content_image": null,
"character": {"name": "Alex", "image": "https://ix-marketing.imgix.net/focalpoint.png"},
"isUser": true
},
{
"chapter_id": "ch2",
"text": "To the edge of the galaxy, Alex. Our colony’s dying—we need to find the lost artifact to save it.",
"content_image": null,
"character": {"name": "Bob", "image": "https://ix-marketing.imgix.net/footer-image.png"},
"isUser": false
}
],
"choices": [{"text": "Ask Bob about the artifact", "next_scene": "scene2"}]
}
// Next Request
Generate scene2 of 'Star Voyage', 'star-voyage', Sci-Fi, scifi, 3 scenes, 90 chapters, 30 chapters per scene. Prior scene summary: 'In scene1, Alex and Bob began a thrilling space mission to find a lost artifact that could save their colony.'

## Example Output for Last Scene

// METADATA
{}
// SCENE
{
"story_id": "star-voyage",
"scene_id": "scene3",
"background": "https://images.unsplash.com/photo-1507525428034-b723cf961d3e",
"chapters": [
{
"chapter_id": "ch61",
"text": "We did it, Bob! The artifact is powering up the colony!",
"content_image": null,
"character": {"name": "Alex", "image": "https://ix-marketing.imgix.net/focalpoint.png"},
"isUser": true
},
{
"chapter_id": "ch62",
"text": "You were amazing, Alex. Let’s celebrate back at base!",
"content_image": null,
"character": {"name": "Bob", "image": "https://ix-marketing.imgix.net/footer-image.png"},
"isUser": false
}
],
"choices": [
{
"text": "Celebrate with Bob and try 'Moonlit Secrets' next",
"next_scene": null,
"next_story": "moonlit-secrets"
}
]
}
// Next Request
<empty>

## Reminder for Requests

- First scene: "Generate scene1 of '{story_title}', '{story_id}', {category}, {category_id}, {total_scenes} scenes, {total_chapters} chapters, {chapters_per_scene} chapters per scene."

Request:
Generate scene1 of 'Dirty Little Secret', 'dirty-little-secret', Hot, hot, 3 scenes, 75 chapters, 25 chapters per scene.
Can you give it in json and 25 chapters for each scene starting from scene 1
