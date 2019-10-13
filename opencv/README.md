# OpenCV

## g++
compile, including opencv headers and libraries
[compiling opencv in c++](https://stackoverflow.com/questions/9094941/compiling-opencv-in-c)
```sh
g++ xxx.cpp `pkg-config --cflags --libs opencv`
```

## cmake
### use OpenCV after installing
To use OpenCV, add the following in `CMakeLists.txt`:
```
set(OpenCV_DIR /usr/local/lib)
FIND_PACKAGE(OpenCV REQUIRED)
add_definitions(-DOPENCV)
add_executable(<exeutable_file_name> <./src/xxx.cpp>)
# ${OpenCV_LIBS} is set by "FIND_PACKAGE"
target_link_libraries(<exeutable_file_name>  ${OpenCV_LIBS})
```

### use OpenCV without installing
To use OpenCV without installing, we need to:
1. copy all `/usr/local/lib/libopencv*.so.4.1.2` into `${CMAKE_SOURCE_DIR}/libs`
2. make softlinks: libopencv_photo.so -> libopencv_photo.so.4.1.2 and libopencv_photo.so.4.1 -> libopencv_photo.so.4.1.2
3. copy `/usr/local/include/opencv4/opencv2` into `${PROJECT_SOURCE_DIR}/include`

And then add the following in `CMakeLists.txt`:
```
# ${CMAKE_SOURCE_DIR} is the directory containing CMakeLists.txt
link_directories(${CMAKE_SOURCE_DIR}/libs)
set(OpenCV_LIBS "opencv_photo;opencv_videoio;opencv_imgcodecs;opencv_imgproc;opencv_dnn;opencv_calib3d;opencv_stitching;opencv_core;opencv_features2d;opencv_ml;opencv_video;opencv_flann;opencv_objdetect;opencv_highgui;opencv_gapi;opencv_superres;opencv_img_hash;opencv_bgsegm;opencv_line_descriptor;opencv_tracking;opencv_datasets;opencv_xobjdetect;opencv_face;opencv_quality;opencv_hdf;opencv_text;opencv_bioinspired;opencv_xfeatures2d;opencv_xphoto;opencv_rgbd;opencv_ximgproc;opencv_surface_matching;opencv_fuzzy;opencv_stereo;opencv_freetype;opencv_dnn_objdetect;opencv_shape;opencv_reg;opencv_hfs;opencv_videostab;opencv_saliency;opencv_dpm;opencv_structured_light;opencv_optflow;opencv_dnn_superres;opencv_aruco;opencv_sfm;opencv_plot;opencv_phase_unwrapping;opencv_ccalib")
# message("OpenCV_LIBS" "${OpenCV_LIBS}")
# should copy /usr/local/lib/libopencv_photo.so.4.1.2(and all other opencv libs) to "${CMAKE_SOURCE_DIR}/libs"
# and make softlinks libopencv_photo.so -> libopencv_photo.so.4.1.2 and libopencv_photo.so.4.1 -> libopencv_photo.so.4.1.2
add_executable(<exeutable_file_name> <./src/xxx.cpp>)
target_link_libraries(<exeutable_file_name>  ${OpenCV_LIBS})
# should copy /usr/local/include/opencv4/opencv2 into ${PROJECT_SOURCE_DIR}/include
target_include_directories(<exeutable_file_name> PUBLIC ${PROJECT_SOURCE_DIR}/include)
```