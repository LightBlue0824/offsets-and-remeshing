# Offsets and Remeshing

This repository contains an implementation of the method presented in "Feature-Preserving Offset Mesh Generation from Topology-Adapted Octrees".
It generates offsets for triangle meshes and performs feature aware remeshing.

## Dependencies

The code is based on [CGAL](https://www.cgal.org/) and was tested with version 5.5.2. For the offsetting library requires slight modification of the Octrees package.

The code requires the following libraries to be installed:

- [CGAL](https://www.cgal.org/) the results in the paper were produced with version 5.5.2 with slight modification to the Octrees package.
- GLOG
- CLI11

The Orthree in CGAL needs to be slightly modified:

1. The `split()` function in Orthtree must be public and all point handling (everything after node.split()) must be deleted.
2. The `split()` and `unsplit()` functions in Orthtree/Node.h must be public.

# Usage of the CLI

Example input for command line interface _voxeloffset_cli_.

```
./build/src/voxeloffset_cli/Voxeloffset_cli -p "../../data/loader_normalized.off" -o "loader_d12_r5/out.off" --debugout "loader_d12_r5" -d --d1 12 --d2 18 -r 1 -n -q
```

Explanation:

- `-p` primal mesh
- `-o` output mesh
- `--debugout` folder for debug output
- `-d` perform Dual Contouring
- `--d0` minimum octree subdivision level (can be 0)
- `--d1` maximum octree subdivision level (must be greater than `d0`)
- `--d2` octree subdivision level for removing non-manifold vertices (must be greater than `d1`)
- `-r` number of remeshing steps. For not performing remeshing, set this to 0
- `--lminfactor` multiplier for the minimal edge length
- `--lmaxfactor` multiplier for the maximal edge length
- `-n` normalize input mesh to the unit cube

## Remeshing only

```
./build/src/voxeloffset_cli/Voxeloffset_cli -p "../../data/loader_normalized.off" --remeshinput "loader_triangulated_d12_outer.off" -o "loader_d12_outer_r5/out.off" --debugout "loader_d12_outer_r5" -r 1 -q -j 0.01
```

Explanation:

- `-p` primal mesh
- `--remeshinput` offset mesh that should be remeshed
- `-o` output mesh
- `--debugout` folder for debug output
- `-r` number of remeshing steps.
- `--lminfactor` multiplier for the minimal edge length
- `--lmaxfactor` multiplier for the maximal edge length
