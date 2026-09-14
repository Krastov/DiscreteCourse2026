# DiscreteCourse2026 Initial Repository Design

## Purpose

Create the initial repository structure for the course **离散数学与结构（I）**. The repository will separate the shared main notes from independently maintained student contributions.

## Source material

Copy the complete tracked contents of `git@github.com:Polaris-Aeterna/loom-notes.git` at source commit `290fa4042d33a7dd11f290eb39fba7f5d5b71369` into `main_notes/`.

The source repository's nested `.git` directory will not be copied. This makes `main_notes/` ordinary content owned by `PhotonYan/DiscreteCourse2026`, rather than a submodule or nested repository. Existing source files, examples, documentation, license, images, and templates will otherwise retain their paths and contents.

## Repository structure

```text
DiscreteCourse2026/
├── README.md
├── main_notes/
│   └── complete loom-notes snapshot
├── contribution/
│   └── README.md
└── docs/
    └── superpowers/specs/
        └── 2026-09-14-initial-course-repository-design.md
```

## Root documentation

The root `README.md` will:

- identify the course as **离散数学与结构（I）**;
- explain that `main_notes/` contains the shared main-note foundation;
- explain that `contribution/` is reserved for individual student notes;
- link readers to the contribution instructions.

## Contribution policy

`contribution/README.md` will state that:

- every contributor creates one personal subfolder directly under `contribution/`;
- the subfolder should be named with the contributor's name or GitHub username;
- personal notes, figures, and related files stay inside that personal subfolder;
- contributors must not place personal notes in or directly modify `main_notes/` for that purpose.

The initial setup will not create folders for named students, because their preferred public folder names are not yet known.

## Verification

Before publishing the initial setup:

1. Compare the copied file list and tracked file hashes against the source commit, accounting only for the removed nested `.git` metadata.
2. Confirm that `main_notes/` is not a submodule or nested Git worktree.
3. Confirm that both README files contain the course name and directory rules described above.
4. Check the final Git diff for malformed whitespace or unintended files.
5. Commit and push the resulting repository to the `main` branch of `PhotonYan/DiscreteCourse2026`.

## Out of scope

- Rewriting or customizing the Loom class or examples.
- Creating individual contributor folders before contributors choose their names.
- Configuring branch protection, automation, or deployment.
- Keeping an automatic synchronization link to the source repository.
