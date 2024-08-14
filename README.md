# Getting started with typst

## Installation

Using `nix`
```sh
nix-shell -p typst
```
with home-manager - add to `home.packages` in `home.nix`
```nix
pkgs.typst
```
run latest development version
```sh
nix run github:typst/typst -- --version
```

## Editor integration

`Neovim`
References:
[Tinymist server configuration example](https://github.com/Myriad-Dreamin/tinymist/blob/main/editors/neovim)
[Lspconfig server configuration example](https://github.com/neovim/nvim-lspconfig/blob/master/doc/server_configurations.md#tinymist)
[My lspconfig](https://github.com/hrutvikyadav/nt4/blob/main/lua/riaari/lspconfig.lua)

### Patches

1. Install `tinymist` language server with `Mason`
2. Create autocommand to set correct filetype for `.typ` files
    ```lua
    vim.api.nvim_create_autocmd(
        {
            "BufNewFile",
            "BufRead",
        },
        {
            pattern = "*.typ",
            callback = function()
            local buf = vim.api.nvim_get_current_buf()
            -- vim.api.nvim_buf_set_option(buf, "filetype", "typst") -- WARN: deprecated
            vim.bo[buf].filetype = "typst"
            end
        })
    ```
    This is different from the example in the `tinymist` repository, because the example is deprecated.
    ```diff
    - vim.api.nvim_buf_set_option(buf, "filetype", "typst")
    + vim.bo[buf].filetype = "typst"
    ```
3. Add `tinymist` language server configuration to `lspconfig`
    ```lua
    return {
        --- todo: these configuration from lspconfig maybe broken
        single_file_support = true,
        root_dir = function()
            return vim.fn.getcwd()
        end,
        --- See [Tinymist Server Configuration](https://github.com/Myriad-Dreamin/tinymist/blob/main/Configuration.md) for references.
        settings = {}
    }
    
    ```

## Additional tooling

### Hot reloading

Install with `rust`
```sh
cargo install typst-live
```

**OR**

Run directly with `nix`
```sh
nix run github:ItsEthra/typst-live ./testfile.typ
```
