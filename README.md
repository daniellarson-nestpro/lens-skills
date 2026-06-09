# lens-skills

Public **sync mirror** of the Mindscale Lens agent skills. Paperclip's skill importer
clones public repos only, so skills authored in the private product repo
(`Mindscale-Partners/mindscale-lens/skills/`) are mirrored here at repo root for
syncing onto Paperclip agents (`owner/repo/skill-name` keys), matching the
`daniellarson-nestpro/aglg-brain-skills` pattern.

Canonical source of truth: `Mindscale-Partners/mindscale-lens/skills/<skill>/`.
Do not edit here directly; update the canonical copy and re-mirror.

## Skills
- `librarian-ingest` — Librarian ingestion capability: convert mixed source docs to clean
  markdown, classify + place into the Lens with valid gbrain `type:` frontmatter, dedup +
  no-contradiction against existing canon, commit + push so the gbrain autopilot syncs.
