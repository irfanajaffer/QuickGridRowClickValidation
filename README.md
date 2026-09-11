# QuickGridRowClickValidation

Validation sample for QuickGrid row click support in Blazor, targeting .NET 11 RC1 and later.

## Sample route

Open `/validation-test` in either app.

- Server app: `BlazorServerApp/BlazorServerApp`
- WebAssembly app: `BlazorWasmApp/BlazorWasmApp`
- Standalone WASM sample page: `BlazorWasmApp/BlazorWasmApp/BlazorWasmApp.Client/Pages/ValidationTest.razor`

## Run the sample

Server app:

```powershell
dotnet run --project BlazorServerApp/BlazorServerApp/BlazorServerApp.csproj
```

WebAssembly app:

```powershell
dotnet run --project BlazorWasmApp/BlazorWasmApp/BlazorWasmApp.csproj
```

## What the sample covers

- Two QuickGrid instances over the same 20-item in-memory data source.
- Row click handling with async completion, selection tracking, and callback logging.
- RowClass styling for selected rows.
- A comparison grid with no `OnRowClick` handler and no clickable row styling.
- Action button and checkbox interactions inside each row.
- Rapid click helper buttons for concurrent callback validation.
- A selection details panel with timestamped state.

## Validate the matrix

1. Open `/validation-test` and confirm the summary panel, log panel, and validation checklist render.
2. Verify the data set includes row 1, row 10, and row 20.
3. In Grid 1, click the row content, cell padding, left edge, and right edge for the same row and confirm the row highlights and the selection panel updates.
4. Click row 1, then row 10, before the first callback completes and confirm the log shows start and complete entries for both rows.
5. Use the rapid click helper buttons to queue the same row twice and multiple rows rapidly, then confirm the log order and final selected item.
6. Click the Action button inside a row and confirm the button log appears and the row click is also recorded.
7. Toggle the Checkbox inside a row and confirm the checkbox log appears and the row click is also recorded.
8. Compare Grid 1 and Grid 2 cursor and hover styling. Grid 1 should show clickable styling; Grid 2 should not.
9. Confirm the selection details panel shows the selected item ID, name, category, price, and last selected timestamp.

## Deployment checks

- Published output: run `dotnet publish` for the relevant project and verify the page still renders at `/validation-test`.
- .NET 10 to .NET 11 upgrade: rebuild after updating the target framework and confirm the QuickGrid package reference still resolves.
- Trimmed publish: publish with trimming enabled and verify the page loads and the logs still update.
- AOT publish: for the WASM project, publish with AOT enabled and confirm the page still loads in the browser.
- Hot Reload: use `dotnet watch` on either app and confirm UI edits are reflected without a full restart.
- Reverse proxy deployment: verify the route works behind the proxy and that the static assets for QuickGrid load correctly.
- Multi-server deployment: verify the app behaves the same behind multiple instances because the sample only uses in-memory state in the browser session.

## Notes

- Row click validation requires an interactive host to execute the callbacks. Static SSR can render the page markup, but it will not execute the click handlers until the component is interactive.
- The sample uses only standard Blazor and QuickGrid APIs.
