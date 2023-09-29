# Instructions for building mlperf app with Samsung backend support 

## Clone the repo 
git clone https://github.com/mlcommons/mobile_app_open
cd mobile_app_open

## Switch to submission v3.1 freeze branch 
git checkout ea1937f4e3428384103eb8a1890445df9cf1f2dc

## Apply patch for Samsung updated backend 
git apply samsung_submission_v3.1.patch 

## Copy new Samsung backend files 
cp mbe_config_2400.hpp mobile_back_samsung/samsung/lib/public/include/
cp mbe_config_2400.pbtxt mobile_back_samsung/samsung/lib/public/include/

## Copy libraries:
from https://github.com/mlcommons/mobile_back_samsung/tree/submission_v3.1_samsung_backend/samsung_libs 
to:
mobile_back_samsung/samsung/lib/internal/

## Build the app
make WITH_SAMSUNG=1 flutter/android/release

## Push app to device
adb install output/android-apks/2023-MM-DD_mlperfbench-ea1937f-.apk 

## Launch the app once to create the cache folder
adb shell "am start -n org.mlcommons.android.mlperfbench/org.mlcommons.android.mlperfbench.MainActivity"

## Push samsung models to the device cache folder
create a directory MLPerf_sideload and copy models from closed/Samsung/code/ExynosNN/models to it
adb push path/to/MLPerf_sideload  /sdcard/Android/data/org.mlcommons.android.mlperfbench/files/

## Push the datatsets to the device
adb push mlperf_datasets /sdcard/Android/data/org.mlcommons.android.mlperfbench/files/

# Now you can launch the app, select submission mode and press GO
