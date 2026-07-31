# testrepo — JupyterLite with xeus-cpp and an in-browser terminal

A static, serverless [JupyterLite](https://jupyterlite.readthedocs.io/) deployment for GitHub Pages featuring:

- **xeus-cpp** — a C++ Jupyter kernel compiled to WebAssembly ([jupyterlite-xeus](https://jupyterlite-xeus.readthedocs.io/)), so C++ notebooks run entirely in the browser.
- **In-browser terminal** — [jupyterlite-terminal](https://jupyterlite-terminal.readthedocs.io/) (powered by [cockle](https://github.com/jupyterlite/cockle)), providing a bash-like shell with `coreutils` (`ls`, `cat`, `cp`, `mv`, `rm`, `mkdir`, `touch`, `wc`, ...), `nano`, `vim`, `grep`, `sed`, `less`, `tree`, `lua`, and a `git` implementation (`git2cpp`).

No backend server is required — everything runs client-side via WebAssembly.

## Deploying

1. Push this repository to GitHub.
2. In the repository **Settings → Pages**, set the source to **GitHub Actions**.
3. In **Settings → Actions → General**, ensure workflow permissions are set to **Read and write permissions**.
4. Push to `main` (or re-run the workflow). The site builds and deploys automatically via [.github/workflows/deploy.yml](.github/workflows/deploy.yml).
5. Your site will be available at `https://<your-username>.github.io/<repo-name>/`.

## Customizing

- **Kernel packages**: edit [environment.yml](environment.yml). It uses the `emscripten-forge` channel to install WebAssembly builds of packages (e.g. add `numpy`, other xeus kernels, etc.) alongside `xeus-cpp`.
- **Build-time tooling** (the CI machine's own Python env, e.g. `jupyterlite-core`, `jupyterlite-xeus`, `jupyterlite-terminal` versions): edit [.github/build-environment.yml](.github/build-environment.yml).
- **Notebooks / files shipped with the site**: add them under [content/](content/).
- **Terminal toggle**: controlled by `terminalsAvailable` in [jupyter-lite.json](jupyter-lite.json).

## Local build

```bash
micromamba create -f .github/build-environment.yml -n jupyterlite-site
micromamba activate jupyterlite-site
jupyter lite build --contents content --output-dir dist
jupyter lite serve --output-dir dist
```
