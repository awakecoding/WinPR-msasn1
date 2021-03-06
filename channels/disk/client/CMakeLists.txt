# FreeRDP: A Remote Desktop Protocol Client
# FreeRDP cmake build script
#
# Copyright 2012 Marc-Andre Moreau <marcandre.moreau@gmail.com>
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

set(MODULE_NAME "disk")
set(MODULE_PREFIX "CHANNEL_RDPDR_DISK_CLIENT")

set(${MODULE_PREFIX}_SRCS
	disk_file.c
	disk_file.h
	disk_main.c)
	
if(WIN32)
	set(${MODULE_PREFIX}_SRCS ${${MODULE_PREFIX}_SRCS}
		statvfs.c
		statvfs.h
		dirent.h)
endif()

if(MSVC AND (NOT WITH_STATIC_PLUGINS))
	set(${MODULE_PREFIX}_SRCS ${${MODULE_PREFIX}_SRCS} module.def)
endif()

include_directories(../../rdpdr/client)

add_library(${MODULE_NAME} ${${MODULE_PREFIX}_SRCS})
set_target_properties(${MODULE_NAME} PROPERTIES PREFIX "")

if(WITH_MONOLITHIC_BUILD)
	set(${MODULE_PREFIX}_LIBS freerdp winpr)
else()
	set(${MODULE_PREFIX}_LIBS freerdp-utils winpr-crt winpr-synch winpr-thread winpr-interlocked)
endif()

target_link_libraries(${MODULE_NAME} ${${MODULE_PREFIX}_LIBS})

install(TARGETS ${MODULE_NAME} DESTINATION ${FREERDP_PLUGIN_PATH})

set_property(TARGET ${MODULE_NAME} PROPERTY FOLDER "Channels/${MODULE_NAME}/Client")


