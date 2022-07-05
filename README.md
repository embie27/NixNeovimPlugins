# nixpkgs-vim-extra-plugins

Nix flake of miscellaneous Vim/Neovim plugins.

## Table of contents

- [Description](#description)
- [Usage](#usage)
    - [In flake](#in-flake)
    - [In legacy Nix](#in-legacy-nix)
    - [Via NUR](#via-nur)
- [Available Vim/Neovim plugins](#available-vimneovim-plugins)
- [Contribution](#contribution)
    - [How to add a new plugin](#how-to-add-a-new-plugin)
- [License](#license)

## Description

This flake contains Nix packages of miscellaneous Vim/Neovim plugins.
Most of them are simply not provided by the official nixpkgs.
Some of them are provided but patched in this flake, or their version/revision
in the official nixpkgs is not up to date for my use case.

Packages are automatically updated twice per week using GitHub Actions.

Moreover, Neovim plugins listed in [awesome-neovim][0] are automatically generated by
parsing the README. Since these packages are automatically generated, some of them could
be broken due to lack of appropriate overrides (missing dependencies, build inputs, etc.).
So you should be careful if you want to use them.

## Usage

### In flake

The overlay simply adds extra Vim plugins into `pkgs.vimExtraPlugins`.
Use it as you normally do, like

```nix
{
  inputs = {
    flake-utils.url = "github:numtide/flake-utils";
    vim-extra-plugins.url = "github:m15a/nixpkgs-vim-extra-plugins";
  };
  outputs = { self, nixpkgs, flake-utils, vim-extra-plugins, ... }:
  flake-utils.lib.eachDefaultSystem (system:
  let
    pkgs = import nixpkgs {
      inherit system;
      overlays = [ vim-extra-plugins.overlay ];
    };
  in {
    packages = {
      my-neovim = pkgs.neovim.override {
        configure = {
          packages.example = with pkgs.vimExtraPlugins; {
            start = [
              lspactions
              vim-hy
            ];
          };
        };
      };
    };
  });
}
```

### In legacy Nix

It is handy to use `builtins.getFlake`, which was [introduced in Nix 2.4][1]. For example,

```nix
with import <nixpkgs> {
  overlays = [
    (builtins.getFlake "github:m15a/nixpkgs-vim-extra-plugins").overlay
  ];
};
```

For Nix <2.4, use `builtins.fetchTarball` instead.

```nix
with import <nixpkgs> {
  overlays = [
    (import (builtins.fetchTarball {
      url = "https://github.com/m15a/nixpkgs-vim-extra-plugins/archive/main.tar.gz";
    })).overlay
  ];
};
```

### Via NUR

You can also use it via [NUR][2] at `nur.repos.m15a.vimExtraPlugins`, see [the package list][3].

[0]: https://github.com/rockerBOO/awesome-neovim
[1]: https://nixos.org/manual/nix/stable/release-notes/rl-2.4.html?highlight=builtins.getFlake#other-features
[2]: https://nur.nix-community.org/
[3]: https://nur.nix-community.org/repos/m15a/

## Available Vim/Neovim plugins

| Plugin owner/repo | Recent commit | Nix package name |
| :- | :- | :- |
| [0styx0/abbreinder.nvim](https://github.com/0styx0/abbreinder.nvim) | 2022-01-15 | `abbreinder-nvim` |
| [AckslD/nvim-revJ.lua](https://github.com/AckslD/nvim-revJ.lua) | 2021-09-12 | `nvim-revJ-lua` |
| [AllenDang/nvim-expand-expr](https://github.com/AllenDang/nvim-expand-expr) | 2021-08-14 | `nvim-expand-expr` |
| [CRAG666/code_runner.nvim](https://github.com/CRAG666/code_runner.nvim) | 2022-03-20 | `code-runner-nvim` |
| [David-Kunz/cmp-npm](https://github.com/David-Kunz/cmp-npm) | 2021-10-27 | `cmp-npm` |
| [David-Kunz/jester](https://github.com/David-Kunz/jester) | 2022-03-22 | `jester` |
| [FrenzyExists/aquarium-vim](https://github.com/FrenzyExists/aquarium-vim) | 2021-12-26 | `aquarium-vim` |
| [Fymyte/rasi.vim](https://github.com/Fymyte/rasi.vim) | 2022-02-16 | `rasi-vim` |
| [Iron-E/nvim-cartographer](https://github.com/Iron-E/nvim-cartographer) | 2022-02-04 | `nvim-cartographer` |
| [JoosepAlviste/nvim-ts-context-commentstring](https://github.com/JoosepAlviste/nvim-ts-context-commentstring) | 2022-03-18 | `nvim-ts-context-commentstring` |
| [L3MON4D3/LuaSnip](https://github.com/L3MON4D3/LuaSnip) | 2022-04-01 | `LuaSnip` |
| [LinArcX/telescope-command-palette.nvim](https://github.com/LinArcX/telescope-command-palette.nvim) | 2022-01-31 | `telescope-command-palette-nvim` |
| [LionC/nest.nvim](https://github.com/LionC/nest.nvim) | 2021-09-26 | `nest-nvim` |
| [LoricAndre/OneTerm.nvim](https://github.com/LoricAndre/OneTerm.nvim) | 2022-03-14 | `OneTerm-nvim` |
| [Everblush/everblush.vim](https://github.com/Everblush/everblush.vim) | 2022-03-27 | `everblush-vim` |
| [mcauley-penney/tidy.nvim](https://github.com/mcauley-penney/tidy.nvim) | 2022-01-08 | `tidy-nvim` |
| [Mofiqul/dracula.nvim](https://github.com/Mofiqul/dracula.nvim) | 2022-01-27 | `dracula-nvim` |
| [Mofiqul/vscode.nvim](https://github.com/Mofiqul/vscode.nvim) | 2022-03-27 | `vscode-nvim` |
| [MunifTanjim/nui.nvim](https://github.com/MunifTanjim/nui.nvim) | 2022-03-08 | `nui-nvim` |
| [NFrid/due.nvim](https://github.com/NFrid/due.nvim) | 2022-03-10 | `due-nvim` |
| [NTBBloodbath/cheovim](https://github.com/NTBBloodbath/cheovim) | 2021-08-25 | `cheovim` |
| [NTBBloodbath/doom-one.nvim](https://github.com/NTBBloodbath/doom-one.nvim) | 2021-12-29 | `doom-one-nvim` |
| [NTBBloodbath/galaxyline.nvim](https://github.com/NTBBloodbath/galaxyline.nvim) | 2022-01-21 | `galaxyline-nvim` |
| [NTBBloodbath/rest.nvim](https://github.com/NTBBloodbath/rest.nvim) | 2022-01-26 | `rest-nvim` |
| [ThemerCorp/themer.lua](https://github.com/ThemerCorp/themer.lua) | 2022-03-12 | `themer-lua` |
| [PHSix/nvim-hybrid](https://github.com/PHSix/nvim-hybrid) | 2022-01-22 | `nvim-hybrid` |
| [Pocco81/AbbrevMan.nvim](https://github.com/Pocco81/AbbrevMan.nvim) | 2021-07-15 | `AbbrevMan-nvim` |
| [Pocco81/AutoSave.nvim](https://github.com/Pocco81/AutoSave.nvim) | 2021-12-14 | `AutoSave-nvim` |
| [Pocco81/DAPInstall.nvim](https://github.com/Pocco81/DAPInstall.nvim) | 2022-01-27 | `DAPInstall-nvim` |
| [Pocco81/HighStr.nvim](https://github.com/Pocco81/HighStr.nvim) | 2021-08-17 | `HighStr-nvim` |
| [RRethy/nvim-treesitter-textsubjects](https://github.com/RRethy/nvim-treesitter-textsubjects) | 2022-04-02 | `nvim-treesitter-textsubjects` |
| [RishabhRD/gruvy](https://github.com/RishabhRD/gruvy) | 2022-01-09 | `gruvy` |
| [RishabhRD/lspactions](https://github.com/RishabhRD/lspactions) | 2022-01-19 | `lspactions` |
| [RishabhRD/lspactions](https://github.com/RishabhRD/lspactions) [branch: `nvim-0.6-compatible`] | 2022-01-08 | `lspactions-nvim06-compatible` |
| [RishabhRD/nvim-rdark](https://github.com/RishabhRD/nvim-rdark) | 2020-12-25 | `nvim-rdark` |
| [Th3Whit3Wolf/onebuddy](https://github.com/Th3Whit3Wolf/onebuddy) | 2021-04-01 | `onebuddy` |
| [Th3Whit3Wolf/space-nvim](https://github.com/Th3Whit3Wolf/space-nvim) | 2021-07-08 | `space-nvim` |
| [ThePrimeagen/vim-be-good](https://github.com/ThePrimeagen/vim-be-good) | 2021-03-16 | `vim-be-good` |
| [TimUntersberger/neofs](https://github.com/TimUntersberger/neofs) | 2021-11-02 | `neofs` |
| [Xuyuanp/yanil](https://github.com/Xuyuanp/yanil) | 2022-03-28 | `yanil` |
| [abecodes/tabout.nvim](https://github.com/abecodes/tabout.nvim) | 2022-03-19 | `tabout-nvim` |
| [adelarsq/neoline.vim](https://github.com/adelarsq/neoline.vim) | 2022-04-02 | `neoline-vim` |
| [adisen99/apprentice.nvim](https://github.com/adisen99/apprentice.nvim) | 2021-12-12 | `apprentice-nvim` |
| [adisen99/codeschool.nvim](https://github.com/adisen99/codeschool.nvim) | 2021-12-12 | `codeschool-nvim` |
| [akinsho/dependency-assist.nvim](https://github.com/akinsho/dependency-assist.nvim) | 2021-11-11 | `dependency-assist-nvim` |
| [akinsho/flutter-tools.nvim](https://github.com/akinsho/flutter-tools.nvim) | 2022-03-27 | `flutter-tools-nvim` |
| [akinsho/toggleterm.nvim](https://github.com/akinsho/toggleterm.nvim) | 2022-04-02 | `toggleterm-nvim` |
| [alaviss/nim.nvim](https://github.com/alaviss/nim.nvim) | 2021-10-07 | `nim-nvim` |
| [alec-gibson/nvim-tetris](https://github.com/alec-gibson/nvim-tetris) | 2021-06-28 | `nvim-tetris` |
| [alexaandru/nvim-lspupdate](https://github.com/alexaandru/nvim-lspupdate) | 2021-12-21 | `nvim-lspupdate` |
| [aliou/bats.vim](https://github.com/aliou/bats.vim) | 2021-01-10 | `bats-vim` |
| [amirrezaask/fuzzy.nvim](https://github.com/amirrezaask/fuzzy.nvim) | 2021-05-13 | `fuzzy-nvim` |
| [andersevenrud/nordic.nvim](https://github.com/andersevenrud/nordic.nvim) | 2022-03-13 | `nordic-nvim` |
| [anott03/nvim-lspinstall](https://github.com/anott03/nvim-lspinstall) | 2021-07-23 | `nvim-lspinstall` |
| [anuvyklack/pretty-fold.nvim](https://github.com/anuvyklack/pretty-fold.nvim) | 2022-04-01 | `pretty-fold-nvim` |
| [aserowy/tmux.nvim](https://github.com/aserowy/tmux.nvim) | 2022-03-28 | `tmux-nvim` |
| [b0o/mapx.nvim](https://github.com/b0o/mapx.nvim) | 2022-02-24 | `mapx-nvim` |
| [beauwilliams/focus.nvim](https://github.com/beauwilliams/focus.nvim) | 2022-03-09 | `focus-nvim` |
| [beauwilliams/statusline.lua](https://github.com/beauwilliams/statusline.lua) | 2022-03-20 | `statusline-lua` |
| [bfredl/nvim-luadev](https://github.com/bfredl/nvim-luadev) | 2022-01-26 | `nvim-luadev` |
| [bkegley/gloombuddy](https://github.com/bkegley/gloombuddy) | 2021-04-16 | `gloombuddy` |
| [bluz71/vim-moonfly-colors](https://github.com/bluz71/vim-moonfly-colors) | 2022-03-30 | `vim-moonfly-colors` |
| [bluz71/vim-nightfly-guicolors](https://github.com/bluz71/vim-nightfly-guicolors) | 2022-03-30 | `vim-nightfly-guicolors` |
| [booperlv/nvim-gomove](https://github.com/booperlv/nvim-gomove) | 2022-04-02 | `nvim-gomove` |
| [catppuccin/nvim](https://github.com/catppuccin/nvim) | 2022-03-20 | `catppuccin` |
| [chipsenkbeil/distant.nvim](https://github.com/chipsenkbeil/distant.nvim) | 2021-11-13 | `distant-nvim` |
| [clojure-vim/jazz.nvim](https://github.com/clojure-vim/jazz.nvim) | 2019-04-30 | `jazz-nvim` |
| [code-biscuits/nvim-biscuits](https://github.com/code-biscuits/nvim-biscuits) | 2021-11-12 | `nvim-biscuits` |
| [crispgm/nvim-go](https://github.com/crispgm/nvim-go) | 2022-01-28 | `nvim-go` |
| [crispgm/nvim-tabline](https://github.com/crispgm/nvim-tabline) | 2022-02-21 | `nvim-tabline` |
| [crispgm/telescope-heading.nvim](https://github.com/crispgm/telescope-heading.nvim) | 2022-02-18 | `telescope-heading-nvim` |
| [danielpieper/telescope-tmuxinator.nvim](https://github.com/danielpieper/telescope-tmuxinator.nvim) | 2021-08-19 | `telescope-tmuxinator-nvim` |
| [danymat/neogen](https://github.com/danymat/neogen) | 2022-04-01 | `neogen` |
| [datwaft/bubbly.nvim](https://github.com/datwaft/bubbly.nvim) | 2021-11-15 | `bubbly-nvim` |
| [davidgranstrom/nvim-markdown-preview](https://github.com/davidgranstrom/nvim-markdown-preview) | 2022-02-07 | `nvim-markdown-preview` |
| [davidgranstrom/osc.nvim](https://github.com/davidgranstrom/osc.nvim) | 2021-08-02 | `osc-nvim` |
| [davidgranstrom/scnvim](https://github.com/davidgranstrom/scnvim) | 2022-03-31 | `scnvim` |
| [dcampos/nvim-snippy](https://github.com/dcampos/nvim-snippy) | 2022-03-27 | `nvim-snippy` |
| [dkarter/bullets.vim](https://github.com/dkarter/bullets.vim) | 2022-01-30 | `bullets-vim` |
| [edolphin-ydf/goimpl.nvim](https://github.com/edolphin-ydf/goimpl.nvim) | 2021-12-07 | `goimpl-nvim` |
| [ekickx/clipboard-image.nvim](https://github.com/ekickx/clipboard-image.nvim) | 2022-03-15 | `clipboard-image-nvim` |
| [ellisonleao/glow.nvim](https://github.com/ellisonleao/glow.nvim) | 2022-03-10 | `glow-nvim` |
| [ethanholz/nvim-lastplace](https://github.com/ethanholz/nvim-lastplace) | 2021-10-15 | `nvim-lastplace` |
| [feline-nvim/feline.nvim](https://github.com/feline-nvim/feline.nvim) | 2022-03-24 | `feline-nvim` |
| [folke/zen-mode.nvim](https://github.com/folke/zen-mode.nvim) | 2021-11-07 | `zen-mode-nvim` |
| [fuenor/JpFormat.vim](https://github.com/fuenor/JpFormat.vim) | 2019-07-12 | `JpFormat-vim` |
| [gbprod/cutlass.nvim](https://github.com/gbprod/cutlass.nvim) | 2022-03-17 | `cutlass-nvim` |
| [gbprod/substitute.nvim](https://github.com/gbprod/substitute.nvim) | 2022-03-22 | `substitute-nvim` |
| [gennaro-tedesco/nvim-commaround](https://github.com/gennaro-tedesco/nvim-commaround) | 2022-01-14 | `nvim-commaround` |
| [glacambre/firenvim](https://github.com/glacambre/firenvim) | 2022-02-25 | `firenvim` |
| [glepnir/indent-guides.nvim](https://github.com/glepnir/indent-guides.nvim) | 2021-03-26 | `indent-guides-nvim` |
| [glepnir/prodoc.nvim](https://github.com/glepnir/prodoc.nvim) | 2021-01-23 | `prodoc-nvim` |
| [goolord/alpha-nvim](https://github.com/goolord/alpha-nvim) | 2022-04-02 | `alpha-nvim` |
| [h-hg/fcitx.nvim](https://github.com/h-hg/fcitx.nvim) | 2021-12-11 | `fcitx-nvim` |
| [henriquehbr/ataraxis.lua](https://github.com/henriquehbr/ataraxis.lua) | 2021-10-24 | `ataraxis-lua` |
| [henriquehbr/nvim-startup.lua](https://github.com/henriquehbr/nvim-startup.lua) | 2022-02-09 | `nvim-startup-lua` |
| [hkupty/nvimux](https://github.com/hkupty/nvimux) | 2022-02-28 | `nvimux` |
| [houtsnip/vim-emacscommandline](https://github.com/houtsnip/vim-emacscommandline) | 2017-11-24 | `vim-emacscommandline` |
| [hylang/vim-hy](https://github.com/hylang/vim-hy) | 2022-03-09 | `vim-hy` |
| [ibhagwan/fzf-lua](https://github.com/ibhagwan/fzf-lua) | 2022-04-02 | `fzf-lua` |
| [inkch/vim-fish](https://github.com/inkch/vim-fish) | 2022-03-06 | `vim-fish-inkch` |
| [is0n/fm-nvim](https://github.com/is0n/fm-nvim) | 2022-03-24 | `fm-nvim` |
| [is0n/jaq-nvim](https://github.com/is0n/jaq-nvim) | 2022-03-24 | `jaq-nvim` |
| [ishan9299/modus-theme-vim](https://github.com/ishan9299/modus-theme-vim) | 2022-02-15 | `modus-theme-vim` |
| [jakewvincent/mkdnflow.nvim](https://github.com/jakewvincent/mkdnflow.nvim) | 2022-04-01 | `mkdnflow-nvim` |
| [jakewvincent/texmagic.nvim](https://github.com/jakewvincent/texmagic.nvim) | 2021-09-01 | `texmagic-nvim` |
| [jameshiew/nvim-magic](https://github.com/jameshiew/nvim-magic) | 2022-02-03 | `nvim-magic` |
| [jamestthompson3/nvim-remote-containers](https://github.com/jamestthompson3/nvim-remote-containers) | 2022-03-07 | `nvim-remote-containers` |
| [jbyuki/instant.nvim](https://github.com/jbyuki/instant.nvim) | 2021-10-26 | `instant-nvim` |
| [jbyuki/nabla.nvim](https://github.com/jbyuki/nabla.nvim) | 2022-04-03 | `nabla-nvim` |
| [jbyuki/one-small-step-for-vimkind](https://github.com/jbyuki/one-small-step-for-vimkind) | 2021-10-26 | `one-small-step-for-vimkind` |
| [jghauser/auto-pandoc.nvim](https://github.com/jghauser/auto-pandoc.nvim) | 2022-03-12 | `auto-pandoc-nvim` |
| [jghauser/follow-md-links.nvim](https://github.com/jghauser/follow-md-links.nvim) | 2022-04-03 | `follow-md-links-nvim` |
| [jghauser/kitty-runner.nvim](https://github.com/jghauser/kitty-runner.nvim) | 2022-03-13 | `kitty-runner-nvim` |
| [jghauser/mkdir.nvim](https://github.com/jghauser/mkdir.nvim) | 2022-03-12 | `mkdir-nvim` |
| [jim-at-jibba/ariake-vim-colors](https://github.com/jim-at-jibba/ariake-vim-colors) | 2021-02-23 | `ariake-vim-colors` |
| [johann2357/nvim-smartbufs](https://github.com/johann2357/nvim-smartbufs) | 2021-06-14 | `nvim-smartbufs` |
| [jose-elias-alvarez/null-ls.nvim](https://github.com/jose-elias-alvarez/null-ls.nvim) | 2022-04-02 | `null-ls-nvim` |
| [jubnzv/mdeval.nvim](https://github.com/jubnzv/mdeval.nvim) | 2021-10-02 | `mdeval-nvim` |
| [jubnzv/virtual-types.nvim](https://github.com/jubnzv/virtual-types.nvim) | 2022-03-17 | `virtual-types-nvim` |
| [jvgrootveld/telescope-zoxide](https://github.com/jvgrootveld/telescope-zoxide) | 2021-10-21 | `telescope-zoxide` |
| [kana/vim-textobj-indent](https://github.com/kana/vim-textobj-indent) | 2013-01-18 | `vim-textobj-indent` |
| [kazhala/close-buffers.nvim](https://github.com/kazhala/close-buffers.nvim) | 2021-11-14 | `close-buffers-nvim` |
| [kdheepak/monochrome.nvim](https://github.com/kdheepak/monochrome.nvim) | 2021-07-14 | `monochrome-nvim` |
| [klen/nvim-config-local](https://github.com/klen/nvim-config-local) | 2022-03-26 | `nvim-config-local` |
| [konapun/vacuumline.nvim](https://github.com/konapun/vacuumline.nvim) | 2022-03-13 | `vacuumline-nvim` |
| [nvim-orgmode/orgmode](https://github.com/nvim-orgmode/orgmode) | 2022-04-03 | `orgmode` |
| [kvrohit/substrata.nvim](https://github.com/kvrohit/substrata.nvim) | 2022-03-28 | `substrata-nvim` |
| [kyazdani42/blue-moon](https://github.com/kyazdani42/blue-moon) | 2022-02-12 | `blue-moon` |
| [ldelossa/vimdark](https://github.com/ldelossa/vimdark) | 2022-03-20 | `vimdark` |
| [lewis6991/spellsitter.nvim](https://github.com/lewis6991/spellsitter.nvim) | 2022-03-29 | `spellsitter-nvim` |
| [lourenci/github-colors](https://github.com/lourenci/github-colors) | 2022-03-06 | `github-colors` |
| [luisiacc/gruvbox-baby](https://github.com/luisiacc/gruvbox-baby) | 2022-03-15 | `gruvbox-baby` |
| [lukas-reineke/cmp-rg](https://github.com/lukas-reineke/cmp-rg) | 2022-01-13 | `cmp-rg` |
| [luukvbaal/nnn.nvim](https://github.com/luukvbaal/nnn.nvim) | 2022-03-11 | `nnn-nvim` |
| [m00qek/plugin-template.nvim](https://github.com/m00qek/plugin-template.nvim) | 2022-01-05 | `plugin-template-nvim` |
| [madskjeldgaard/reaper-nvim](https://github.com/madskjeldgaard/reaper-nvim) | 2021-01-29 | `reaper-nvim` |
| [marko-cerovac/material.nvim](https://github.com/marko-cerovac/material.nvim) | 2022-04-01 | `material-nvim` |
| [matbme/JABS.nvim](https://github.com/matbme/JABS.nvim) | 2021-09-22 | `JABS-nvim` |
| [mcchrish/zenbones.nvim](https://github.com/mcchrish/zenbones.nvim) | 2022-02-14 | `zenbones-nvim` |
| [mfussenegger/nvim-treehopper](https://github.com/mfussenegger/nvim-treehopper) | 2021-12-20 | `nvim-treehopper` |
| [michaelb/sniprun](https://github.com/michaelb/sniprun) | 2022-03-16 | `sniprun` |
| [mizlan/iswap.nvim](https://github.com/mizlan/iswap.nvim) | 2022-03-20 | `iswap-nvim` |
| [mjlbach/babelfish.nvim](https://github.com/mjlbach/babelfish.nvim) | 2021-09-14 | `babelfish-nvim` |
| [mnacamura/iron.nvim](https://github.com/mnacamura/iron.nvim) | 2021-12-19 | `iron-nvim-mnacamura` |
| [mnacamura/nvim-srcerite](https://github.com/mnacamura/nvim-srcerite) | 2021-12-17 | `nvim-srcerite` |
| [mnacamura/vim-fennel-syntax](https://github.com/mnacamura/vim-fennel-syntax) | 2021-07-08 | `vim-fennel-syntax` |
| [mnacamura/vim-r7rs-syntax](https://github.com/mnacamura/vim-r7rs-syntax) | 2021-07-09 | `vim-r7rs-syntax` |
| [monaqa/dial.nvim](https://github.com/monaqa/dial.nvim) | 2022-03-28 | `dial-nvim` |
| [monkoose/matchparen.nvim](https://github.com/monkoose/matchparen.nvim) | 2022-03-28 | `matchparen-nvim` |
| [ms-jpq/coq_nvim](https://github.com/ms-jpq/coq_nvim) | 2022-04-03 | `coq-nvim` |
| [nanotee/nvim-lsp-basics](https://github.com/nanotee/nvim-lsp-basics) | 2021-05-16 | `nvim-lsp-basics` |
| [nanotee/sqls.nvim](https://github.com/nanotee/sqls.nvim) | 2022-02-21 | `sqls-nvim` |
| [nekonako/xresources-nvim](https://github.com/nekonako/xresources-nvim) | 2021-11-23 | `xresources-nvim` |
| [nikvdp/neomux](https://github.com/nikvdp/neomux) | 2021-12-23 | `neomux` |
| [noib3/nvim-cokeline](https://github.com/noib3/nvim-cokeline) | 2022-03-15 | `nvim-cokeline` |
| [norcalli/nvim-base16.lua](https://github.com/norcalli/nvim-base16.lua) | 2019-10-16 | `nvim-base16-lua` |
| [notomo/cmdbuf.nvim](https://github.com/notomo/cmdbuf.nvim) | 2022-03-28 | `cmdbuf-nvim` |
| [notomo/gesture.nvim](https://github.com/notomo/gesture.nvim) | 2022-03-28 | `gesture-nvim` |
| [novakne/kosmikoa.nvim](https://github.com/novakne/kosmikoa.nvim) | 2021-11-19 | `kosmikoa-nvim` |
| [numToStr/Comment.nvim](https://github.com/numToStr/Comment.nvim) | 2022-04-03 | `Comment-nvim` |
| [numToStr/Navigator.nvim](https://github.com/numToStr/Navigator.nvim) | 2022-03-28 | `Navigator-nvim` |
| [nvim-telescope/telescope-bibtex.nvim](https://github.com/nvim-telescope/telescope-bibtex.nvim) | 2022-03-16 | `telescope-bibtex-nvim` |
| [nxvu699134/vn-night.nvim](https://github.com/nxvu699134/vn-night.nvim) | 2021-07-24 | `vn-night-nvim` |
| [nyngwang/NeoRoot.lua](https://github.com/nyngwang/NeoRoot.lua) | 2022-01-24 | `NeoRoot-lua` |
| [ojroques/nvim-hardline](https://github.com/ojroques/nvim-hardline) | 2022-03-08 | `nvim-hardline` |
| [ojroques/nvim-lspfuzzy](https://github.com/ojroques/nvim-lspfuzzy) | 2022-01-16 | `nvim-lspfuzzy` |
| [p00f/cphelper.nvim](https://github.com/p00f/cphelper.nvim) | 2022-04-03 | `cphelper-nvim` |
| [petertriho/cmp-git](https://github.com/petertriho/cmp-git) | 2022-03-26 | `cmp-git` |
| [petertriho/nvim-scrollbar](https://github.com/petertriho/nvim-scrollbar) | 2022-04-03 | `nvim-scrollbar` |
| [pianocomposer321/consolation.nvim](https://github.com/pianocomposer321/consolation.nvim) | 2021-09-01 | `consolation-nvim` |
| [pianocomposer321/yabs.nvim](https://github.com/pianocomposer321/yabs.nvim) | 2021-12-11 | `yabs-nvim` |
| [projekt0n/github-nvim-theme](https://github.com/projekt0n/github-nvim-theme) | 2022-04-01 | `github-nvim-theme` |
| [pwntester/codeql.nvim](https://github.com/pwntester/codeql.nvim) | 2022-03-28 | `codeql-nvim` |
| [pwntester/octo.nvim](https://github.com/pwntester/octo.nvim) | 2022-04-01 | `octo-nvim` |
| [quangnguyen30192/cmp-nvim-ultisnips](https://github.com/quangnguyen30192/cmp-nvim-ultisnips) | 2022-03-19 | `cmp-nvim-ultisnips` |
| [rafaelsq/nvim-goc.lua](https://github.com/rafaelsq/nvim-goc.lua) | 2022-01-20 | `nvim-goc-lua` |
| [rafcamlet/nvim-luapad](https://github.com/rafcamlet/nvim-luapad) | 2021-12-29 | `nvim-luapad` |
| [rafcamlet/tabline-framework.nvim](https://github.com/rafcamlet/tabline-framework.nvim) | 2022-03-09 | `tabline-framework-nvim` |
| [raimon49/requirements.txt.vim](https://github.com/raimon49/requirements.txt.vim) | 2022-03-30 | `requirements-txt-vim` |
| [ray-x/go.nvim](https://github.com/ray-x/go.nvim) | 2022-03-30 | `go-nvim` |
| [ray-x/guihua.lua](https://github.com/ray-x/guihua.lua) | 2022-03-06 | `guihua-lua` |
| [ray-x/navigator.lua](https://github.com/ray-x/navigator.lua) | 2022-03-16 | `navigator-lua` |
| [renerocksai/telekasten.nvim](https://github.com/renerocksai/telekasten.nvim) | 2022-01-24 | `telekasten-nvim` |
| [rhysd/vim-gfm-syntax](https://github.com/rhysd/vim-gfm-syntax) | 2021-10-04 | `vim-gfm-syntax` |
| [rktjmp/highlight-current-n.nvim](https://github.com/rktjmp/highlight-current-n.nvim) | 2022-03-22 | `highlight-current-n-nvim` |
| [rktjmp/hotpot.nvim](https://github.com/rktjmp/hotpot.nvim) | 2022-03-23 | `hotpot-nvim` |
| [rktjmp/paperplanes.nvim](https://github.com/rktjmp/paperplanes.nvim) | 2022-02-05 | `paperplanes-nvim` |
| [rlane/pounce.nvim](https://github.com/rlane/pounce.nvim) | 2022-01-26 | `pounce-nvim` |
| [rmehri01/onenord.nvim](https://github.com/rmehri01/onenord.nvim) | 2022-01-08 | `onenord-nvim` |
| [rockerBOO/boo-colorscheme-nvim](https://github.com/rockerBOO/boo-colorscheme-nvim) | 2022-03-26 | `boo-colorscheme-nvim` |
| [rose-pine/neovim](https://github.com/rose-pine/neovim) | 2022-04-01 | `rose-pine` |
| [s1n7ax/nvim-comment-frame](https://github.com/s1n7ax/nvim-comment-frame) | 2021-09-14 | `nvim-comment-frame` |
| [s1n7ax/nvim-terminal](https://github.com/s1n7ax/nvim-terminal) | 2021-11-12 | `nvim-terminal` |
| [sQVe/sort.nvim](https://github.com/sQVe/sort.nvim) | 2022-01-03 | `sort-nvim` |
| [saifulapm/chartoggle.nvim](https://github.com/saifulapm/chartoggle.nvim) | 2021-10-25 | `chartoggle-nvim` |
| [sainnhe/everforest](https://github.com/sainnhe/everforest) | 2022-03-21 | `everforest` |
| [savq/melange](https://github.com/savq/melange) | 2022-02-21 | `melange` |
| [savq/paq-nvim](https://github.com/savq/paq-nvim) | 2021-12-27 | `paq-nvim` |
| [seandewar/nvimesweeper](https://github.com/seandewar/nvimesweeper) | 2021-09-07 | `nvimesweeper` |
| [sgur/vim-textobj-parameter](https://github.com/sgur/vim-textobj-parameter) | 2017-05-16 | `vim-textobj-parameter` |
| [shaeinst/roshnivim-cs](https://github.com/shaeinst/roshnivim-cs) | 2022-02-16 | `roshnivim-cs` |
| [sindrets/winshift.nvim](https://github.com/sindrets/winshift.nvim) | 2021-11-15 | `winshift-nvim` |
| [skanehira/christmas.vim](https://github.com/skanehira/christmas.vim) | 2021-12-24 | `christmas-vim` |
| [startup-nvim/startup.nvim](https://github.com/startup-nvim/startup.nvim) | 2022-03-28 | `startup-nvim` |
| [stevearc/dressing.nvim](https://github.com/stevearc/dressing.nvim) | 2022-03-31 | `dressing-nvim` |
| [stevearc/gkeep.nvim](https://github.com/stevearc/gkeep.nvim) | 2022-03-30 | `gkeep-nvim` |
| [stevearc/qf_helper.nvim](https://github.com/stevearc/qf_helper.nvim) | 2022-01-28 | `qf-helper-nvim` |
| [svermeulen/vim-yoink](https://github.com/svermeulen/vim-yoink) | 2021-09-15 | `vim-yoink` |
| [svermeulen/vimpeccable](https://github.com/svermeulen/vimpeccable) | 2021-12-28 | `vimpeccable` |
| [tamago324/nlsp-settings.nvim](https://github.com/tamago324/nlsp-settings.nvim) | 2022-04-03 | `nlsp-settings-nvim` |
| [kkharji/lspsaga.nvim](https://github.com/kkharji/lspsaga.nvim) | 2022-03-14 | `lspsaga-nvim` |
| [tamton-aquib/staline.nvim](https://github.com/tamton-aquib/staline.nvim) | 2022-03-01 | `staline-nvim` |
| [tanvirtin/monokai.nvim](https://github.com/tanvirtin/monokai.nvim) | 2022-01-29 | `monokai-nvim` |
| [tanvirtin/vgit.nvim](https://github.com/tanvirtin/vgit.nvim) | 2022-02-04 | `vgit-nvim` |
| [terrortylor/nvim-comment](https://github.com/terrortylor/nvim-comment) | 2022-03-15 | `nvim-comment` |
| [theniceboy/nvim-deus](https://github.com/theniceboy/nvim-deus) | 2021-08-26 | `nvim-deus` |
| [tiagovla/tokyodark.nvim](https://github.com/tiagovla/tokyodark.nvim) | 2022-03-11 | `tokyodark-nvim` |
| [titanzero/zephyrium](https://github.com/titanzero/zephyrium) | 2022-02-20 | `zephyrium` |
| [tjdevries/astronauta.nvim](https://github.com/tjdevries/astronauta.nvim) [branch: `edc19d30a3c51a8c3fc3f606008e5b4238821f1e`] | 2021-11-09 | `astronauta-nvim` |
| [tjdevries/express_line.nvim](https://github.com/tjdevries/express_line.nvim) | 2021-12-01 | `express-line-nvim` |
| [tjdevries/gruvbuddy.nvim](https://github.com/tjdevries/gruvbuddy.nvim) | 2021-04-15 | `gruvbuddy-nvim` |
| [tjdevries/vlog.nvim](https://github.com/tjdevries/vlog.nvim) | 2020-08-04 | `vlog-nvim` |
| [tveskag/nvim-blame-line](https://github.com/tveskag/nvim-blame-line) | 2021-07-27 | `nvim-blame-line` |
| [nvim-neorg/neorg](https://github.com/nvim-neorg/neorg) | 2022-04-02 | `neorg` |
| [williamboman/nvim-lsp-installer](https://github.com/williamboman/nvim-lsp-installer) | 2022-04-03 | `nvim-lsp-installer` |
| [windwp/nvim-projectconfig](https://github.com/windwp/nvim-projectconfig) | 2021-11-10 | `nvim-projectconfig` |
| [nvim-pack/nvim-spectre](https://github.com/nvim-pack/nvim-spectre) | 2022-03-28 | `nvim-spectre` |
| [windwp/windline.nvim](https://github.com/windwp/windline.nvim) | 2022-03-19 | `windline-nvim` |
| [winston0410/commented.nvim](https://github.com/winston0410/commented.nvim) | 2022-03-12 | `commented-nvim` |
| [xeluxee/competitest.nvim](https://github.com/xeluxee/competitest.nvim) | 2022-02-13 | `competitest-nvim` |
| [xiyaowong/nvim-cursorword](https://github.com/xiyaowong/nvim-cursorword) | 2022-03-11 | `nvim-cursorword` |
| [xiyaowong/nvim-transparent](https://github.com/xiyaowong/nvim-transparent) | 2022-03-17 | `nvim-transparent` |
| [yashguptaz/calvera-dark.nvim](https://github.com/yashguptaz/calvera-dark.nvim) | 2021-08-13 | `calvera-dark-nvim` |
| [yonlu/omni.vim](https://github.com/yonlu/omni.vim) | 2021-02-20 | `omni-vim` |
| [gitlab:yorickpeterse/nvim-pqf](https://gitlab.com/yorickpeterse/nvim-pqf) | 2021-10-29 | `nvim-pqf` |
| [gitlab:yorickpeterse/nvim-window](https://gitlab.com/yorickpeterse/nvim-window) | 2022-03-23 | `nvim-window` |

## Contribution

### How to add a new plugin

#### 1. Add a new entry to `manifest.txt`

All Nix derivations of plugins found in `pkgs/vim-plugins.nix` are generated
from `manifest.txt`, in which each line corresponds to each plugin.
An entry is specified by

```bnf
<entry> ::= [ <repo-type>  ":" ] <repo-full-name> [ ":"  [ <git-ref> ] [ ":" <attr-name> ] ]

<repo-type> ::= "github" | "gitlab"

<repo-full-name> ::= <owner-name> "/" <repo-name>
```

- If `<repo-type>` is omitted, it defaults to GitHub.
  Only GitHub and GitLab are supported.
- `<git-ref>` can be either branch name or commit hash. If omitted,
  the latest commit hash in the default branch will be used.
- Attribute name of a plugin (`pkgs.vimExtraPlugins.${attr-name}`) is
  automatically determined from `<repo-name>` by default.
  If `<attr-name>` is set in an entry, it will replace the default name.

**Examples:**

- `foo/bar`: a GitHub repo `bar` of owner `foo`, using default branch.
- `gitlab:foo/bar`:  a GitLab repo, using default branch.
- `foo/bar:dev`: a GitHub repo, using `dev` branch.
- `foo/bar:97be0965f9a0944629ba67e5fd0b05b898d34e61`: a GitHub repo,
  pinned to a commit `97be0965f9a0944629ba67e5fd0b05b898d34e61`.
- `foo/bar::baz`: a GitHub repo, using default branch, renamed to `baz`.

After adding your entry, run:

```
nix run .#update-vim-plugins -- lint
```

So that entries are checked and formatted.

#### 2. Update Nix expression and README

Next, run this:

```
nix run .#update-vim-plugins
```

After that, `pkgs/vim-plugins.nix` and the plugin list in `README.md` are updated.

#### 3. Override your plugin derivation in `overrides.nix`

In `overrides.nix`, you see something like

```nix
  {
    # ...

    lspactions = super.lspactions.overrideAttrs (_: {
      dependencies = with final.vimPlugins; [
        plenary-nvim
        popup-nvim
        self.astronauta-nvim
      ];
    });

    # ...
  }
```

Add your overrides here if needed.

#### 4. Create a Pull Request

Anyone is welcome to add another plugin to this repo.
Feel free to create a PR with your new plugins!
In that case, make sure you commit
`manifest.txt`, `pkgs/vim-plugins.nix`, and optionally `overrides.nix` if changed.
`README.md` will be updated by GitHub Action so it is not mandatory.

## License

[MIT](LICENSE)
