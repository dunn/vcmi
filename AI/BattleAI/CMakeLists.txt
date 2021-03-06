project(battleAI)
cmake_minimum_required(VERSION 2.6)

include_directories(${Boost_INCLUDE_DIRS} ${CMAKE_HOME_DIRECTORY} ${CMAKE_HOME_DIRECTORY}/include ${CMAKE_CURRENT_SOURCE_DIR} ${CMAKE_HOME_DIRECTORY}/lib)

set(battleAI_SRCS
		StdInc.cpp

		AttackPossibility.cpp
		BattleAI.cpp
		common.cpp
		EnemyInfo.cpp
		main.cpp
		PotentialTargets.cpp
		StackWithBonuses.cpp
		ThreatMap.cpp
)

set(battleAI_HEADERS
		StdInc.h

		AttackPossibility.h
		BattleAI.h
		common.h
		EnemyInfo.h
		PotentialTargets.h
		StackWithBonuses.h
		ThreatMap.h
)

if (ANDROID) # android compiles ai libs into main lib directly, so we skip this library and just reuse sources list
	return()
endif()
add_library(BattleAI SHARED ${battleAI_SRCS} ${battleAI_HEADERS})
target_link_libraries(BattleAI vcmi)

set_target_properties(BattleAI PROPERTIES ${PCH_PROPERTIES})
cotire(BattleAI)

if (NOT APPLE) # Already inside vcmiclient bundle
	install(TARGETS BattleAI RUNTIME DESTINATION ${AI_LIB_DIR} LIBRARY DESTINATION ${AI_LIB_DIR})
endif()

