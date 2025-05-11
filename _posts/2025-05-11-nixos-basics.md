**How to install package in NixOS**

sudo nano /etc/nixos/configuration.nix


sudo nixos-rebuild switch

```
environment.systemPackages = with pkgs; [
  google-chrome
  telegram-desktop
  lens

  vim
  wget
  git
  vscode
  ];
```