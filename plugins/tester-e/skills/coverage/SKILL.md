---
name: coverage
description: Check whether tests in this repository verify everything their JIRA / Azure DevOps item asks for, list the gaps, and add the missing steps.
disable-model-invocation: true
argument-hint: "[suite | test key …] [--apply]"
---

Does each test actually cover the item it was written for? Follow the `tester-e-tests`
skill's rules.

1. Read `.tester-e/config.yaml`. Work out the tests from `$ARGUMENTS`: a test key
   (`<suite>/<name>`) or a test directory; nothing → ask, rather than analyse (and pay
   for) every test. Tests must be synced: find each with `find_test_cases`
   (`metadata_key: external_key`, `metadata_value: "<repo>::<key>"`). A test with no
   `issue:` in its file and no key among its tags is not linked — say which, and offer to
   add `issue: PROJ-123`, then `/tester-e:sync`, instead of guessing an item.
2. **Say the price first.** Call `check_issue_coverage` with `dry_run: true` for each test;
   it returns the linked item and the credits (one per step) and spends nothing. Show the
   total and wait for a yes. A test with `supported: false` is skipped, with its `reason`.
3. Run `check_issue_coverage` for each confirmed test, one at a time (30-120 s each).
   If a file changed since the last sync, say the analysis reads the synced version.
4. Report plainly, per test: verdict and percentage, the one-sentence summary, then each gap —
   the requirement, why it is missing, the proposed step. Read the proposal against the test
   and the app's source: the proposal is a draft, and its selector may be a guess.
5. **Adding the steps** (only with `--apply`, or when the user agrees, gap by gap):
   write the step into the test's YAML file — the repository is the source of truth — validate
   with `validate_test_cases`, then `upsert_test_cases` as `/tester-e:sync` does. Say that
   updating a synced case is free. Do not use `apply_coverage_proposals` for these tests: a
   change made only on tester-e is overwritten by the next sync. It is for tests that are not
   kept in a repository.
6. Do not change a test to raise its percentage without verifying the behaviour; a step that
   asserts nothing does not close a gap. Offer to re-run the analysis afterwards, and say that
   costs credits again.
