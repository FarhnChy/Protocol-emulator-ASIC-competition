# Tiny Tapeout template baseline

## Scope and provenance

This stage establishes the official template flow only. No programmable engine
has been implemented. The original README text, competition rules, Git history,
and `origin` remote are preserved. No upstream `.git` directory was imported.

Inspected live with Git on October 31, 2026:

- Template repository: https://github.com/TinyTapeout/ttihp-verilog-template
- Default branch: `main`, commit `4b1204ab387b5ab94db1d57067051d8d89a2f83c`.
- Other upstream branches: `cmos5l` at
  `231f26a58b8d2efbab25bef24b29c94362a431a3` and `ttihp0p2` at
  `5045d84c8b501a8c00a1fcbbd42e726298cd9cda`.
- Imported all tracked files from that `main` commit, except that README.md
  was merged with our existing README. Local changes also fill in project
  metadata, set 6x4 tiles, replace datasheet placeholders, and add this guide.
- Official action `ttihp27a` resolved to
  `e7a50f8f46f9f8df4b2c9935bfeb1cbc6b6ae4f6` when inspected.

The live default branch differs from the stale web snapshot used in an earlier
response. Use the commit-linked files below as evidence:

- [GDS workflow](https://github.com/TinyTapeout/ttihp-verilog-template/blob/4b1204ab387b5ab94db1d57067051d8d89a2f83c/.github/workflows/gds.yaml):
  `pdk: ihp-sg13cmos5l`, with `ttihp27a` for GDS, precheck, gate-level tests,
  and viewer. All these action references are kept unchanged.
- [Action implementation](https://github.com/TinyTapeout/tt-gds-action/blob/e7a50f8f46f9f8df4b2c9935bfeb1cbc6b6ae4f6/action.yml):
  explicitly accepts `ihp-sg13cmos5l`, selects LibreLane `3.1.0.dev4`, and
  explains its Magic and ciel requirements.
- [Template Makefile](https://github.com/TinyTapeout/ttihp-verilog-template/blob/4b1204ab387b5ab94db1d57067051d8d89a2f83c/test/Makefile):
  defaults gate-level models to CMOS5L, including separate UDP models when present.
- [Metadata](https://github.com/TinyTapeout/ttihp-verilog-template/blob/4b1204ab387b5ab94db1d57067051d8d89a2f83c/info.yaml):
  lists 6x4 as supported. Our `info.yaml` selects it.
- The imported devcontainer also selects CMOS5L and LibreLane `3.1.0.dev4`.

The older `cmos5l` branch uses the experimental `ihp-cmos5l` action reference;
it also retains older devcontainer defaults and tile comments. The complete
current `main` configuration is used instead of mixing these branches.
Official action references and their support-tools dependency remain upstream
defaults, so future runs may pick up upstream dependency changes.

## Hardware and tests

`src/project.v` remains the upstream `tt_um_example` combinational adder.
`test/tb.v`, `test/test.py`, and `test/Makefile` remain unchanged and agree on
the module/source names. The sample test drives 20 and 30 and expects 50.
This is a template smoke test, not protocol or reset-safety verification.

On Linux with Python, GNU Make, Icarus Verilog (`iverilog` and `vvp`) installed:

```sh
python -m pip install -r test/requirements.txt
cd test
make -B
python -m cocotb_tools.check_results results.xml
```

The result checker is required to confirm test success. The official CI uses
Python 3.11 and installs its dependencies itself. For gate-level simulation,
first generate the GDS/netlist, provide the corresponding PDK models and
`test/gate_level_netlist.v`, then run `make -B GATES=yes` and the result checker.
The official `gl_test` job handles that setup automatically.

## Checks on this computer

- Python 3.12.10 and Git are available.
- WSL reports that Windows Subsystem for Linux is not installed.
- GNU Make, Icarus Verilog, vvp, Verilator, Yosys, cocotb-config, Docker,
  and GitHub CLI are absent from PATH. Cocotb and pytest are not installed
  in the current Python environment.
- No Linux distribution or local ASIC toolchain was installed.
- PyYAML 6.0.3 was installed only into a temporary directory for static checks.
- Git whitespace/integrity checks, YAML/JSON/JSONC parsing, Python syntax compilation,
  source/test/metadata consistency, and upstream file-integrity checks passed.
- Two initial static-check attempts needed checker fixes: sandbox access to the
  temporary YAML package required an escalated read, and the devcontainer file
  uses valid JSONC comments rather than strict JSON. The corrected checks passed;
  no upstream template file was changed to accommodate either issue.
- RTL simulation, HDL lint/synthesis, GDS generation, precheck, gate-level
  simulation, and timing closure have NOT been run. No baseline pass is claimed.
- GitHub Actions has not been launched from this setup session. Results must be
  checked on the exact pushed commit before declaring the baseline established.

## GitHub settings

In `FarhnChy/Protocol-emulator-ASIC-competition`:

1. Under **Settings > Actions > General**, enable Actions and allow the actions
   referenced by the official workflows. If an organization restricts third-party
   actions, its policy must permit these dependencies too. No custom secrets are
   referenced by these workflows; they use the normal GitHub token.
2. Under **Settings > Pages > Build and deployment > Source**, select
   **GitHub Actions**. This is required for the GDS viewer deployment, not RTL
   simulation or precheck. The viewer job already requests `pages: write` and
   `id-token: write`; no blanket write-token setting is needed for that job.
   See the [official Pages troubleshooting instructions](https://tinytapeout.com/faq/#my-github-action-is-failing-on-the-pages-part).
3. If Pages is unavailable for your repository visibility/account plan, address
   that GitHub limitation before expecting the viewer to pass. Core GDS,
   precheck, and gate-level job results can still be assessed individually.
4. Check Actions runner availability/billing limits if GitHub refuses to start
   the Ubuntu runners. Repository settings have not been inspected remotely.

## Commit, push, and run

From the repository root in PowerShell, review and commit the explicit file set:

```powershell
git status --short
git diff -- README.md
git add README.md COMPETITION_RULES.md LICENSE .gitignore .devcontainer .vscode .github docs info.yaml src test
git diff --cached --check
git diff --cached --stat
git diff --cached
git commit -m "Establish official Tiny Tapeout CMOS5L template baseline"
git push -u origin main
```

The current branch is `main`. This session does not commit or push for you.
Configure GitHub settings before pushing if possible: a push automatically
launches **test**, **gds**, and **docs**. The **fpga** workflow is imported
unchanged and normally runs only by manual dispatch; it is optional here.

To launch again manually, open the repository's **Actions** tab, choose **test**
or **gds**, click **Run workflow**, select **main**, and confirm **Run workflow**.
Manual dispatch appears after the workflow exists on the default branch.
There is no separate precheck workflow: **gds** runs **precheck**, **gl_test**,
and **viewer** after its **gds** job succeeds. **docs** can also be dispatched.
Avoid dispatching duplicates while push-triggered runs are already active.

For the exact committed SHA, require green **test**, **gds**, **precheck**, and
**gl_test** jobs. Inspect physical-flow timing reports as well; a configured
clock period is not evidence of timing closure. Check **docs** and **viewer**
separately. Download the RTL `test-results`, `GDS_logs`, `tt_submission`, and
`gatelevel_test_results` artifacts and record run URLs/results before calling
the baseline passing. If a job fails, retain its logs and make the smallest
setup fix without changing the example hardware.
