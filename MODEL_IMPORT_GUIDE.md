# Import your own model (LiteRT) into AI Edge Gallery

This guide explains the current ("latest") model import flow used by this app branch.

## What file can be imported

The app currently accepts only local model files with one of these extensions:

- `.litertlm`
- `.task`

If a file name contains `-web`, the app treats it as a web-only model and blocks import.

## Step-by-step import in the app (Android)

1. Open **Models** screen.
2. Tap **+** (Import model).
3. Choose **From local model file**.
4. Pick your `.litertlm` or `.task` file.
5. In **Import Model** dialog, set model defaults:
   - `defaultMaxTokens`
   - `defaultTopk`
   - `defaultTopp`
   - `defaultTemperature`
   - capability flags (`supportImage`, `supportAudio`, `supportTinyGarden`, `supportMobileActions`, `supportThinking`)
   - compatible accelerators (`CPU`, `GPU`, `NPU`, depending on device)
6. Tap **Import** and wait for copy to finish.

After import, the file is copied into app external storage under:

`<app_external_files>/__imports/<file_name>`

## How imported model is used by tasks

The app registers imported models as LLM models and enables tasks based on selected flags:

- Always enabled: **AI Chat**, **Prompt Lab**
- Enabled when `supportImage = true`: **Ask Image**
- Enabled when `supportAudio = true`: **Audio Scribe**
- Enabled when `supportTinyGarden = true`: **Tiny Garden**
- Enabled when `supportMobileActions = true`: **Mobile Actions**

## Notes for preparing your own model

- Use LiteRT-compatible model artifacts (commonly `.litertlm`; `.task` is also accepted by picker/import flow).
- For conversion/export pipeline, see LiteRT-LM project and model-specific docs.
- If import succeeds but generation fails, start with conservative defaults (`CPU`, lower max tokens) and tune up.

## Source references in this repo

- Import UI and copy flow: `Android/src/app/src/main/java/com/google/ai/edge/gallery/ui/modelmanager/GlobalModelManager.kt` and `.../ModelImportDialog.kt`
- Persisted imported-model schema: `Android/src/app/src/main/proto/settings.proto` (`ImportedModel`, `LlmConfig`)
- Imported-model runtime mapping: `Android/src/app/src/main/java/com/google/ai/edge/gallery/ui/modelmanager/ModelManagerViewModel.kt`
