# QuickGrid Row Clicks Validation

Validation sample for QuickGrid row click support in Blazor, targeting .NET 11 RC1 and later.

## Build

Validated locally on Windows using the SDK pinned by `global.json`:

```text
.NET SDK 11.0.100-rc.1.26425.128
```

Both sample applications build successfully with no errors or warnings. The builds may display `NETSDK1057`, an informational notice that a preview .NET SDK is in use.

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

The completed UI validation covered the following scenarios:

- Confirm the page renders the summary panel, callback log, and validation checklist.
- Verify the data set includes row 1, row 10, and row 20.
- In Grid 1, click the cell content, padding, and row edges and confirm `row-click` is recorded and the selection panel updates.
- Click buttons and checkboxes inside a row and verify whether the row handler is invoked (propagation is not suppressed by default in this sample).
- Rapid row clicks preserve the expected callback log order and final selection.

Detailed manual steps and expected outcomes are in the `Evidence/` folder.

## Configuration

Tested configurations:

- Windows host
- Interactive Server
- Interactive WebAssembly

Static SSR, MAUI Hybrid, and standalone server-less automation were outside the requested validation scope.

## Evidence

- Manual validation steps and expected outcomes: `Evidence/` files
- Canonical validation report: `Evidence/69130-QuickGrid-Row-Clicks-Validation-Report-Updated.docx`
- Screenshots and videos: see the `Evidence/` directory

## Current validation status

The overall result is **passed**. All requested validation scenarios and requirements were executed and verified in both Interactive Server and Interactive WebAssembly. No tests are pending.

### Problems Found

None. No functional, rendering, behavioral, accessibility-related, or validation-blocking issues were identified. All scenarios within the requested validation scope behaved as expected, and no product defects were observed.

### Not Covered

None. All explicitly requested test cases, configurations, and mandatory validation scenarios were completed; no requested validation was omitted.
