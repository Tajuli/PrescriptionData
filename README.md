# PrescriptionData

Public, read-only-for-consumers template distribution for Prescription Android. Maintainers publish reviewed templates here; no patient information or credentials may be stored.

## Canonical snapshot

`country_templates.json` is the single authoritative UTF-8 JSON array consumed by Prescription PR #8 and intended for PrescriptionAdmin. Publish the whole validated array in one atomic GitHub contents commit. The raw endpoint is:

https://raw.githubusercontent.com/Tajuli/PrescriptionData/main/country_templates.json

Consumers require no token. Admin writers need a fine-grained credential restricted to this repository and Contents write access. Never embed that credential in source or APK. Use the latest blob SHA when updating to detect concurrent changes.

## Template contract

Each entry has a stable globally unique `id` (bundled- followed by lowercase letters, digits and hyphens), ISO 3166-1 alpha-2 `country`, nonempty `name`, `keywords`, `medicines` array and `advicePoints` string array. IDs must never be recycled for a different complaint. The existing Android model is the authority for field names and types. Optional fields can be omitted. No extra fields in the snapshot.

Medicine strings: name (required nonblank), form, strength, morning, afternoon, night, bedtime, customDose, meal, duration, unit, frequency, route, instructions, eligibility, optionGroup, ingredient, country. weightBased is a boolean. Country is normalized to the parent template by the Android app. Use the app's existing dose and duration fields; do not infer a dose from a complaint alone. All regimens must be clinically reviewed before publication.

Example (empty, intentionally not a treatment recommendation):

```json
{
  "id": "bundled-bd-hairfall-male",
  "country": "BD",
  "name": "Hair Fall Male",
  "keywords": "hairfall alopecia চুল পড়া পুরুষ",
  "medicines": [],
  "advicePoints": []
}
```

## Publishing

Read the current snapshot and SHA, modify a draft, validate the full snapshot against the contract, review the changes and publish using the current SHA. Do not publish patient records, unreviewed regimens, personal details, tokens or secrets. Avoid emptying a previously populated snapshot inadvertently. Git history provides an audit trail and rollback source. A public commit can remain accessible even after deletion.

The Android client is intended to preserve local clinician edits and deletions. A new or modified default is not a patient-specific prescription; the treating clinician must assess suitability before use. Synchronization depends on the client version, network, and Android background scheduling.

The initial data copies the existing app complaint catalog without adding unapproved medicines or advice.
