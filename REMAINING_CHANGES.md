# Remaining Jupyter Notebook Enhancement Changes

## Completed ✅
1. ✅ proto/cline/commands.proto - Added notebook_cell_json field
2. ✅ proto/cline/state.proto - Added enhanced_notebook_interaction_enabled field
3. ✅ src/core/controller/commands/addToCline.ts - Added notebook support
4. ✅ src/core/controller/commands/explainWithCline.ts - Added notebook support
5. ✅ src/core/controller/commands/fixWithCline.ts - Added notebook support
6. ✅ src/core/controller/commands/improveWithCline.ts - Added notebook support
7. ✅ src/core/controller/index.ts - Added enhancedNotebookInteractionEnabled to state
8. ✅ src/core/controller/state/updateSettings.ts - Added setting update handler

## Still Required 📋

### Core Mentions System
**src/core/mentions/index.ts**
- Add `enhancedNotebookInteractionEnabled` parameter to `parseMentions` function
- Update `getFileOrFolderContent` to accept and pass the flag
- Pass flag to `extractTextFromFile` calls

### System Prompts
**src/core/prompts/system-prompt/components/editing_files.ts**
- Add conditional section for Jupyter Notebook files
- Include JSON format requirements and examples
- Document \\n handling in notebook cells

**src/core/prompts/system-prompt/components/tool_use/examples.ts**
- Add Jupyter notebook editing examples

**src/core/prompts/system-prompt/tools/replace_in_file.ts**
- Add notebook-specific instructions

### Storage & State
**src/core/storage/utils/state-helpers.ts**
- Add `enhancedNotebookInteractionEnabled` to settings helpers

### Task System
**src/core/task/ToolExecutor.ts**
- Pass `enhancedNotebookInteractionEnabled` to parseMentions calls

**src/core/task/index.ts**
- Add `updateEnhancedNotebookInteractionEnabled` method
- Store setting in task config
- Pass to tool handlers

**src/core/task/tools/handlers/ReadFileToolHandler.ts**
- Pass enhanced notebook flag to extractTextFromFile

**src/core/task/tools/handlers/SummarizeTaskHandler.ts**
- Pass enhanced notebook flag if needed

**src/core/task/tools/handlers/WriteToFileToolHandler.ts**
- Pass enhanced notebook flag to extractTextFromFile

**src/core/task/tools/types/TaskConfig.ts**
- Add `enhancedNotebookInteractionEnabled?: boolean` field

### Extension Integration
**src/extension.ts**
- Extract notebook cell context from VS Code notebook API
- Pass notebook JSON to command handlers
- Handle .ipynb files in command registration

**src/hosts/external/ExternalDiffviewProvider.ts**
- Add notebook support methods

**src/hosts/vscode/VscodeDiffViewProvider.ts**
- Implement notebook cell extraction
- Handle notebook-specific diff views

**src/hosts/vscode/commandUtils.ts**
- Add utility functions for notebook cell extraction
- Helper methods for JSON manipulation

### Editor Integration
**src/integrations/editor/DiffViewProvider.ts**
- Add abstract methods for notebook support

**src/integrations/misc/extract-file-content.ts**
- Pass enhanced notebook flag through

**src/integrations/misc/extract-text.ts**
- Add special handling for .ipynb files
- Preserve JSON structure when flag is enabled
- Return raw JSON instead of extracted text for notebooks

### Registry
**src/registry.ts**
- Export notebook-related utilities if needed

### Shared Types
**src/shared/ExtensionMessage.ts**
- Add `enhancedNotebookInteractionEnabled?: boolean` to ExtensionState interface

**src/shared/storage/state-keys.ts**
- Add `"enhancedNotebookInteractionEnabled"` to state keys

### WebView UI
**webview-ui/src/components/settings/sections/FeatureSettingsSection.tsx**
- Add checkbox/toggle for "Enhanced Notebook Interaction"
- Wire up to state management

**webview-ui/src/context/ExtensionStateContext.tsx**
- Add `enhancedNotebookInteractionEnabled?: boolean` to context type

## Key Implementation Notes

### Notebook Cell JSON Format
When extracting notebook cells, the JSON should include:
```json
{
  "cell_type": "code",
  "execution_count": null,
  "metadata": {},
  "outputs": [],
  "source": [
    "line1\\n",
    "line2\\n",
    "line3"
  ]
}
```

### Critical Formatting Rules for .ipynb
- Each line except the last ends with `\\n`
- Each source line is a separate JSON string in array
- Quotes and commas must match exactly
- Preserve JSON structure in SEARCH/REPLACE blocks

### VS Code Notebook API
Use these APIs for notebook handling:
- `vscode.workspace.notebookDocuments`
- `vscode.window.activeNotebookEditor`
- `notebookDocument.getCells()`
- `cell.document.getText()`

## Testing Checklist
- [ ] Proto files compile successfully
- [ ] Settings UI shows new toggle
- [ ] Toggle persists across sessions
- [ ] Commands work with .ipynb files
- [ ] Notebook cell context is extracted
- [ ] JSON format is preserved in edits
- [ ] Regular files still work normally
- [ ] extractTextFromFile handles flag correctly
