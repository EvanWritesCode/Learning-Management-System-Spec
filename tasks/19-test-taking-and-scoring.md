# Task 19 — Chapter test-taking and scoring

## Context and dependencies
Tasks 13, 14, and 18 provide authored questions, revision rules, and completion calculation. A test is available only for a published chapter and requires connectivity.

## Work
Render all four question types, keep unsubmitted answers in local draft storage scoped to user/chapter/revision, and restore them after reload. Grade single choice, exact-set multiple choice, and short accepted answers deterministically. Normalize short answers by case and surrounding/repeated whitespace. For written answers, collect the learner's response, reveal model answer/rubric only after the response is committed for self-review, and let the learner assign zero or one point. Each question is one point; compare the unrounded points/question-count percentage with the threshold captured for that chapter revision. Submit one immutable attempt with answer and question snapshots, pass_percent_snapshot, score, pass result, and chapter revision. In the submission transaction, verify the chapter is still published, active, and at the expected revision, including its passPercent; reject a stale submission without erasing its draft and explain that the learner must start the current test. Do not relabel old answers as belonging to a newer revision. No application UPDATE or individual DELETE path may modify the submitted attempt. Clear the local draft only after confirmed submission. Support unlimited retakes.

## Verification
Unit-test all grading types and threshold boundaries, including 79.999... versus 80, blank answers, and duplicate multi-select options. Test reload before submission, offline submission prevention, duplicate-click protection, and chapter revision changes during a test. Specifically race a passPercent-only edit with submission and verify the attempt is either committed under the original revision/threshold before the edit or rejected as stale afterward. Verify a failed write does not erase answers or create a false pass. Re-run direct-API immutability checks from Task 04.

## Done when
The learner can complete a detailed chapter test, self-score written responses, and obtain a correct persisted pass/fail result.
