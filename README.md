# QuickGrid Row Clicks Validation

Validation sample for QuickGrid row click support in Blazor, targeting .NET 11 RC1 and later.

## Build

Tested locally on the validation machine using the preview SDKs available there.

```text
.NET SDK 11.0.100-rc.1.26425.128 (preview present)
```

The repository does not include a `global.json`; add one to pin an SDK if you require exact reproduction.

## Sample applications

| Project | Render mode | Validation |
|---|---:|---|
| `BlazorServerApp/BlazorServerApp` | Interactive Server | Per-row click handling, propagation, and async callback ordering |
| `BlazorWasmApp/BlazorWasmApp.Client` | Interactive WebAssembly | Same validations as Server, standalone WASM client artifacts |

## How to run

From the repository root, run the chosen sample:

```powershell
dotnet run --project .\BlazorServerApp\BlazorServerApp\BlazorServerApp.csproj
dotnet run --project .\BlazorWasmApp\BlazorWasmApp\BlazorWasmApp.csproj
```

Open: `http://localhost:<port>/validation-test` for each app (the port is shown by `dotnet run`).

## How to verify

Build the projects and confirm successful output:

```powershell
dotnet build .\BlazorServerApp\BlazorServerApp\BlazorServerApp.csproj --no-restore -nologo -v:minimal
dotnet build .\BlazorWasmApp\BlazorWasmApp.slnx --no-restore -nologo -v:minimal
```

Manual verification checklist (UI):

- Confirm the page renders the summary panel, callback log, and validation checklist.
- Verify the data set includes row 1, row 10, and row 20.
- In Grid 1, click the cell content, padding, and row edges and confirm `row-click` is recorded and the selection panel updates.
- Click buttons and checkboxes inside a row and verify whether the row handler is invoked (propagation is not suppressed by default in this sample).
- For rapid-click scenarios use browser automation (Playwright/Selenium) to click rendered rows quickly and assert log order and final selection.

Detailed manual steps and expected outcomes are in the `Evidence/` folder.

## Configuration

Tested configurations:

- Windows host
- Interactive Server
- Interactive WebAssembly

Out of scope: Static SSR, MAUI Hybrid, and explicit standalone server-less automation.

## Evidence

- Manual validation steps and expected outcomes: `Evidence/` files
- Canonical validation report: `Evidence/69130-QuickGrid-Row-Clicks-Validation-Report-Updated.docx`
- Screenshots and videos: see the `Evidence/` directory

## Current validation status

The overall result is **partially passed**. Key points:

- Core row-click behavior is functional in both Interactive Server and Interactive WebAssembly.
- Mandatory class contract initially failed (selected rows used `selected-row`); this has been corrected to `selected` in both samples and CSS.
- Rapid-click helper buttons that directly invoked callbacks were removed from both samples; use browser automation to validate rapid-click behavior via the real event path.
- Propagation: the samples do not use `@onclick:stopPropagation` and now explicit messaging documents that nested control clicks reach the row handler.
- Builds: both projects build successfully with no errors and no warnings on the validation machine; an informational preview SDK notice (`NETSDK1057`) was observed during builds.

If you want automated rapid-click verification, add a small Playwright or Selenium harness to click DOM rows and assert callback logs; I can prepare that script and example run commands.
