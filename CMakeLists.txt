cmake_minimum_required(VERSION 3.11)

project(nginx VERSION 1.9.9 LANGUAGES C)

if(ALL_IN_ONE)
file(GLOB nginx_core_SRC ${CMAKE_CURRENT_SOURCE_DIR}/src/core/*.c)
file(GLOB nginx_event_SRC ${CMAKE_CURRENT_SOURCE_DIR}/src/event/*.c)
file(GLOB nginx_http_SRC ${CMAKE_CURRENT_SOURCE_DIR}/src/http/*.c)
file(GLOB nginx_mail_SRC ${CMAKE_CURRENT_SOURCE_DIR}/src/mail/*.c)
file(GLOB nginx_misc_SRC ${CMAKE_CURRENT_SOURCE_DIR}/src/misc/*.c)
file(GLOB nginx_os_SRC ${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/*.c)
file(GLOB nginx_stream_SRC ${CMAKE_CURRENT_SOURCE_DIR}/src/stream/*.c)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src/core)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src/event)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/objs)

add_executable(nginx ${nginx_core_SRC}
		     ${nginx_event_SRC}
		     ${nginx_mail_SRC}
		     ${nginx_os_SRC}
		     ${nginx_stream_SRC}
		     ${nginx_misc_SRC}
		     ${nginx_http_SRC})

add_definitions(-DNGX_THREADS)
target_link_libraries(nginx PRIVATE pthread)
# SET(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} -pthread")
# set_target_properties(nginx PROPERTIES LINK_LIBRARIES -pthread)
else()
set(ALL_INCS ${CMAKE_CURRENT_SOURCE_DIR}/src/core
			${CMAKE_CURRENT_SOURCE_DIR}/src/event
			${CMAKE_CURRENT_SOURCE_DIR}/src/event/modules
			${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix
			${CMAKE_CURRENT_SOURCE_DIR}/objs
			${CMAKE_CURRENT_SOURCE_DIR}/src/http
			${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules)

file(GLOB CORE_DEPS ${CMAKE_CURRENT_SOURCE_DIR}/src/core/nginx.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_config.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_core.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_log.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_palloc.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_array.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_list.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_hash.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_buf.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_queue.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_string.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_parse.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_parse_time.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_inet.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_file.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_crc.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_crc32.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_murmurhash.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_md5.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_sha1.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_rbtree.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_radix_tree.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_rwlock.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_slab.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_times.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_shmtx.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_connection.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_cycle.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_conf_file.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_module.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_resolver.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_open_file_cache.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_crypt.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_proxy_protocol.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_syslog.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/event/ngx_event.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/event/ngx_event_timer.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/event/ngx_event_posted.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/event/ngx_event_connect.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/event/ngx_event_pipe.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/event/ngx_event_udp.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_time.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_errno.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_alloc.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_files.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_channel.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_shmem.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_process.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_setaffinity.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_setproctitle.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_atomic.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_gcc_atomic_x86.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_thread.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_socket.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_os.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_user.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_dlopen.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_process_cycle.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_linux_config.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_linux.h
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_regex.h
	${CMAKE_CURRENT_SOURCE_DIR}/objs/ngx_auto_config.h)

set(CORE_INCS ${CMAKE_CURRENT_SOURCE_DIR}/src/core
			${CMAKE_CURRENT_SOURCE_DIR}/src/event
			${CMAKE_CURRENT_SOURCE_DIR}/src/event/modules
			${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix
			${CMAKE_CURRENT_SOURCE_DIR}/objs)


file(GLOB HTTP_DEPS ${CMAKE_CURRENT_SOURCE_DIR}/src/http/ngx_http.h
				${CMAKE_CURRENT_SOURCE_DIR}/src/http/ngx_http_request.h
				${CMAKE_CURRENT_SOURCE_DIR}/src/http/ngx_http_config.h
				${CMAKE_CURRENT_SOURCE_DIR}/src/http/ngx_http_core_module.h
				${CMAKE_CURRENT_SOURCE_DIR}/src/http/ngx_http_cache.h
				${CMAKE_CURRENT_SOURCE_DIR}/src/http/ngx_http_variables.h
				${CMAKE_CURRENT_SOURCE_DIR}/src/http/ngx_http_script.h
				${CMAKE_CURRENT_SOURCE_DIR}/src/http/ngx_http_upstream.h
				${CMAKE_CURRENT_SOURCE_DIR}/src/http/ngx_http_upstream_round_robin.h
				${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_ssi_filter_module.h)

file(GLOB core_SRC ${CMAKE_CURRENT_SOURCE_DIR}/src/core/nginx.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_log.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_palloc.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_array.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_list.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_hash.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_buf.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_queue.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_output_chain.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_string.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_parse.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_parse_time.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_inet.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_file.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_crc32.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_murmurhash.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_md5.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_sha1.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_rbtree.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_radix_tree.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_slab.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_times.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_shmtx.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_connection.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_cycle.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_spinlock.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_rwlock.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_cpuinfo.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_conf_file.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_resolver.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_open_file_cache.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_crypt.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_regex.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_proxy_protocol.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/core/ngx_syslog.c)
	
file(GLOB event_SRC ${CMAKE_CURRENT_SOURCE_DIR}/src/event/ngx_event.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/event/ngx_event_timer.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/event/ngx_event_posted.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/event/ngx_event_accept.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/event/ngx_event_udp.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/event/ngx_event_connect.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/event/ngx_event_pipe.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/event/modules/ngx_epoll_module.c)

file(GLOB os_SRC ${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_time.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_errno.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_alloc.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_files.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_socket.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_recv.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_readv_chain.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_udp_recv.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_send.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_writev_chain.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_udp_send.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_udp_sendmsg_chain.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_channel.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_shmem.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_process.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_daemon.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_setaffinity.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_setproctitle.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_posix_init.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_user.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_dlopen.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_process_cycle.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_linux_init.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/os/unix/ngx_linux_sendfile_chain.c)
	
file(GLOB http_SRC ${CMAKE_CURRENT_SOURCE_DIR}/src/http/ngx_http.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/ngx_http_core_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/ngx_http_special_response.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/ngx_http_request.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/ngx_http_parse.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_log_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/ngx_http_request_body.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/ngx_http_variables.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/ngx_http_script.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/ngx_http_upstream.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/ngx_http_upstream_round_robin.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/ngx_http_file_cache.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/ngx_http_write_filter_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/ngx_http_header_filter_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_chunked_filter_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_range_filter_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_gzip_filter_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/ngx_http_postpone_filter_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_ssi_filter_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_charset_filter_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_userid_filter_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_headers_filter_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/ngx_http_copy_filter_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_not_modified_filter_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_static_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_autoindex_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_index_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_mirror_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_try_files_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_auth_basic_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_access_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_limit_conn_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_limit_req_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_geo_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_map_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_split_clients_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_referer_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_rewrite_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_proxy_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_fastcgi_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_uwsgi_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_scgi_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_memcached_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_empty_gif_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_browser_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_upstream_hash_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_upstream_ip_hash_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_upstream_least_conn_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_upstream_random_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_upstream_keepalive_module.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/http/modules/ngx_http_upstream_zone_module.c)

file(GLOB rtmp_SRC ${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_init.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_handshake.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_handler.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_amf.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_send.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_shared.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_eval.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_receive.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_core_module.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_cmd_module.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_codec_module.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_access_module.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_record_module.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_live_module.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_play_module.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_flv_module.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_mp4_module.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_netcall_module.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_relay_module.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_bandwidth.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_exec_module.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_auto_push_module.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_notify_module.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_log_module.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_limit_module.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_bitop.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_proxy_protocol.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/hls/ngx_rtmp_hls_module.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/dash/ngx_rtmp_dash_module.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/hls/ngx_rtmp_mpegts.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/dash/ngx_rtmp_mp4.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_stat_module.c
				${CMAKE_CURRENT_SOURCE_DIR}/rtmp/ngx_rtmp_control_module.c)

file(GLOB ngx_ssl_SRC ${CMAKE_CURRENT_SOURCE_DIR}/src/event/ngx_event_openssl.c
					${CMAKE_CURRENT_SOURCE_DIR}/src/event/ngx_event_openssl_stapling.c)

file(GLOB modules_SRC ${CMAKE_CURRENT_SOURCE_DIR}/objs/ngx_modules.c)

include_directories(${ALL_INCS} ${CORE_INCS} ${CMAKE_CURRENT_SOURCE_DIR}/rtmp)
add_executable(nginx ${modules_SRC} ${core_SRC} ${event_SRC} ${http_SRC} ${os_SRC} ${CORE_DEPS} ${HTTP_DEPS} ${rtmp_SRC} ${ngx_ssl_SRC})

# set_target_properties(nginx PROPERTIES LINK_LIBRARIES -pthread -dl -crypt -pcre2-8 -z -Wl -E)
# SET(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} -dl -lpthread -lcrypt -lpcre2-8 -lz -Wl,-E")
# SET(CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG} -dl -lpthread -lcrypt -lpcre2-8 -lz -Wl,-E")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -pipe  -O -W -Wall -Wpointer-arith -Wno-unused-parameter -Werror -g")
# -pipe  -O -W -Wall -Wpointer-arith -Wno-unused-parameter -Werror -g
target_link_libraries(nginx PRIVATE dl pthread pcre2-8 crypt z)
target_link_libraries(nginx PRIVATE ssl crypto)
endif(ALL_IN_ONE) # all in one