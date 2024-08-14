cmake_minimum_required(VERSION 3.5)
project(mono2d_trash_detection)

# Default to C99
if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 99)
endif()

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
# uncomment the following section in order to fill in
# further dependencies manually.
# find_package(<dependency> REQUIRED)

if(PLATFORM_X3)
  message("build platform X3")
  set(PREFIX_PATH x3)
elseif(PLATFORM_X5)
  message("build ${PROJECT_NAME} for platform X5")
  set(PREFIX_PATH x5)
elseif(PLATFORM_X86)
  message("build platform X86")
  set(PREFIX_PATH x3)
else()
  message("invalid platform, build platform X3 default")
  set(PREFIX_PATH x3)
endif()

# Install executables
install(DIRECTORY
  ${PROJECT_SOURCE_DIR}/config/${PREFIX_PATH}/
  DESTINATION lib/${PROJECT_NAME}/config/
)

install(FILES
  ${PROJECT_SOURCE_DIR}/config/ppyoloworkconfig.json
  DESTINATION lib/${PROJECT_NAME}/config/
)
install(FILES
  ${PROJECT_SOURCE_DIR}/config/trash_coco.list
  DESTINATION lib/${PROJECT_NAME}/config/
)
install(FILES
  ${PROJECT_SOURCE_DIR}/config/trashDet0028.jpg
  DESTINATION lib/${PROJECT_NAME}/config/
)

ament_package()
