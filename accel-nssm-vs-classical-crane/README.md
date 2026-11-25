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

## Matched comparison: time series and results

The folder `matched_comparison` holds the stored outputs that the figures and tables of the matched comparison of the article read. The 12 time-series files (`score96_timeseries/`) hold, for each of the six seeds (11, 21, 41, 51, 61 and 71) of the two networks, the free-run predictions on the 96 scoring episodes of the selection set: the arrays `pred`, `ref` (reference trajectory), `commands`, `mass_kg`, `family` and `episode_ids`. The five JSON files (`results/`) are the result files that the analysis scripts wrote. In the three JSON files that carried absolute paths of the machine, the path prefixes are replaced by `<repo>` and `<unit>`. No number is changed, so the redaction changes only text and the checksums below are those of the published files.

| File | Size in bytes | SHA-256 |
|---|---|---|
| `matched_comparison/score96_timeseries/N1_accel_pure_seed11_score96_timeseries.npz` | 7800758 | `a009fb63348d6e7be0a1ccee81d7f44d193fbe7404801a6dcb19b6b752ce0e8a` |
| `matched_comparison/score96_timeseries/N1_accel_pure_seed21_score96_timeseries.npz` | 7800158 | `edb061a5dadbf941fccd8eaf032269a52618b4162a344e4b8c3352362df02412` |
| `matched_comparison/score96_timeseries/N1_accel_pure_seed41_score96_timeseries.npz` | 7800058 | `5fa496564a83c4fe4081c07dfd6bb05b952cd3ad99bf1cad6e4ec4ff955a12cd` |
| `matched_comparison/score96_timeseries/N1_accel_pure_seed51_score96_timeseries.npz` | 7800419 | `dbb00306035747d459618f155214d2d00e74a15215543460eef1d7b72760e0a0` |
| `matched_comparison/score96_timeseries/N1_accel_pure_seed61_score96_timeseries.npz` | 7803161 | `68bcf336b9436c41b5634debdd01a631493e9b03c4f41cebfe9a0ebdf54e5b70` |
| `matched_comparison/score96_timeseries/N1_accel_pure_seed71_score96_timeseries.npz` | 7799758 | `bbed277364a8c9bda458f9cb25b58e0255146f673ac4ac23cc3c91a4308f734a` |
| `matched_comparison/score96_timeseries/N4_direct_plain_w362_seed11_score96_timeseries.npz` | 7816099 | `5fedc1330e66faf9c438f8db0986aae67deab7bd31da94fb37b6a86628032590` |
| `matched_comparison/score96_timeseries/N4_direct_plain_w362_seed21_score96_timeseries.npz` | 7810695 | `46517c5b80f0efef1e34fa7a8fbe639d14f3d5fb290d085350bbf926aba03210` |
| `matched_comparison/score96_timeseries/N4_direct_plain_w362_seed41_score96_timeseries.npz` | 7814528 | `d68dd65685d0c7139042e20edb340b27c2f6c69930535ac22cc72fcd2ad0bc93` |
| `matched_comparison/score96_timeseries/N4_direct_plain_w362_seed51_score96_timeseries.npz` | 7812306 | `23c274a8835aeae12ffd3a54da66cfb93ce366c1687acfbd5bbb6001a4434554` |
| `matched_comparison/score96_timeseries/N4_direct_plain_w362_seed61_score96_timeseries.npz` | 7812025 | `fcb1dc3ca3ca61c734c3fcc1b68b32fa3f99e3aef236b1a188ff83a5e57160be` |
| `matched_comparison/score96_timeseries/N4_direct_plain_w362_seed71_score96_timeseries.npz` | 7816807 | `f99ba6981fbf76a67231c76e31d6692cd533d195c9073ca0503507137b419e5f` |
| `matched_comparison/results/ab_b_table_b1.json` | 11209 | `b60c1642d215df777e1fc3d746e750a4a0bdb0ee502f07a9d990de27be76ace2` |
| `matched_comparison/results/analysis_b1_exit_channels.json` | 14764 | `af4550f251c48fdff78f08233d9f9e31a32920afbee086cca7d448417f6f610d` |
| `matched_comparison/results/analysis_b1_results.json` | 49649 | `f5e391c4b3fce692924dc45acae69a2f49f1e02ebae4ff761c68cbb9d0a96031` |
| `matched_comparison/results/analysis_posthoc_edge_baseline_results.json` | 591299 | `cbb7c51cd33b984855bf09eb6ee17511b048c4b2bb214aca187ce5ee19ae62c1` |
| `matched_comparison/results/velocity_structure_results.json` | 3511 | `02a63e1e27fa3901971b74d4d5ba0c45731d423c5757ca5771b8bf585783290e` |

`MANIFEST.sha256` lists these checksums too. To check a download, run `sha256sum -c MANIFEST.sha256` in this folder.
