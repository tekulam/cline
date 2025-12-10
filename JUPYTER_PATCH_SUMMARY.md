# Jupyter Enhancements for Cline - Patch Summary

## Overview
This patch adds enhanced Jupyter Notebook (.ipynb) support to Cline, including:
- Notebook cell context awareness
- Improved editing capabilities for .ipynb files
- Special handling of notebook JSON format
- Settings toggle for enhanced notebook interaction

## Changes Required

### 1. Proto Files

#### proto/cline/commands.proto
- Add `notebook_cell_json` field to CommandContext message

#### proto/cline/state.proto
- Add `enhanced_notebook_interaction_enabled` field to UpdateSettingsRequest

### 2. Core Controller Commands

#### src/core/controller/commands/addToCline.ts
- Add notebook cell JSON context support
- Check for notebook files (.ipynb)
- Include raw cell JSON in context

#### src/core/controller/commands/explainWithCline.ts
- Add notebook cell JSON to explanation context

#### src/core/controller/commands/fixWithCline.ts
- Add notebook cell JSON to fix context

#### src/core/controller/commands/improveWithCline.ts
- Add notebook cell JSON to improvement context
- Handle cases with/without selected text

### 3. Controller State Management

#### src/core/controller/index.ts
- Add `enhancedNotebookInteractionEnabled` to state

#### src/core/controller/state/updateSettings.ts
- Handle updates to `enhancedNotebookInteractionEnabled` setting

### 4. Mentions System

#### src/core/mentions/index.ts
- Pass `enhancedNotebookInteractionEnabled` flag through mention parsing
- Update `getFileOrFolderContent` to accept the flag

### 5. System Prompts

#### src/core/prompts/system-prompt/components/editing_files.ts
- Add special section for Jupyter Notebook files
- Document JSON format requirements
- Provide examples for editing .ipynb files

#### src/core/prompts/system-prompt/components/tool_use/examples.ts
- Add Jupyter notebook editing examples

#### src/core/prompts/system-prompt/tools/replace_in_file.ts
- Add notebook-specific instructions

### 6. Task System

#### src/core/storage/utils/state-helpers.ts
- Add `enhancedNotebookInteractionEnabled` to settings

#### src/core/task/ToolExecutor.ts
- Pass enhanced notebook flag to parseMentions

#### src/core/task/index.ts
- Add `updateEnhancedNotebookInteractionEnabled` method
- Store and use the setting

#### src/core/task/tools/handlers/ReadFileToolHandler.ts
- Pass enhanced notebook flag to extractTextFromFile

#### src/core/task/tools/handlers/WriteToFileToolHandler.ts
- Pass enhanced notebook flag to extractTextFromFile

#### src/core/task/tools/types/TaskConfig.ts
- Add `enhancedNotebookInteractionEnabled` to TaskConfig

### 7. Extension Integration

#### src/extension.ts
- Add notebook cell context extraction
- Pass notebook JSON to commands
- Handle .ipynb files specially

#### src/hosts/external/ExternalDiffviewProvider.ts
- Add notebook support

#### src/hosts/vscode/VscodeDiffViewProvider.ts
- Add extensive notebook cell handling
- Extract cell context from notebooks

#### src/hosts/vscode/commandUtils.ts
- Add utility functions for notebook handling

### 8. Editor Integration

#### src/integrations/editor/DiffViewProvider.ts
- Add abstract methods for notebook support

#### src/integrations/misc/extract-file-content.ts
- Pass enhanced notebook flag

#### src/integrations/misc/extract-text.ts
- Add special handling for .ipynb files
- Preserve JSON structure when flag is enabled

### 9. Registry

#### src/registry.ts
- Export notebook-related utilities

### 10. Shared Types

#### src/shared/ExtensionMessage.ts
- Add `enhancedNotebookInteractionEnabled` to ExtensionState

#### src/shared/storage/state-keys.ts
- Add state key for the setting

### 11. WebView UI

#### webview-ui/src/components/settings/sections/FeatureSettingsSection.tsx
- Add UI toggle for enhanced notebook interaction

#### webview-ui/src/context/ExtensionStateContext.tsx
- Add `enhancedNotebookInteractionEnabled` to context

## Implementation Strategy

1. Update proto files first
2. Update shared types and state management
3. Update core controller and commands
4. Update task system and tool handlers
5. Update integration layers
6. Update UI components
7. Test with .ipynb files
