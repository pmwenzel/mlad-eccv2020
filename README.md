# MLAD Challenge

# Usage

## Dataset Preparation
For the re-localization challenge, we provide 5 different traversals of the same sequence at
different times. We provide a reference map, which contains high-accurate
ground truth. We also do provide another traversal with ground truth to be used
for training. One sequence can be used for validation
and the two remaining sequences will be used as test sequences. The directory of the reference map has the following structure:

```
.
├── Calibration
│   ├── undistorted_calib_0.txt
│   ├── undistorted_calib_1.txt
│   └── undistorted_calib_stereo.txt
├── KeyFrameData
├── RelocalizationFilesTest
│   ├── relocalizationFile_recording_2020-03-24_17-45-31_easy.txt
│   ├── relocalizationFile_recording_2020-03-24_17-45-31_hard.txt
│   ├── relocalizationFile_recording_2020-03-24_17-45-31_moderate.txt
│   ├── relocalizationFile_recording_2020-04-23_19-37-00_easy.txt
│   ├── relocalizationFile_recording_2020-04-23_19-37-00_hard.txt
│   └── relocalizationFile_recording_2020-04-23_19-37-00_moderate.txt
├── RelocalizationFilesTrain
│   └── relocalizationFile_recording_2020-03-24_17-36-22.txt
├── RelocalizationFilesVal
│   └── relocalizationFile_recording_2020-03-03_12-03-23.txt
├── keyframe_accuracy.txt
├── poses.txt
├── times.txt
└── undistorted_images
    ├── cam0
    └── cam1
```

Here,
- `Calibration` contains both the intrinsic and extrinsic calibration of the
  stereo camera. 
- `KeyFrameData` contains the KeyFrameFiles.
- `RelocalizationFilesTest` contains the re-localization files between the
  source (reference) and the target sequence. 
- `RelocalizationFilesTrain` contains the re-localization files between the
  source (reference) and the training sequence together with the relative poses.
- `RelocalizationFilesVal` contains the re-localization files between the
  source (reference) and the validation sequence together with the relative
poses.
- `keyframe_accuracy.txt` is a list of keyframes with a translational accuracy
  lower than `0.05m`.
- `poses.txt` is a list of `6DOF` poses for all keyframes from the left camera to world
  coordinates.
- `times.txt` is a list of times in unix timestamps, and exposure times for each keyframe.
- `undistorted_images` contains both the images from the left and right camera,
  respectively.

The KeyFrameData folder contains a `.txt` file for each keyframe. Each Keyframe
file contains the following data: timestamp, camera intrinsics, camToWorld
transformation, exposure time and point cloud data. Each point is saved with
the following properties:

* `u,v` image coordinates of depth point in pixels
* `idepth_scaled` inverse depth value in `1/m`
  * metric 3D camera coordinates are obtained as follows: `x=(u-cx)/(fx*idepth_scaled)`; `y=(v-cy)/(fy*idepth_scaled)`; `z=1/idepth_scaled`
* `idepth_hessian` hessian component of the corresponding inverse depth
  * estimated inverse depth variance is obtained as follows: `var=1/idepth_hessian`
* `maxRelBaseline` maximum relative stereo baseline
* `numGoodRes` not relevant
* `status` not relevant
* `color information` color values of an 8 pixel patch around the depth pixel (see https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=7898369 for definition)

The `6DOF` poses are specified as translation (`t_x`, `t_y`, `t_z`), and quaternion (`q_x`, `q_y`, `q_z`, `q_w`). The test re-localization files are provided as easy, moderate, and hard version, which differ mainly in a larger translational distance between the source and target frame.

The directories of the test sequences should have the following structure:

```
.
├── Calibration
│   ├── undistorted_calib_0.txt
│   ├── undistorted_calib_1.txt
│   └── undistorted_calib_stereo.txt
├── times.txt
└── undistorted_images
    ├── cam0
    └── cam1
```

## Task

Each re-localization file contains a list of first the keyframe from the source (reference) sequence, and the target
keyframe. The task is to provide the relative pose between the source and the
target. Note that the relative pose for the validation file is from cam0 of the
source sequence to cam0 of the target sequence, respectively.   

## Submission

For the test sequences, the ground truth is withheld. For submitting your
results to the challenge, please refer to the steps described at:
https://sites.google.com/view/mlad-eccv2020/challenge.

