# Task 08 — Versioned chapter JSON Schema and external-LLM prompt kit

## Context and dependencies
Tasks 03 and 07 define persisted chapter structure. The app never calls an LLM at runtime; users may use an external LLM and import one chapter at a time. This task defines the public import contract used by Task 09.

## Work
Create a downloadable JSON Schema with schemaVersion fixed to 1, a matching TypeScript/Zod validator, and a complete example chapter. The fields are title, summary, objectives (unique short keys plus text), ordered materials, assessment, and flashcards. Material variants: markdown with bodyMarkdown; article/video with HTTPS URL; PDF with title and an unresolved upload placeholder. The assessment has optional passPercent defaulting to 80 and at least one question. Question variants: single_choice, multiple_choice, short_answer, written. Every question has promptMarkdown, explanationMarkdown, and objectiveKeys. Choice questions include unique option keys and correct key(s); short answers include nonempty acceptedAnswers; written questions include modelAnswerMarkdown and rubricMarkdown. Cards have frontMarkdown and backMarkdown. Reject unknown schema versions, duplicate keys, invalid references, empty required content, and insecure URLs. Assign database UUIDs later, not in imported JSON.

Create copyable prompt templates for a course outline and full chapter, instructing an external LLM to return schema-valid JSON and to include recall, application, and explanation questions. Include author guidance on reviewing accuracy and rights to copied text. Do not implement a model API integration.

## Verification
Validate the example against both JSON Schema and Zod. Add valid/invalid fixtures for every material/question variant, duplicated objective keys, bad references, malformed URLs, and future schemaVersion. Confirm schema and example are downloadable from the app.

## Done when
An external author and a coding agent can produce and validate a chapter without guessing the import format.
