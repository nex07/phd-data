# accel-nssm-vs-classical-crane

Simulation data of a small overhead crane with a variable rope length and a variable payload mass. The data accompany a manuscript in preparation. Further sets will be added.

## Files

| File | Size in bytes | SHA-256 |
|---|---|---|
| `data/devB_in.h5` | 15072927 | `46421a15abd15ae59fa8db1c0e04a2a014eb779b8b9c8b033990bd87444194d9` |
| `data/devB_ood.h5` | 15704609 | `51ba19dbfa7408c5092abc8db00b22bccc40383e472d48c09bf01657f69ec85e` |

`MANIFEST.sha256` lists the same checksums in the format of `sha256sum`. To check the files, run `sha256sum -c MANIFEST.sha256` in this folder.

## What the two sets are

Both sets are development sets. They are made with the same plant and the same generator and differ in the excitation families and in the number of episodes per cell.

| Set | Episodes | Payload masses (kg) | Excitation families | Episodes per mass and family | Seed base |
|---|---|---|---|---|---|
| `devB_in` | 256 | 1, 6, 11, 15 | F0, F1, F3, F5 | 16 | 10100000 |
| `devB_ood` | 256 | 1, 6, 11, 15 | F2, F4 | 32 | 10200000 |

The seed of an episode is the seed base plus the episode index plus a multiple of 256 for every rejected attempt. A trajectory is rejected when it leaves the workspace envelope or the velocity limits of the plant. The realised seeds lie between 10100001 and 10101354 for `devB_in` and between 10200000 and 10200740 for `devB_ood`.

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

The two sets are used to score trained models in free run over 20 s. The model starts from the first state of an episode and receives the stored command sequence.

## Not yet included

The generator, the plant source and the scoring scripts are not part of this first upload. Additional sets will be added.

## Licence

See the file `LICENSE` in the top folder of the repository.
