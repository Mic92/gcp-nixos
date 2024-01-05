# gcp-nixos

## incremental rebuild

```
nixos-rebuild --target-host tomaskrupka.cz --use-remote-sudo switch -I nixos-config=configuration.nix
```

## new machine setup

Create new machine according to https://nixos.wiki/wiki/Install_NixOS_on_GCE

1. connect to the new instance with Cloud Shell
2. `sudo nano /etc/nixos/configuration.nix`
    1. add: `nix.settings.trusted-users = [ "root" "tom" ];`
    2. `sudo nixos-rebuild switch`
3. Generate sops key for the device
    1. `nix-shell -p ssh-to-age --run 'cat /etc/ssh/ssh_host_ed25519_key.pub | ssh-to-age'`
    2. add the result to `.sops.yaml`
4. Rebuild the secrets file
    1. TODO: improve this:
    2. `sops -d secrets/gcp-instance.yaml > secrets/tmp.yaml`
    3. `sops -e secrets/tmp.yaml > secrets/gcp-instance.yaml`
    4. `rm secrets/tmp.yaml`

# encrypt a file verbatim

```
sops --input-type binary -e secrets/tmp.json > secrets/encrypted.json
```
