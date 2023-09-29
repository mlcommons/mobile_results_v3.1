######
###### Build mlperf_app.apk with QTI backend support for testing on Android
######

git clone https://github.com/mlcommons/mobile_app_open
cd mobile_app_open
git checkout submission-v3.1
git apply 0001-MLPerf-submission-for-QTI-backend-for-Mobile-V3.1.patch
git apply 0002-Fix-for-invalid-results-for-SD-4-Gen-2.patch

#download the snpe sdk 2.13.4.230831 from this path
https://qpm.qualcomm.com/#/main/tools/details/qualcomm_neural_processing_sdk
# Select "Linux" and "Version 2.13.4.230831" from this path.
# Instructions to install Qualcomm Package Manager 3 is also in the same page above.

Install QPM3 from https://qpm.qualcomm.com/#/main/tools/details/QPM3

From terminal, run below commands:

1. qpm-cli --login <username>
2. qpm-cli --license-activate qualcomm_neural_processing_sdk
3. qpm-cli --extract qualcomm_neural_processing_sdk (or)
   qpm-cli --extract <full path to downloaded .qik file>

# rename the folder extracted above to qaisw-2.13.4.230831213719_60417

# copy the snpe sdk to mobile_back_qti
# make sure to copy qaisw-2.13.4.230831213719_60417,
make OFFICIAL_BUILD=false FLUTTER_BUILD_NUMBER=1 WITH_QTI=1 docker/flutter/android/release

# Install app to device
adb install output/android-apks/<date>_mlperfbench-<commit_id>-qt.apk
#Open the app once on the device (to create the app's directory)

# Generate the DLCs
cd mobile_back_qti/DLC/
make SNPE_SDK=<path to unzipped qaisw-2.13.4.230831213719_60417>

# Push the models to the device
adb push output/DLC/mlperf_models /sdcard/Android/data/org.mlcommons.android/mlperfbench/files/

# Push the datatsets to the device (required only for accuracy)
adb push mlperf_datasets /sdcard/Android/data/org.mlcommons.android/mlperfbench/files/

# select submission mode and press GO
