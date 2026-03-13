# Changelog

All notable changes to this project will be documented in this file.
## [0.1.7] - 2026-03-13


### Features

- add experimental distilled (AI-optimized) rule variants in `aidlc-rules-distilled/` for reduced token consumption
- add `DISTILLATION-INSTRUCTIONS.md` — pipeline and quality checks for producing and maintaining distilled rules


### Documentation

- add distillation section to README: motivation, strategy, source of truth, benchmark results, and setup instructions


### Performance

- distilled rules achieve 57.3% total token reduction (18.4M → 7.9M) in sci-calc benchmark
- executor input tokens reduced 58.0%, max context size reduced 41.3%, wall clock time reduced 19.7%
- functional equivalence maintained: 100% unit tests, 88/88 contract tests, zero lint/security findings

## [0.1.6] - 2026-03-05


### Bug Fixes

- codebuild cache and download fix (#93)
- correct copy-paste error in error-handling.md (#96)


### Documentation

- update changelog for v0.1.5


### Features

- add CodeBuild workflow (#92)


### Miscellaneous

- add templates for github issues (#97)
## [0.1.5] - 2026-02-24


### Documentation

- update changelog for v0.1.4 (#88)
## [0.1.4] - 2026-02-24


### Bug Fixes

- correct GitHub Copilot instructions and Kiro CLI rule-details path resolution (#82, #84) (#87)


### Documentation

- update changelog for v0.1.3 (#77)
## [0.1.3] - 2026-02-11


### Bug Fixes

- require actual system time for audit timestamps (#56)


### Documentation

- clarify ZIP download location and consolidate notes (#70)
## [0.1.2] - 2026-02-08


### Bug Fixes

- typo in core-workflow.md
- rename rule and move to bottom of Critical Rules section


### Documentation

- update changelog for v0.1.1
- update README to direct users to GitHub Releases (#61)
- add Windows CMD setup instructions and ZIP note (#68)


### Features

- add test automation friendly code generation rules
- add frontend design coverage in Construction phase
## [0.1.1] - 2026-01-22


### Documentation

- update changelog for v0.0.5
- update changelog for v0.0.6


### Features

- adding AIDLC skill to work with IDEs such as Claude, OpenCode and others
- addin
- add leo file


### Miscellaneous

- removing wrong files
- removing wrong files
## [0.1.0] - 2026-01-22


### Features

- add Kiro CLI support and multi-platform architecture

