![](../../workflows/gds/badge.svg) ![](../../workflows/docs/badge.svg) ![](../../workflows/test/badge.svg) ![](../../workflows/fpga/badge.svg)

# Context-Aware Predictive Battery FDI (Tiny Tapeout)

A small digital ASIC that watches battery voltage, current and two temperatures, predicts trouble a few samples
ahead, tells sensor faults apart from cell faults, and isolates the pack on its own with a fail-safe interlock.

- [Project datasheet and pin map](docs/info.md)

## Features

- **Look-ahead isolation**: value 8 samples ahead = EMA + 8 x slope, shifts only.
- **Load-context residuals**: current explains (or fails to explain) temperature and voltage trends.
- **Fault-signature decoder**: 3-bit first-cause class (V/I/T sensor, cell short, thermal, hard limit, timeout).
- **Graded fail-safe response**: DERATE, then ISOLATE; isolated at reset; interlocked clear; watchdog.

## Repository layout

| Path | Contents |
|------|----------|
| `src/project.v` | Tiny Tapeout top module `tt_um_rvce_battery_fdi` |
| `src/bfdi_sync.v` | input synchronisers and edge detectors |
| `src/bfdi_channels.v` | four-channel EMA / slope / look-ahead datapath |
| `src/bfdi_decoder.v` | residuals, fault-signature decoder, latch, watchdog |
| `model/` | Python reference model, RTL cross-check and false-trip analysis |
| `test/` | cocotb testbench (`make`) and a plain Verilog testbench (`tb_plain.v`) |
| `docs/info.md` | datasheet text used by the Tiny Tapeout website |

## Verification

```sh
cd test && make                 # cocotb RTL tests
python3 model/check_rtl.py      # RTL vs Python model, bit-accurate output comparison
python3 model/analyze.py 200    # context-gated vs ungated: false isolations and detection
```

## Credits

We gratefully acknowledge the Center of Excellence (CoE) in Integrated Circuits and Systems (ICAS) and the
Department of Electronics and Communication Engineering (ECE) for providing the necessary resources and guidance.
(Replace with the full credits paragraph from the submission instructions.)

Built with [Tiny Tapeout](https://tinytapeout.com).
