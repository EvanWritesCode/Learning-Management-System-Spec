# Task 09 — Chapter JSON import with validation and preview

## Context and dependencies
Tasks 04, 07, and 08 provide owner security, draft courses, and the chapter-v1 contract. Import must create a new draft chapter, not overwrite existing content.

## Work
Build file-pick and paste-JSON entry points, parse/validate locally, display field-level errors, and preview title, objectives, materials, questions, answer keys, cards, and unresolved PDF placeholders. Require the owner to choose the destination course and confirm. Implement one database-side import operation, for example a security-invoker Postgres RPC, that checks course ownership and inserts the chapter plus children in one transaction. Preserve source order, generate UUIDs, default passPercent to 80, and return the new chapter ID. Keep PDF placeholders unresolved until Task 12 uploads files; draft preview must identify them. On failed import, do not create partial rows or erase the submitted JSON.

## Verification
Import the Task 08 example and compare persisted order/count/content. Test invalid JSON, invalid schema, another user's course ID, duplicate submit, and injected HTML/unsafe URLs. Force a child-insert failure and confirm transaction rollback. Confirm imported chapter remains a draft and existing chapters are unchanged.

## Done when
One complete, validated chapter can be imported predictably with no partial data and no cross-user access.
