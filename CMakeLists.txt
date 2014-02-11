cmake_minimum_required ( VERSION 2.8 )

project ( lzmq C )
include ( cmake/dist.cmake )
include ( lua )

include_directories ( ${CMAKE_CURRENT_SOURCE_DIR}/lzmq/src )

set ( LUA_LZTIMER_SRC lzmq/src/ztimer.c lzmq/src/lzutils.c )

install_lua_module ( lzmq               lzmq/src/lua/lzmq/ffi/lzmq.lua     )
install_lua_module ( lzmq.ffi           lzmq/src/lua/lzmq/ffi.lua          )
install_lua_module ( lzmq.loop          lzmq/src/lua/lzmq/loop.lua         )
install_lua_module ( lzmq.poller        lzmq/src/lua/lzmq/ffi/poller.lua   )
install_lua_module ( lzmq.threads       lzmq/src/lua/lzmq/threads.lua      )
install_lua_module ( lzmq.ffi.api       lzmq/src/lua/lzmq/ffi/api.lua      )
install_lua_module ( lzmq.ffi.error     lzmq/src/lua/lzmq/ffi/error.lua    )
install_lua_module ( lzmq.llthreads.ex  lzmq/src/lua/lzmq/llthreads/ex.lua )
install_lua_module ( lzmq.impl.threads  lzmq/src/lua/lzmq/impl/threads.lua )
install_lua_module ( lzmq.impl.loop     lzmq/src/lua/lzmq/impl/loop.lua    )

if(UNIX AND NOT APPLE)
  set ( RT_LIBRARY rt )
else()
  set ( RT_LIBRARY )
endif()

install_lua_module ( lzmq.timer ${LUA_LZTIMER_SRC} LINK ${RT_LIBRARY} )

install_data ( lzmq/README.md lzmq/LICENCE.txt )

install_example ( lzmq/examples/ )

install_test ( lzmq/test/ lzmq/examples/utils.lua )
