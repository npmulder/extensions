# xe

This system extension provides Intel Xe GPU driver for discrete Intel Arc GPUs including Battlemage (B-series) graphics cards.

## Installation

See [Installing Extensions](https://github.com/siderolabs/extensions#installing-extensions).

## Usage

Enable the `xe` module in Talos machine config. For Arc B570/B580 GPUs, you need to add the `force_probe` parameter:

```yaml
machine:
  kernel:
    modules:
      - name: xe
        parameters:
          - force_probe=e20b  # For Arc B570
          # - force_probe=e20c  # For Arc B580
```

## Verification

You can verify the module is loaded by reading `/proc/modules`:

```bash
talosctl -n <node-ip> read /proc/modules | grep xe
```

Check that your GPU is detected:

```bash
talosctl -n <node-ip> read /proc/driver/dri/0/name
```

You should see the xe driver in use. You can also check DRM devices:

```bash
talosctl -n <node-ip> ls /dev/dri/
```

To verify GPU detection in dmesg:

```bash
talosctl -n <node-ip> dmesg | grep -i "xe\|arc\|battlemage"
```