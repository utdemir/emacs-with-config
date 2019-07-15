# emacs-with-config

This function will give you an Emacs derivation which loads the given
script alongside with all the packages mentioned in it (`doom-themes`,
`undo-tree` and `magit` in this case):

```
let
pkgs = import <nixpkgs> {};
emacsWithConfig = pkgs.callPackage 
  (builtins.fetchTarball "https://github.com/utdemir/emacs-with-config/archive/master.tar.gz")
  {};
in
emacsWithConfig ''
  (require 'use-package)

  (use-package doom-themes
    :config
    (load-theme 'doom-molokai t))

  (use-package undo-tree
    :defer 2
    :init
    (setq undo-tree-visualizer-timestamps t)
    :config
    (global-undo-tree-mode))

  (use-package magit
    :bind
    ("C-x m" . magit-status))
''
```

To be more precise, it will:

1. Parse the given script to find the `use-package` usages and collect
the required packages.
2. Use `pkgs.emacsWithPackages` to get an emacs distribution with 
the required packages.
3. Bytecompile the given script.
4. Wrap `pkgs.emacs` to load the given script by default.
