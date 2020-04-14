# emacs-with-config

**Deprecated**: Much better & maintained version is here: https://github.com/nix-community/emacs-overlay

This function will give you an Emacs derivation which loads the given
script alongside with all the packages mentioned in it via `use-package`.

* default.nix:

```
let
pkgs = import <nixpkgs> {};
emacsWithConfig = pkgs.callPackage
  (builtins.fetchTarball "https://github.com/utdemir/emacs-with-config/archive/master.tar.gz")
  {};
in
emacsWithConfig ./emacs.el
```

* emacs.el:

```
(require 'use-package)

(use-package undo-tree
  :defer 2
  :init
  (setq undo-tree-visualizer-timestamps t)
  :config
  (global-undo-tree-mode))

(use-package magit
  :bind
  ("C-x m" . magit-status))
```

To be more precise, it will:

1. Parse the given script to find the `use-package` usages and collect
the required packages.
2. Use `pkgs.emacsWithPackages` to get an emacs distribution with
the required packages.
3. Bytecompile the given script.
4. Wrap `pkgs.emacs` to load the given script by default.
