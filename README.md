# truenas-ryzenadj-build

Script to build ryzen_smu kernel drivers and ryzenadj userspace binaries using Docker on TrueNAS SCALE.

RyzenAdj: CPU power parameters utility (https://github.com/FlyGoat/RyzenAdj)  
ryzen_smu: Kernel module dependency (https://github.com/amkillam/ryzen_smu)

## Usage

```bash
sudo bash docker_build_ryzenadj.sh
```

Output:  
- `/mnt/apps/home/drivers/ryzen_smu.ko` (loaded)  
- `/mnt/apps/home/scripts/ryzenadj` (executable)

## Prerequisites

- TrueNAS SCALE 24.10+ with Docker support
- Kernel headers: `linux-headers-truenas-production-amd64`
- AMD Ryzen CPU (Zen 2+), `/dev/mem` or `/sys` access

## Configuration

Set environment variables before running:

- `BUILD_DIR=/tmp/custom`  # Build directory (default: `/tmp/ryzenadj_build`)
- `DRIVERS_DIR=/path`      # Module storage (default: `/mnt/apps/home/drivers`)
- `SCRIPTS_DIR=/path`      # Binary storage (default: `/mnt/apps/home/scripts`)
- `CLEANUP=1`              # Remove build dir on exit

The two most likely settings to be changed are the script and driver destination folders (`DRIVERS_DIR` and `SCRIPTS_DIR`).

## Notes

- ryzen_smu auto-loads and verifies version (0.1.x compatibility)
- Both artifacts installed to persistent TrueNAS 26 paths
- For reboot persistence: add `insmod /mnt/apps/home/drivers/ryzen_smu.ko` in System Settings -> Advanced -> Init/Shutdown Scripts -> Post Init
- Troubleshooting: `dmesg | grep -i ryzen_smu`
- TrueNAS 26 BETA: May enforce Secure Boot (disable or sign module)
- No warranty provided; review before running.
- Tested and working with TrueNAS 26.0.0-BETA.1
