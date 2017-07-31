# hipRAND

The hipRAND library provides functions that generate pseudo-random and quasi-random numbers.
The library is implemented in the [HIP](https://github.com/ROCm-Developer-Tools/HIP)
programming language, optimised for AMD's latest discrete GPUs. It is designed to run on top
of AMD's Radeon Open Compute [ROCm](https://rocm.github.io/) runtime, but it also works
on CUDA enabled GPUs.

Additionally, hipRAND includes a small wrapper library which allows user to easily port CUDA
applications that use cuRAND library to the [HIP](https://github.com/ROCm-Developer-Tools/HIP)
layer.

## Requirements

* cmake (2.8.12 or later)
* C++ compiler with C++11 support
* For AMD platforms:
    * [ROCm](https://rocm.github.io/install.html) (1.5 or later)
* For CUDA platforms:
    * [HIP](https://github.com/ROCm-Developer-Tools/HIP) (hcc is not required)
    * Latest CUDA SDK

## Build and Install

```
git clone https://github.com/ROCmSoftwarePlatform/hipRAND.git

# go to hipRAND directory, create and go to build directory
cd hipRAND; mkdir build; cd build

# configure hipRAND, setup options for your system
# build options: BUILD_TEST, BUILD_BENCHMARK (off by default), BUILD_CRUSH_TEST (off by default)
cmake ../. # or cmake-gui ../.
cmake -DBUILD_BENCHMARK=ON -DBUILD_CRUSH_TEST=ON ../.

# build
# for ROCM-1.6, if a HCC runtime error is caught, consider setting HCC_AMDGPU_TARGET=<arch> in front of make as a workaround 
make -j4

# optionally, run tests if they're enabled
ctest

# install
sudo make install
```

## Running Benchmarks and Tests

```
# go to hipRAND build directory
cd hipRAND; cd build

# to run benchmark for generate functions
# engine -> all, philox, mrg32k3a, xorwow
# distribution -> all, uniform-uint, uniform-float, uniform-double, normal-float, normal-double, log-normal-float,
#                 log-normal-double, poisson
# further option can be found using --help
./benchmark/benchmark_rocrand_generate --engine <engine> --dis <distribution>

# to run benchmark for device kernel functions
# engine -> all, philox, mrg32k3a, xorwow
# distribution -> all, uniform-uint, uniform-float, uniform-double, normal-float, normal-double, log-normal-float,
#                 log-normal-double, poisson, discrete-poisson, discrete-custom
# further option can be found using --help
./benchmark/benchmark_rocrand_kernel --engine <engine> --dis <distribution>

# to compare against curand (curand must be supported), simply run
./benchmark/benchmark_curand_generate --engine <engine> --dis <distribution>
./benchmark/benchmark_curand_kernel --engine <engine> --dis <distribution>

# to run unit tests
./test/<unit-test>

# to run crush test (to test randomness of generators, not to be used with QRNGs)
# curand version of test also exists
./test/crush_test_rocrand --engine <engine>

# to run Pearson Chi-squared test
# curand version of test also exists
./test/pearson_chi_squared_rocrand --engine <engine>
```

## Documentation

```
# go to hipRAND doc directory
cd hipRAND; cd doc

# run doxygen
doxygen Doxyfile

# open html/index.html

```

## Contributions and License

Contributions of any kind are most welcome! More details are found
at [CONTRIBUTING](./CONTRIBUTING.md) and [LICENSE](./LICENSE.txt).
