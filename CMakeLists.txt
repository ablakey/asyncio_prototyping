cmake_minimum_required(VERSION 2.8.3)
project(asyncio_prototyping)

find_package(catkin REQUIRED COMPONENTS
  catkin_virtualenv
  rospy
)

catkin_package()

catkin_generate_virtualenv(
  PYTHON_VERSION 3.7
)


install(FILES requirements.txt
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})


catkin_install_python(
  PROGRAMS
        scripts/summator
        scripts/numberwang
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY launch/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch
)
