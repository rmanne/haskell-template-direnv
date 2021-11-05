# haskell-template-direnv

Haskell project template optimized for use with nix flakes, haskell-language-server, and editors that can integrate with direnv and haskell-language-server. This has been primarily tested on spacemacs, but it should work just as well for vim (and its variants) and vanilla emacs.

This template is heavily inspired by [srid/haskell-template](https://github.com/srid/haskell-template).

## Getting Started

### Install and configure nix

1. [Install nix](https://nixos.org/download.html)
2. [Enable flakes](https://nixos.wiki/wiki/Flakes)
3. [Install direnv](https://direnv.net/) (nix package name: `direnv`)
  - [Basic package management with nix](https://nixos.org/manual/nix/stable/package-management/basic-package-mgmt.html)
  - For [declarative package management](https://nixos.org/manual/nixpkgs/stable/#sec-declarative-package-management), simply add `direnv` under `environment.systemPackages` in one of:
    - `/etc/nixos/configuration.nix` if you're using nixos
    - `~/.nixpkgs/darwin-configuration.nix` if you're using darwin-nix (on macos)
    - `~/.config/nixpkgs/config.nix` if you're using nix but without nixos or darwin-nix
  - Fish has built-in support for direnv, so you don't need to do any additional configuration for shell integration.
4. [Install nix-direnv](https://github.com/nix-community/nix-direnv) (nix package name: `nix-direnv`). Make sure to follow the instructions in `nix-direnv`'s README for configuring `nix` and `direnv`.
  - Note: darwin-nix _does_ configure it correctly with `pathsToLink`

### Sanity check the project template against your nix configuration

1. Run `nix develop` to build shell tools required to build your project.
2. Ensure `haskell-language-server` is in the path now (`which haskell-language-server`)
3. Run `nix build` to build the project.
4. Run `./result/bin/haskell-template-direnv`, and observe the output `Test`.

### Configure your editor

This section will assume working knowledge of your editor.

#### Spacemacs

Add the following to `dotspacemacs-configuration-layers`
```
     (auto-completion :variables
                      auto-completion-enable-help-tooltip t
                      auto-completion-enable-sort-by-usage t
                      )
     helm ; optional, but you probably want it
     (haskell :variables
              haskell-completion-backend 'lsp
              lsp-haskell-server-path "haskell-language-server"
              lsp-haskell-process-path-hie "haskell-language-server"
              )
     lsp
```

Add the following to `dotspacemacs-additional-packages`:
```
                                      (lsp-haskell :location (recipe :fetcher github :repo "emacs-lsp/lsp-haskell"))
                                      direnv
```

Add the following to `dotspacemacs/user-config`:
```
  (add-hook 'haskell-mode-hook #'direnv-update-environment)
```

Additional resources:
- [hls-tatics-plugin's README has instructions on how to define keybindings for wingman's actions](https://hackage.haskell.org/package/hls-tactics-plugin)
- You may want to set `projectile-indexing-method` to `alien` for haskell projects, see [this emacs.stackexchange post](https://emacs.stackexchange.com/questions/38728/helm-projectile-find-file-doesnt-ignore-some-files)

#### Vanilla Emacs

The [haskell-language-server configuration docs](https://haskell-language-server.readthedocs.io/en/lahaskell-template-direnv/configuration.html#emacs) has resources on how to install for emacs. It links 3 packages that provide editor support. You should make sure to follow the README in each one, and make sure that the LSP server path points to `haskell-language-server` and _not_ `haskell-language-server-wrapper` (which is the default).

#### Vim/Neovim

The [haskell-language-server configuration docs](https://haskell-language-server.readthedocs.io/en/lahaskell-template-direnv/configuration.html#vim-or-neovim) has resources on how to install for emacs. Just make sure the command points to `haskell-language-server` and not `haskell-language-server-wrapper`.

### Verify your editor configuration

Open `Main.hs`. The project root is the git repo root (projectile will ask you if you are using emacs). Try adding something like:
```
main2 :: IO ()
main2 = _
```
Hover over the `_` and make sure you are presented with the corresponding code actions. If LSP is not properly loaded with emacs, you may need to kill the buffer and re-open the file.

### Update the project

Before using it for real,
- Rename all occurrences of `haskell-template-direnv` to `myproject`, and rename the cabal file to `myproject.cabal`. 
- Update `myproject.cabal` with your details.
- Rerun `nix develop` to verify everything continues to work.

## Tips

- Run `nix flake update` to nixpkgs and other flake inputs.

## Alternatives

- [srid/haskell-template](https://github.com/srid/haskell-template) (VSCode-specific)
- haskell.nix: [Getting started with Flakes](https://input-output-hk.github.io/haskell.nix/tutorials/getting-started-flakes/)
- [Serokell's Flake template](https://github.com/serokell/templates/tree/master/haskell-cabal2nix)
  - [Same, but using haskell.nix](https://github.com/serokell/templates/pull/2)
