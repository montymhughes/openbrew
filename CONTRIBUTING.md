# Contributing to openbrew

Thanks for your interest in contributing! openbrew is intentionally simple — a single Python file with no dependencies — and we'd like to keep it that way.

## Reporting bugs

Open an issue on GitHub with:
- What you ran (the command)
- What you expected to happen
- What actually happened
- Your OS and Python version (`python3 --version`)

If the issue involves your journal file, include a minimal example that reproduces the problem (not your actual data).

## Suggesting features

Open an issue to discuss before writing code. openbrew aims to stay small and focused, so not every feature will be a good fit. A feature is more likely to be accepted if it:

- Solves a real problem you've encountered while using openbrew
- Doesn't add external dependencies
- Keeps the single-file architecture
- Follows the plain-text philosophy (human-readable, greppable, git-friendly)

## Making changes

1. Fork the repo and create a branch from `main`
2. Make your changes to the `openbrew` file
3. Test with the included `example.journal` to make sure nothing breaks
4. Update the CHANGELOG if your change is user-facing
5. Open a pull request with a clear description of what you changed and why

## Adding new fields

This is the most common type of contribution. The pattern is:

1. Add the field to the `Bean` or `Brew` class `__init__`
2. Add a parser line in `parse_journal` to recognise it
3. Add a serializer line in `serialize_bean` or `serialize_brew` to write it
4. Optionally add an interactive prompt in `cmd_add_bean` or `cmd_add_brew`
5. Update the README field reference table

## Code style

- Keep it simple and readable
- No external dependencies
- Match the existing style (the codebase is intentionally straightforward Python)
- Comments where the "why" isn't obvious

## License

By contributing, you agree that your contributions will be licensed under the MIT License.
