# Apache Magpie Study Notes

## README Understanding

### What Magpie is

Magpie is a reusable ASF-project automation framework made of agent-readable skills.

In simple words, Magpie helps Apache/open-source project maintainers automate or assist repeated project work such as issue triage, pull request management, security workflows, release management, contributor mentoring, and repository health checks.

It is not only one tool or one bot. It is a framework made of multiple skill families that agents can use inside adopter repositories.

### Adoption model

Magpie uses a **snapshot + agentic overrides** adoption model.

This means:

- A project downloads a local snapshot of the Magpie framework into `.apache-magpie/`.
- That snapshot is not committed to Git.
- The project commits only the bootstrap setup skill and configuration/override files.
- Project-specific behavior is written as Markdown overrides inside `.apache-magpie-overrides/`.

So the adopter project can use Magpie without vendoring the whole framework source.

### Committed files

These files/directories are expected to be committed in the adopter repository:

- `magpie-setup` skill
- `.apache-magpie.lock`
- `.apache-magpie-overrides/`
- `.gitignore` entries
- project documentation note about Magpie adoption

These are committed because they define how the project adopts and customizes Magpie.

### Gitignored files

These files/directories are expected to stay local and not be committed:

- `.apache-magpie/`
- `.apache-magpie.local.lock`
- runtime symlinks

These are gitignored because they are generated/downloaded runtime state.

### Key commands

Important setup and maintenance commands:

- `/magpie-setup`
- `/magpie-setup upgrade`
- `/magpie-setup verify`
- `/magpie-setup override <skill>`

### My current understanding

Magpie adoption works like this:

```text
Adopter project
        ↓
commits only the setup/bootstrap skill
        ↓
setup downloads Magpie framework snapshot into .apache-magpie/
        ↓
setup creates skill symlinks under .agents/skills/
        ↓
project-specific custom behavior lives in .apache-magpie-overrides/
        ↓
future contributors use .apache-magpie.lock to recreate the same setup
```

### Terms to remember

| Term | Meaning |
|---|---|
| ASF | Apache Software Foundation |
| Adopter | A project that uses Magpie |
| Skill | Agent-readable workflow/instruction pack |
| Snapshot | Downloaded local copy of Magpie framework |
| Gitignored | Present locally but not committed |
| Symlink | Shortcut pointing to another file/folder |
| Override | Project-specific customization |
| Lock file | File that records exact version/source |
| Drift | Mismatch between local install and committed lock |
| Skill family | Group of related Magpie skills |
