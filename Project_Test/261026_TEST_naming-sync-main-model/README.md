# Naming-convention sync from the main model only TEST

Plan: [261026_PLAN_naming-sync-main-model](../../Project_Plan/261026_PLAN_naming-sync-main-model.md)

## Managed regression

The check lives in the naming-convention smoke, which owns the save-time sync:
[`261023_TEST_panel-cladding-naming-convention`](../261023_TEST_panel-cladding-naming-convention/README.md)
(`Program.cs`, `VerifyMainModel`). It covers `PanelCladdingProjectService.IsMainModelPath`:

- accepted: `BKT - Wireframe.3dm` and `MET - Wireframe.3dm` with their full project paths,
  `met - wireframe.3DM`, `P00020 - Wireframe.3dm`, surrounding spaces;
- rejected: `MET - Wireframe_STN.3dm`, `BKT - Wireframe - v2.3dm`, `BKT - Wireframe (1).3dm`,
  `BKT - Wireframe copy.3dm`, `BKT_old - Wireframe.3dm`, `bkt-wireframe.3dm`, `XYZ_Wireframe.3dm`,
  `BKT - STN.3dm`, `BKT.3dm`, `.3dmbak`/`.rhl`/no extension, `RhinoAutosave.3dm`,
  ` - Wireframe.3dm`, blank and null;
- `MainModelFileName(" met ")` is `MET - Wireframe.3dm`.

The layer-standard smoke still checks `CodeFromDocumentPath`, which this change does not alter.

```powershell
dotnet run --project Project_Test/261023_TEST_panel-cladding-naming-convention/PanelCladdingNamingConventionSmoke.csproj -c Debug
dotnet run --project Project_Test/261023_TEST_panel-cladding-naming-convention/PanelCladdingNamingConventionSmoke.csproj -c Release
```

## Live check (inside Rhino, after 1.2.1 is installed and attested)

1. Note the hash and time of `MET - Naming Convention.xlsx`.
2. Open and save `MET - Wireframe_STN.3dm`: the command line shows once
   `MET - Naming Convention.xlsx is updated only when MET - Wireframe.3dm is saved; MET - Wireframe_STN.3dm only reads it.`
   and the sheet hash and time are unchanged. A second save prints nothing.
3. Open and save `MET - Wireframe.3dm`: the sheet is reconciled as before (or reported unchanged).

The save hook runs inside Rhino; a build does not prove it.
