# accel-nssm-vs-classical-crane

Simulation data of a small overhead crane with a variable rope length and a variable payload mass. The data accompany a manuscript in preparation. Further sets will be added.

## Files

| File | Size in bytes | SHA-256 |
|---|---|---|
| `data/devB_in.h5` | 15072927 | `46421a15abd15ae59fa8db1c0e04a2a014eb779b8b9c8b033990bd87444194d9` |
| `data/devB_ood.h5` | 15704609 | `51ba19dbfa7408c5092abc8db00b22bccc40383e472d48c09bf01657f69ec85e` |
| `data/devC_in.h5` | 15085982 | `24e7dea3ded330fb1bdba3861acecaf98d17326b01b23e2609e7635f2791b013` |
| `data/devC_ood.h5` | 15704584 | `d5f0cfcff7adc2a72b4177f5494313bc5febab2f7edf14f3f12fe6ad1280de0f` |
| `data/devD_in.h5` | 14997399 | `c40e18bf9e4dcd50bb6febd3f6342eb09916f23e505fbc98f89280eeca3b170c` |
| `data/devD_ood.h5` | 15707833 | `8636bf9f004f39c5289687fc32ae08ab37407d2f5dfa7f8f01d4023b3a7190bc` |
| `data/ident_train_256.h5` | 15548733 | `a1bb634887e3974d20ae9764fa2ffbf40310c145192169917b32ec1cf5c91349` |
| `data/ident_val_64.h5` | 3893161 | `588f1cc41320fe1bb77a4f88f575eadec28fa978574ff59700f106a534e1b5c3` |
| `data/ident_test_64.h5` | 3891296 | `1bdf942e190eb0f61c2bb4d27c159e0c1d5660e05eb27a6876ac9312566fc57c` |
| `data/ident_interpolation_48.h5` | 2923013 | `d3f73682e47f40ee8640b44b3a120427113f42d7363983f609d1c4351e3f49cf` |
| `data/ident_18kg_16.h5` | 979197 | `2e8f421675e15f70f2e1ab83f7b78e01f9bc8723c62b5e3cdbc03555ba9c79db` |

`MANIFEST.sha256` lists the same checksums in the format of `sha256sum`. To check the files, run `sha256sum -c MANIFEST.sha256` in this folder.

The two training files are too large for an ordinary file of the repository (more than 100 MB). Each is attached to its own release of this repository:

| File | Release | Size in bytes | SHA-256 |
|---|---|---|---|
| `train_2048.h5` (release asset) | `train-data-v1` | 120565481 | `95c8e6f177b1ecafee49b654d7178661795c390a4095cde64545f397fdea625d` |
| `train_2048_v2.h5` (release asset) | `train-data-v2` | 120645147 | `a7627ecd75908a05806f5344289134955c7e8dd1006e241fcfc34542fef6dde9` |

Their checksums are in `RELEASE_ASSETS.sha256`. To check them, download the files into this folder and run `sha256sum -c RELEASE_ASSETS.sha256`.

## What the four sets are

The four sets below are made for the study and none of them was used to fit or to select a model. `devB_in` and `devB_ood` are the first pair and `devC_in` and `devC_ood` a second pair with new seed bases. They are made with the same plant and the same generator and differ in the excitation families and in the number of episodes per cell.

| Set | Episodes | Payload masses (kg) | Excitation families | Episodes per mass and family | Seed base |
|---|---|---|---|---|---|
| `devB_in` | 256 | 1, 6, 11, 15 | F0, F1, F3, F5 | 16 | 10100000 |
| `devB_ood` | 256 | 1, 6, 11, 15 | F2, F4 | 32 | 10200000 |
| `devC_in` | 256 | 1, 6, 11, 15 | F0, F1, F3, F5 | 16 | 10300000 |
| `devC_ood` | 256 | 1, 6, 11, 15 | F2, F4 | 32 | 10400000 |

The seed of an episode is the seed base plus the episode index plus a multiple of 256 for every rejected attempt. A trajectory is rejected when it leaves the workspace envelope or the velocity limits of the plant. The realised seeds lie between 10100001 and 10101354 for `devB_in`, between 10200000 and 10200740 for `devB_ood`, between 10300000 and 10301753 for `devC_in` and between 10400001 and 10400730 for `devC_ood`.

## Sets D and the second training file

`devD_in` and `devD_ood` are a third pair of scoring sets, made with the same plant and the same generator as the other sets and with new seed bases. `train_2048_v2.h5` is a second training file of 2048 episodes with a new seed base. Its seeds do not overlap with those of any other file of this folder. The attribute `mass_split` of a file carries the internal name of the set in the generator, which differs from the file name for the sets D (`devD_in.h5` has the internal name `devB_d_in`, `devD_ood.h5` the name `devB_d_ood`, and `train_2048_v2.h5` the name `devT2_train`).

| Set | Episodes | Payload masses (kg) | Excitation families | Episodes per mass and family | Seed base | Realised seeds | Rejected attempts |
|---|---|---|---|---|---|---|---|
| `devD_in` | 256 | 1, 6, 11, 15 | F0, F1, F3, F5 | 16 | 11200000 | 11200000 to 11201387 | 95 |
| `devD_ood` | 256 | 1, 6, 11, 15 | F2, F4 | 32 | 11300000 | 11300000 to 11300906 | 30 |
| `train_2048_v2` | 2048 | 1, 6, 11, 15 | F0, F1, F3, F5 | 128 | 11000000 | 11000000 to 11013227 | 851 |

The two sets D were not used to fit or to select a model. The fitting set of 1920 episodes and the selection set of 128 episodes of the second training file are carved from it in the same way as for the first training file, with the seed 20261105. The file layout, the plant and the units are those described below.

## Files of the identification study

Five files belong to a study of the free-run identification of the same plant, which is reported in the article as a suitability test. They were made with an older version of the generator, which draws the initial rope length over the whole range of the rope and has no group of starts near a rope limit. They hold 1000 samples of 20 s per episode and the same three datasets per episode as the other files. The episodes carry the attributes `m_load` and `episode_seed`. The files were called `dataset.h5` in folders named after the split, and the table gives the original name of each split.

| File | Original name | Episodes | Payload masses (kg) | Seed base | Realised seeds | Use |
|---|---|---|---|---|---|---|
| `ident_train_256.h5` | train | 256 | 1, 6, 11, 15 | 139000 | 139001 to 140230 | fits the models of the study |
| `ident_val_64.h5` | val | 64 | 1, 6, 11, 15 | 239000 | 239001 to 239207 | choice of the checkpoint of the neural model |
| `ident_test_64.h5` | test | 64 | 1, 6, 11, 15 | 339000 | 339001 to 339172 | scoring, 16 episodes per mass |
| `ident_interpolation_48.h5` | interpolation | 48 | 3.5, 8.5, 13 | 439000 | 439001 to 439174 | scoring at masses between the training masses, 16 episodes per mass |
| `ident_18kg_16.h5` | extrapolation | 16 | 18 | 539000 | 539001 to 539027 | scoring at a mass above the training masses, reported only |

The test, interpolation and 18 kg splits were scored once after the models were fitted. The files are unchanged copies and the file names are the only difference to the originals. Renaming does not change the bytes, so the checksums are those of the originals.

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
