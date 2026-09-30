# Task 13 — Chapter test question authoring

## Context and dependencies
Tasks 07 and 08 provide chapters, objectives, and the four question contracts. Tests are central to this reading-heavy app; the same private course owner creates and studies them. Test-taking is Task 19.

## Work
Build forms to add, edit, reorder, and delete single-choice, multiple-choice, short-answer, and written questions. Provide Markdown prompt and explanation fields. Require at least one linked objective and keep links valid when objectives change. Choice editors must have unique option keys and valid answer selection; short-answer editors need accepted variants; written questions need model answer and rubric. Show a question preview and an objective-coverage list. Preserve form state after validation or network errors. For published chapters, route changes through the revision behavior defined in Task 14.

Task 14 must enforce readiness and revision changes atomically for these mutations, including direct API calls. Reject deletion of the last published question or any edit that leaves invalid answer/objective data; retain the existing published test and the user's form input.

## Verification
Create and reload one of each type; reorder with keyboard and pointer. Test invalid/missing answers, duplicate option keys, missing objective references, and empty rubric. Confirm another owner cannot edit by using a forged question ID.

## Done when
A creator can compose a substantial mixed-format chapter test with explicit answers, feedback, and objective coverage.
