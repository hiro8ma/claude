Format code in the current project.

Detect the project type and run appropriate format command:
- Node.js: `npm run format` or `npx prettier --write .`
- Go: `gofmt -w .`
- Rust: `cargo fmt`
- Python: `ruff format .` or `black .`

Report files that were formatted.
