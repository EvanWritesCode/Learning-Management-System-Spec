# Task 04 — Owner-scoped database and PDF storage security

## Context and dependencies
Task 03 created the schema. Every course is private to its owning authenticated user. Frontends will use only the Supabase publishable key; there is no custom server.

## Work
Enable row-level security and explicit operation-specific grants on every exposed table. Permit access only through a course owned by auth.uid(); a user must not assign a child record to another user's course. Progress, attempts, and reviews must also be scoped to the signed-in user. Give test_attempts owner-scoped SELECT and the intended submission INSERT path, with no direct UPDATE or DELETE grant/policy for application roles. Enforce immutable updates in the database as well. Authorized deletion of an owning chapter/course may cascade its attempts; individual attempt deletion and attempt mutation are never exposed through a generic CRUD/RPC path.

Create a private PDF bucket with a 50 MB limit and PDF MIME restriction. Use user/course/material/unique-object paths so replacing a PDF never overwrites the active file. Scope file_operations to its owner and validate target ownership when creating a job; callers cannot register another user's path or an unrelated live object for cleanup. Storage policies must enforce both the authenticated prefix and the owner relation, including the exact authorized pending operation when its target is pending deletion or already removed. Keep that authorization until cleanup finishes. Task 12 adds guarded lifecycle operations and prevents direct writes/deletes from bypassing them; Task 14 similarly closes bypasses around readiness and revision rules. Any privileged helper must check auth.uid(), constrain its inputs, and have a fixed search_path and narrowly granted execution. Use the Storage API for file deletion; do not edit storage metadata tables directly. Ensure anonymous requests cannot read course rows or private PDFs.

## Verification
Create two local test accounts and run SQL/policy tests for reads and writes at each level: permitted own-record operations succeed, cross-user and anonymous requests fail. Using the public key plus user sessions, prove even an attempt's owner cannot UPDATE or DELETE it directly or through an alternate RPC; authorized parent deletion still removes its attempts and cross-owner parent deletion fails. Test direct private-object download, upload, overwrite, and delete attempts. Verify cleanup authorization survives target deletion but cannot be forged to remove a live or foreign object. Re-run these tests when Tasks 12 and 14 add guarded mutations. Verify no secret/service-role key is included in frontend configuration.

## Done when
Database and Storage privacy holds even if a client calls Supabase directly outside the UI. Document the bucket name and path contract for later tasks.
