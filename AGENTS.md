# Convex Backend

See `BUILD.md` for building from source, `CONTRIBUTING.md` for contribution
guidelines, and `npm-packages/AGENTS.md` / `crates/AGENTS.md` for
package-specific development workflows.

## Cloud-specific instructions

### C++ toolchain gotcha

The default C++ compiler on this VM is `clang-18`, which fails to find
`<memory>` and `-lstdc++` when CMake passes
`--target=x86_64-unknown-linux-gnu`. Set `CC=gcc CXX=g++` for all `cargo`
commands (build, test, clippy) to avoid this:

```sh
CC=gcc CXX=g++ cargo build -p local_backend
CC=gcc CXX=g++ cargo test -p value
CC=gcc CXX=g++ cargo clippy -p value --no-deps
```

### Node.js version

The repo requires Node.js 20 (see `.nvmrc`). Activate it with:

```sh
source /home/ubuntu/.nvm/nvm.sh && nvm use
```

### Running the local backend + demo

1. Start the backend: `CC=gcc CXX=g++ cargo run -p local_backend --bin convex-local-backend`
   - Listens on `0.0.0.0:3210` (HTTP) and `0.0.0.0:3211` (dev proxy).
2. Push demo functions: `cd demo && just convex dev --once`
3. Run demo frontend: `cd demo && npm run dev:frontend`
   - Default port 5173 (falls back to 5174 if busy).

### Key commands reference

| Task | Command |
|------|---------|
| Install JS deps | `npm clean-install --prefix scripts && just rush install` |
| Build all JS | `just rush build` |
| Build Rust backend | `CC=gcc CXX=g++ cargo build -p local_backend` |
| Lint JS (dashboard) | `cd npm-packages/dashboard-self-hosted && npm run lint` |
| Lint Rust | `CC=gcc CXX=g++ cargo clippy -p <crate> --no-deps` |
| Test Rust crate | `CC=gcc CXX=g++ cargo test -p <crate>` |
| Test JS package | `cd npm-packages/<pkg> && npx vitest run <file>` |
| Run Convex CLI | `just convex <args>` (auto-sets admin key + local URL) |
