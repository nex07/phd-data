# accel-nssm-vs-classical-crane

Simulation data of a small overhead crane with a variable rope length and a variable payload mass. The data accompany a manuscript in preparation. Further sets will be added.

## Files

| File | Size in bytes | SHA-256 |
|---|---|---|
| `data/devB_in.h5` | 15072927 | `46421a15abd15ae59fa8db1c0e04a2a014eb779b8b9c8b033990bd87444194d9` |
| `data/devB_ood.h5` | 15704609 | `51ba19dbfa7408c5092abc8db00b22bccc40383e472d48c09bf01657f69ec85e` |
| `data/devC_in.h5` | 15085982 | `24e7dea3ded330fb1bdba3861acecaf98d17326b01b23e2609e7635f2791b013` |
| `data/devC_ood.h5` | 15704584 | `d5f0cfcff7adc2a72b4177f5494313bc5febab2f7edf14f3f12fe6ad1280de0f` |

`MANIFEST.sha256` lists the same checksums in the format of `sha256sum`. To check the files, run `sha256sum -c MANIFEST.sha256` in this folder.

The training file is too large for an ordinary file of the repository (more than 100 MB) and is attached to the release `train-data-v1` of this repository:

| File | Size in bytes | SHA-256 |
|---|---|---|
| `train_2048.h5` (release asset) | 120565481 | `95c8e6f177b1ecafee49b654d7178661795c390a4095cde64545f397fdea625d` |

Its checksum is in `RELEASE_ASSETS.sha256`. To check it, download the file into this folder and run `sha256sum -c RELEASE_ASSETS.sha256`.

## What the four sets are

The four sets are made for the study and none of them was used to fit or to select a model. `devB_in` and `devB_ood` are the first pair and `devC_in` and `devC_ood` a second pair with new seed bases. They are made with the same plant and the same generator and differ in the excitation families and in the number of episodes per cell.

| Set | Episodes | Payload masses (kg) | Excitation families | Episodes per mass and family | Seed base |
|---|---|---|---|---|---|
| `devB_in` | 256 | 1, 6, 11, 15 | F0, F1, F3, F5 | 16 | 10100000 |
| `devB_ood` | 256 | 1, 6, 11, 15 | F2, F4 | 32 | 10200000 |
| `devC_in` | 256 | 1, 6, 11, 15 | F0, F1, F3, F5 | 16 | 10300000 |
| `devC_ood` | 256 | 1, 6, 11, 15 | F2, F4 | 32 | 10400000 |

The seed of an episode is the seed base plus the episode index plus a multiple of 256 for every rejected attempt. A trajectory is rejected when it leaves the workspace envelope or the velocity limits of the plant. The realised seeds lie between 10100001 and 10101354 for `devB_in`, between 10200000 and 10200740 for `devB_ood`, between 10300000 and 10301753 for `devC_in` and between 10400001 and 10400730 for `devC_ood`.

## Training file

`train_2048.h5` holds 2048 episodes of 1000 samples (20 s) with the same file layout as the four sets below. The payload masses are 1, 6, 11 and 15 kg and the excitation families are F0, F1, F3 and F5. The seed base is 1000000, the realised seeds lie between 1000000 and 1014265, and 789 attempts were rejected when a trajectory left the workspace envelope or the velocity limits. The fitting set of 1920 episodes and the selection set of 128 episodes used in the article are carved from this file with the seed 20261105. The seed ranges of the file and of the four sets do not overlap.

## Plant

The data come from a Lagrangian model of a small crane with a trolley, a bridge and a hoist, and a payload on a rope. The plant configuration is the preset `hoist20` of the small crane. The plant files carry the commit identifier `ee8076c7f0732a57290e148c8119e505fd0f2abb` and the configuration has the SHA-256 `96cedb9eb604965b7c72fca583ff5fbf8f0c31f809da6c97e0ba7049a095106c`. The plant source files will be added.

## File layout

Each file is an HDF5 file with one group `episodes`. The group holds one subgroup per episode, named `ep000000`, `ep000001` and so on. Every episode has three datasets.

| Dataset | Shape | Type | Meaning |
|---|---|---|---|
| `t` | 1000 | float64 | Time in seconds, sampling interval 0.02 s, so an episode lasts 20 s |
| `x` | 1000 by 10 | float32 | State |
| `u` | 1000 by 3 | float32 | Command |

The columns of `x` are `x`, `x_dot`, `alpha`, `alpha_dot`, `y`, `y_dot`, `beta`, `beta_dot`, `l`, `l_dot`. Lengths and the rope length `l` are in millimetres, velocities in millimetres per second, angles in radians and angular rates in radians per second. The columns of `u` are `u_x`, `u_y`, `u_z`. The command is dimensionless with a saturation interval of minus one to one.

The attributes of an episode are `m_load` (payload mass in kg), `family` (code 0 to 5 for F0 to F5), `episode_seed`, `length_bin`, `edge_side`, `coverage_stratum` and `angle_cap_deg`. The attributes of the file record the generator settings, the plant and the seed base. No state clipping is applied.

## Use in the article

The sets are used to score trained models in free run over 20 s. The model starts from the first state of an episode and receives the stored command sequence. The article compares two network forms on `devB_in` and repeats the comparison on `devC_in`. The article describes `devB_ood` with descriptive values only, and `devC_ood` was not analysed. Neither out-of-distribution file was used for a decision.

## Not yet included

The generator, the plant source and the scoring scripts are not part of this upload. Additional sets will be added.

## Licence

See the file `LICENSE` in the top folder of the repository.
