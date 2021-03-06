
SET(SRC_FILES
	Rtabmap.cpp
	RtabmapThread.cpp
	
	Statistics.cpp
	
	Memory.cpp
	
	DBDriver.cpp
	DBDriverSqlite3.cpp
	DBReader.cpp
	
	Recovery.cpp
	
    Camera.cpp
    CameraThread.cpp
    CameraRGB.cpp
    CameraRGBD.cpp
    CameraStereo.cpp
    CameraModel.cpp
    
    EpipolarGeometry.cpp
	VisualWord.cpp
	VWDictionary.cpp
	BayesFilter.cpp
	Parameters.cpp
    Signature.cpp
	Features2d.cpp
	Transform.cpp
	GeodeticCoords.cpp
	
	util2d.cpp
	
	util3d.cpp
	util3d_filtering.cpp
	util3d_mapping.cpp
	util3d_transforms.cpp
	util3d_registration.cpp
	util3d_surface.cpp
	util3d_features.cpp
	util3d_correspondences.cpp
	util3d_motion_estimation.cpp
	
	SensorData.cpp
	Graph.cpp
	Compression.cpp
	Link.cpp
	
	Optimizer.cpp
	OptimizerTORO.cpp
	OptimizerG2O.cpp
	OptimizerGTSAM.cpp
	OptimizerCVSBA.cpp
	
	Registration.cpp
	RegistrationIcp.cpp
	RegistrationVis.cpp
	
	Odometry.cpp
	OdometryThread.cpp
	OdometryF2M.cpp
	OdometryMono.cpp
	OdometryF2F.cpp
	OdometryFovis.cpp
	OdometryViso2.cpp
	OdometryDVO.cpp
	OdometryORBSLAM2.cpp
	
	Stereo.cpp
	StereoDense.cpp
	StereoCameraModel.cpp
	
	OccupancyGrid.cpp
	
	GainCompensator.cpp
		
	rtflann/ext/lz4.c
	rtflann/ext/lz4hc.c
	FlannIndex.cpp
	
	sqlite3/sqlite3.c	
	
	#clams stuff
	clams/discrete_depth_distortion_model_helpers.cpp
	clams/discrete_depth_distortion_model.cpp
	clams/frame_projector.cpp
	clams/slam_calibrator.cpp
)

IF(OpenCV_VERSION_MAJOR EQUAL 2)
SET(SRC_FILES
    ${SRC_FILES}
    opencv/Orb.cpp
    opencv/solvepnp.cpp
)
ENDIF(OpenCV_VERSION_MAJOR EQUAL 2)

# to get includes in visual studio
IF(MSVC)
    FILE(GLOB HEADERS
       ../include/${PROJECT_PREFIX}/core/*.h
    )
    SET(SRC_FILES ${SRC_FILES} ${HEADERS})
ENDIF(MSVC)

SET(INCLUDE_DIRS
    ${PROJECT_SOURCE_DIR}/utilite/include
	${CMAKE_CURRENT_SOURCE_DIR}/../include
	${CMAKE_CURRENT_SOURCE_DIR}
	${CMAKE_CURRENT_BINARY_DIR}
    ${OpenCV_INCLUDE_DIRS}
	${PCL_INCLUDE_DIRS}
	${ZLIB_INCLUDE_DIRS}
)

SET(LIBRARIES
	${OpenCV_LIBS} 
	${PCL_LIBRARIES} 
	${ZLIB_LIBRARIES} 
)

IF(Freenect_FOUND)
	IF(Freenect_DASH_INCLUDES)
	   ADD_DEFINITIONS("-DFREENECT_DASH_INCLUDES")
	ENDIF(Freenect_DASH_INCLUDES)
	SET(INCLUDE_DIRS
		${INCLUDE_DIRS}
		${Freenect_INCLUDE_DIRS}
	)
	SET(LIBRARIES
		${LIBRARIES}
		${Freenect_LIBRARIES}
	)
ENDIF(Freenect_FOUND)

IF(OpenNI2_FOUND)
	SET(INCLUDE_DIRS
		${INCLUDE_DIRS}
		${OpenNI2_INCLUDE_DIRS}
	)
	SET(LIBRARIES
		${LIBRARIES}
		${OpenNI2_LIBRARIES}
	)
ENDIF(OpenNI2_FOUND)

IF(freenect2_FOUND)
	SET(INCLUDE_DIRS
		${INCLUDE_DIRS}
		${freenect2_INCLUDE_DIRS}
	)
	SET(LIBRARIES
		${LIBRARIES}
		${freenect2_LIBRARIES}
	)
ENDIF(freenect2_FOUND)

IF(KinectSDK2_FOUND)
	SET(INCLUDE_DIRS
		${INCLUDE_DIRS}
		${KinectSDK2_INCLUDE_DIRS}
	)
	SET(LIBRARIES
		${LIBRARIES}
		${KinectSDK2_LIBRARIES}
	)
ENDIF(KinectSDK2_FOUND)

IF(RealSense_FOUND)
	SET(INCLUDE_DIRS
		${INCLUDE_DIRS}
		${RealSense_INCLUDE_DIRS}
	)
	SET(LIBRARIES
		${LIBRARIES}
		${RealSense_LIBRARIES}
	)
ENDIF(RealSense_FOUND)

IF(DC1394_FOUND)
	SET(INCLUDE_DIRS
		${INCLUDE_DIRS}
		${DC1394_INCLUDE_DIRS}
	)
	SET(LIBRARIES
		${LIBRARIES}
		${DC1394_LIBRARIES}
	)
ENDIF(DC1394_FOUND)

IF(FlyCapture2_FOUND)
	SET(INCLUDE_DIRS
		${INCLUDE_DIRS}
		${FlyCapture2_INCLUDE_DIRS}
	)
	SET(LIBRARIES
		${LIBRARIES}
		${FlyCapture2_LIBRARIES}
	)
ENDIF(FlyCapture2_FOUND)

IF(WITH_TORO)
	SET(SRC_FILES
    	${SRC_FILES}
		toro3d/posegraph3.cpp
		toro3d/treeoptimizer3_iteration.cpp
		toro3d/treeoptimizer3.cpp
	
		toro3d/posegraph2.cpp
		toro3d/treeoptimizer2.cpp
	)
ENDIF(WITH_TORO)

IF(G2O_FOUND)
	SET(INCLUDE_DIRS 
		${INCLUDE_DIRS} 
		${G2O_INCLUDE_DIRS}
	)
	SET(LIBRARIES
		${LIBRARIES}
		${G2O_LIBRARIES}
	)
	
	IF(WITH_VERTIGO)
		SET(SRC_FILES
    		${SRC_FILES}
			vertigo/g2o/edge_se2Switchable.cpp
			vertigo/g2o/edge_se3Switchable.cpp
			vertigo/g2o/edge_switchPrior.cpp
			vertigo/g2o/types_g2o_robust.cpp
			vertigo/g2o/vertex_switchLinear.cpp
		)
	ENDIF(WITH_VERTIGO)
ENDIF(G2O_FOUND)

IF(GTSAM_FOUND)
	IF(GTSAM_INCLUDE_DIR)
		SET(INCLUDE_DIRS 
			${GTSAM_INCLUDE_DIR} # place it in front to use Eigen installed by GTSAM
			${INCLUDE_DIRS} 
		)
	ELSE()
		SET(INCLUDE_DIRS 
			${GTSAM_INCLUDE_DIRS} # cmake standard
			${INCLUDE_DIRS} 
		)
	ENDIF()
	add_definitions("-DGTSAM_IMPORT_STATIC")
	SET(LIBRARIES
		${LIBRARIES}
		gtsam
	)
ENDIF(GTSAM_FOUND)

IF(cvsba_FOUND)
	SET(INCLUDE_DIRS 
		${INCLUDE_DIRS} 
		${cvsba_INCLUDE_DIRS}
	)
	SET(LIBRARIES
		${LIBRARIES}
		${cvsba_LIBS}
	)
ENDIF(cvsba_FOUND)

IF(libpointmatcher_FOUND)
	SET(INCLUDE_DIRS 
		${INCLUDE_DIRS} 
		${libpointmatcher_INCLUDE_DIRS}
	)
	SET(LIBRARIES
		${LIBRARIES}
		${libpointmatcher_LIBRARIES}
	)
ENDIF(libpointmatcher_FOUND)

IF(ZED_FOUND)
	SET(INCLUDE_DIRS
		${INCLUDE_DIRS}
		${ZED_INCLUDE_DIRS}
	)
	SET(LIBRARIES
		${LIBRARIES}
		${ZED_LIBRARIES}
	)
	IF(CUDA_FOUND)
		SET(INCLUDE_DIRS
			${INCLUDE_DIRS}
			${CUDA_INCLUDE_DIRS}
		)
		SET(LIBRARIES
			${LIBRARIES}
			${CUDA_LIBRARIES}
		)
	ENDIF(CUDA_FOUND)
ENDIF(ZED_FOUND)

IF(OCTOMAP_FOUND)
    SET(INCLUDE_DIRS
		${INCLUDE_DIRS}
		${OCTOMAP_INCLUDE_DIRS}
	)
	SET(LIBRARIES
		${LIBRARIES}
		${OCTOMAP_LIBRARIES}
	)
	SET(SRC_FILES
    	${SRC_FILES}
		OctoMap.cpp
	)
ENDIF(OCTOMAP_FOUND)

IF(libfovis_FOUND)
    SET(INCLUDE_DIRS
		${INCLUDE_DIRS}
		${libfovis_INCLUDE_DIRS}
	)
	SET(LIBRARIES
		${LIBRARIES}
		${libfovis_LIBRARIES}
	)
ENDIF(libfovis_FOUND)

IF(libviso2_FOUND)
    SET(INCLUDE_DIRS
		${INCLUDE_DIRS}
		${libviso2_INCLUDE_DIRS}
	)
	SET(LIBRARIES
		${LIBRARIES}
		${libviso2_LIBRARIES}
	)
ENDIF(libviso2_FOUND)

IF(dvo_core_FOUND)
    SET(INCLUDE_DIRS
		${INCLUDE_DIRS}
		${dvo_core_INCLUDE_DIRS}
	)
	SET(LIBRARIES
		${LIBRARIES}
		${dvo_core_LIBRARIES}
	)
ENDIF(dvo_core_FOUND)

IF(ORB_SLAM2_FOUND)
    SET(INCLUDE_DIRS
        ${ORB_SLAM2_INCLUDE_DIRS} #before so that g2o includes are taken from ORB_SLAM2 directory before the official g2o one
		${INCLUDE_DIRS}
	)
	SET(LIBRARIES
	    ${ORB_SLAM2_LIBRARIES}
		${LIBRARIES}
	)
ENDIF(ORB_SLAM2_FOUND)

####################################
# Generate resources files
####################################
CONFIGURE_FILE(${CMAKE_CURRENT_SOURCE_DIR}/resources/DatabaseSchema.sql.in ${CMAKE_CURRENT_SOURCE_DIR}/resources/DatabaseSchema.sql)

SET(R
	${CMAKE_CURRENT_SOURCE_DIR}/resources/DatabaseSchema.sql
)

#replace semicolons by spaces
foreach(arg ${R})
   set(RESOURCES "${RESOURCES}" "${arg}")
endforeach(arg ${R})

SET(RESOURCES_HEADERS 
	${CMAKE_CURRENT_BINARY_DIR}/DatabaseSchema_sql.h 
)

IF(ANDROID)

   IF(NOT RTABMAP_RES_TOOL)
      find_host_program(RTABMAP_RES_TOOL rtabmap-res_tool PATHS ${CMAKE_RUNTIME_OUTPUT_DIRECTORY})
      IF(NOT RTABMAP_RES_TOOL)
         MESSAGE( FATAL_ERROR "RTABMAP_RES_TOOL is not defined (it is the path to \"rtabmap-res_tool\" application created by a non-Android build)." )
      ENDIF(NOT RTABMAP_RES_TOOL)
   ENDIF(NOT RTABMAP_RES_TOOL)

   ADD_CUSTOM_COMMAND(
      OUTPUT ${RESOURCES_HEADERS}
      COMMAND ${RTABMAP_RES_TOOL} -n rtabmap -p ${CMAKE_CURRENT_BINARY_DIR} ${RESOURCES}
      COMMENT "[Creating resources]"
      DEPENDS ${R}
   )
ELSE()
   ADD_CUSTOM_COMMAND(
      OUTPUT ${RESOURCES_HEADERS}
      COMMAND ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/rtabmap-res_tool -n rtabmap -p ${CMAKE_CURRENT_BINARY_DIR} ${RESOURCES}
      COMMENT "[Creating resources]"
      DEPENDS ${R} res_tool
   )
ENDIF()

####################################
# Generate resources files END
####################################

add_definitions(${PCL_DEFINITIONS})


# Make sure the compiler can find include files from our library.
INCLUDE_DIRECTORIES(${INCLUDE_DIRS})

# Add binary that is built from the source file "main.cpp".
# The extension is automatically found.
ADD_LIBRARY(rtabmap_core ${SRC_FILES} ${RESOURCES_HEADERS})
TARGET_LINK_LIBRARIES(rtabmap_core rtabmap_utilite ${LIBRARIES})

SET_TARGET_PROPERTIES(
     rtabmap_core
   PROPERTIES
     VERSION ${RTABMAP_VERSION}
     SOVERSION ${RTABMAP_MAJOR_VERSION}.${RTABMAP_MINOR_VERSION}
)

INSTALL(TARGETS rtabmap_core
        RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}" COMPONENT runtime
        LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}" COMPONENT devel
        ARCHIVE DESTINATION "${CMAKE_INSTALL_LIBDIR}" COMPONENT devel)
		
install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/../include/ 
		DESTINATION "${INSTALL_INCLUDE_DIR}"
		COMPONENT devel 
		FILES_MATCHING PATTERN "*.h" PATTERN "*.hpp" 
		PATTERN ".svn" EXCLUDE)

