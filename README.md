# Gaussian CLI utilities

[![Issues](https://img.shields.io/github/issues-raw/SebastianWellig/gaussian-cli-utilities.svg?label=Issues)](https://github.com/SebastianWellig/gaussian-cli-utilities/issues)
[![License](https://img.shields.io/badge/License-GPLv2-orange.svg)](https://github.com/SebastianWellig/gaussian-cli-utilities/blob/master/LICENSE)

Collection of my personal CLI commands for manipulating and inspecting Gaussian files.

## Requirements

- Python 3.8 or newer.
- [Open Babel](https://github.com/openbabel/openbabel) with Python bindings.

## Installation

All commands are standalone files and inside the [bin](bin/) directory of this repository. You can either copy them or add the directory to your `PATH` env variable.

## Commands

Available commands:
- [`gmod`](#gmod) - Distort coordinates by a normal mode.
- [`replcoords`](#replcoords) - Replace input file coordinates.
- [`maxdispl`](#maxdispl) - Print sorted maximum displacements of geometry optimization.
- [`tseigs`](#tseigs) - Print eigenvectors of transition state optimization.

### `gmod`

Distorts coordinates of log file with a vibrational analysis by the i'th normal mode.

| Option | Description | Default |
| --- | --- | --- |
| `-m`<br />`--mode` | Index of the normal mode (starts from 1). The modes are sorted by ascanding order of the corresponding frequencies | `1` |
| `-s`<br />`--scale` | Scaling factor for the normal mode vector | `0.75` |
| `-o`<br />`--output` | Output file with the displaced coordinates. Prints as xyz to stdout if not specified | XYZ on stdout |

Example usage:
```sh
# Displace along the first mode and write XYZ coordinates to stdout.
gmod freq.log > displaced.xyz

# Select a mode, scale its displacement, and choose an output file.
gmod freq.log -m 2 -s 0.5 -o displaced.xyz

# Displace in the opposite direction.
gmod freq.log -m 2 -s -0.75 -o displaced-reverse.xyz
```

### `replcoords`

Replaces the coordinate block in a `com` file **inplace** by coordinates from a file supported by Open Babel, such as `xyz`, `mol` or `log`.

Example usage:
```sh
# Replace coordinates of sp.com by opt.log.
replcoords opt.log sp.com
```

### `maxdispl`

Prints step number together with `Maximum Displacement` lines sorted by decreasing displacement value.

Example usage:
```sh
maxdispl opt.log
```

Example output:
```
     7	 Maximum Displacement     0.063493     0.001800     NO 
     8	 Maximum Displacement     0.062950     0.001800     NO 
    26	 Maximum Displacement     0.060043     0.001800     NO 
     1	 Maximum Displacement     0.058530     0.001800     NO 
    19	 Maximum Displacement     0.058056     0.001800     NO 
    15	 Maximum Displacement     0.057077     0.001800     NO 
    22	 Maximum Displacement     0.044076     0.001800     NO 
```

### `tseigs`

Prints eigenvectors of TS optimization for each step.

| Flag | Description |
| --- | --- |
| `-b` | Include bond lengths |
| `-a` | Include bond angles |
| `-d` | Include dihedrals |

Example usage:
```sh
# Show all coordinate types.
tseigs tsopt.log

# Show only bond lengths.
tseigs -b tsopt.log

# Show only bond lengths and bond angles.
tseigs -ba tsopt.log
```

Example output:
```
 52          R(116,121)          R(116,125)          R(119,125)
                0.69755            -0.30553             0.12909
 53          R(116,121)          R(116,125)          R(119,125)
                0.68971            -0.30918             0.12859
 54          R(116,121)          R(116,125)          R(119,125)
                0.69522            -0.30675             0.13005
```

## License

[GNU General Public License, version 2](LICENSE)
