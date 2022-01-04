# Waveshare Pico-10DOF-IMU

This is a repo for the [Pico-10DOF-IMU](https://www.waveshare.com/wiki/Pico-10DOF-IMU). I have refactored the code provided by waveshare from the archive provided at the wiki to conform to C++ and cmake project standards.

## Building

The [pico-sdk](https://github.com/raspberrypi/pico-sdk) needs to be cloned locally first.
### Standalone
```bash
mkdir -p build && cd $_
PICO_SDK_PATH=/path/to/pico-sdk cmake ..
# alternatively the above command can be split into two if PICO_SDK_PATH is exported as an env var first with export PICO_SDK_PATH=...
make -j4
```

### As a Subproject

First, make sure that the following `add_subdirectory` func call is inside either the root `CMakeLists.txt` or within an intermediate project's `CMakeLists.txt`.

```cmake
add_subdirectory("path/to/waveshare_pico_10dof_imu") # this is the relative path to waveshare_pico_10dof_imu from the CMakeLists.txt this call is added to
```
Next, add the following to each executable that is being built that uses waveshare_pico_10dof_imu.
```cmake
target_include_directories(my_executable
  PRIVATE
  ${waveshare_pico_10dof_imu_INCLUDE_DIRS}
  # other headers that are used go here too
)

target_link_libraries(
  my_executable
  pico_stdlib icm20948 lps22hb hardware_i2c
  # other libraries that are used go here too
)
```