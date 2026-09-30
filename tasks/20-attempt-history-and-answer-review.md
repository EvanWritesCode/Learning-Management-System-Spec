# Task 20 — Test result review and attempt history

## Context and dependencies
Task 19 writes immutable attempts with question snapshots. Learners need feedback for retention and should see how performance changes across unlimited retakes.

## Work
Build a results screen with score, threshold, pass/fail, response-by-response feedback, correct objective answers, explanations, and written-answer rubric/self-score. Build chapter attempt history ordered by submission time and mark the best score and current-revision passing attempt. Use each attempt's stored question snapshot and pass_percent_snapshot, never the chapter's live threshold, so question and threshold edits do not rewrite history. The UI exposes no attempt edit/delete actions and relies on the database restrictions from Tasks 03–04 for enforcement. Explain that deleting the owning chapter/course also deletes its attempt history through the controlled deletion workflow. Provide a Retake action that starts a clean draft for the current revision.

## Verification
Submit a pass and fail, edit a question to increment chapter revision, and confirm old attempts still show their original prompts/answers while current completion reflects the new revision. Change only the threshold from 80 to 90 and confirm an old 80% attempt still displays its original 80% threshold and pass, but is not marked as a current-revision pass. Repeat with a lowered threshold. Test history empty/error states and mobile readability.

## Done when
A learner can understand each result and inspect accurate historical attempts after course edits.
