# The audit

An audit is an independent read of the finished work against the requirements. It fixes nothing, and it is not the closing decision. Its value is an independent reader aimed at the places you already believe are weak.

## Dispatching it

Use one dedicated auditor for the whole implementation run. Spawn it for the first audit, separate from the workers who implemented the work, and keep its handle for every later audit. Give it the requirements as the user stated them and tell it to judge the code against the spec and the repository's rules.

Hand it your suspects: the parts you most doubt, and any earlier finding you want re-verified. A short suspect list aims an audit better than a longer brief does.

## What comes back

A finding is an input, not a verdict. An auditor working from the requirements alone misreads what it has not read closely, and it cannot tell a deliberate decision from a mistake. Check each finding before acting on it, and record the ones you drop, with the reason, so the next audit does not raise them again.

Audit again after the fixes by sending a follow-up task to the same auditor. Give it the changes and findings to re-verify, including why you dropped any findings. Have it inspect the current code so its conclusions reflect the fixes.
