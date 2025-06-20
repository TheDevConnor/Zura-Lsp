# Zura Lsp and syntax highliter

NOTE: This is heavily based on [lsp-sample from vscode-extension-samples][sample] with the goal of removing example-specific code to ease starting a new Language Server.
NOTE: This was also my version of Jeffrey Chupp tutorial series <https://www.youtube.com/watch?v=Xo5VXTRoL6Q&list=PLq5tGLDKHlW9XKRj5-plHdvbkT5AOdM7->

## Getting Started

1. Clone this repo
2. Run `npm install` from the repo root.

## Running in Vscode

From the root directory of this project, run `code .` Then in VS Code

1. Build the extension (both client and server) with `⌘+shift+B` (or `ctrl+shift+B` on windows)
2. Open the Run and Debug view and press "Launch Client" (or press `F5`). This will open a `[Extension Development Host]` VS Code window.
3. Opening or editing a file in that window should show an information message in VS Code like you see below.

   ![example information message](https://semanticart.com/misc-images/minimum-viable-vscode-language-server-extension-info-message.png)

4. Edits made to your `server.ts` will be rebuilt immediately but you'll need to "Launch Client" again (`⌘-shift-F5`) from the primary VS Code window to see the impact of your changes.

[Debugging instructions can be found here][debug]

[debug]: https://code.visualstudio.com/api/language-extensions/language-server-extension-guide#debugging-both-client-and-server
[sample]: https://github.com/microsoft/vscode-extension-samples/tree/main/lsp-sample
[publish]: https://code.visualstudio.com/api/working-with-extensions/publishing-extension
[vsix]: https://code.visualstudio.com/api/working-with-extensions/publishing-extension#packaging-extensions

## Running in Nvim

Place a file like this into your ./nvim/lua/plugins/zura.lua:

```lua
return {
  "neovim/nvim-lspconfig",
  lazy = false,
  priority = 1000,
  config = function()
    -- Filetype setup
    vim.filetype.add({
      extension = { zu = "zura" },
      pattern = { [".*%.zu"] = "zura" },
    })

    -- LSP setup
    local lspconfig = require("lspconfig")
    local configs = require("lspconfig.configs")

    if not configs.zura_ls then
      configs.zura_ls = {
        default_config = {
          cmd = { "/home/thedevconnor/Zura-Transpiled/release/zura", "-lsp" },
          filetypes = { "zura" },
          root_dir = lspconfig.util.root_pattern(".git", ".zura-root"),
        },
      }
    end

    lspconfig.zura_ls.setup({})
  end,
}
```

Now, place the [`zura.vim`](./zura.vim) file into your `./nvim/syntax/` directory.
