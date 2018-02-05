## redis

[![Build Status](https://travis-ci.org/Oefenweb/ansible-redis.svg?branch=master)](https://travis-ci.org/Oefenweb/ansible-redis) [![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-redis-blue.svg)](https://galaxy.ansible.com/tersmitten/redis)

Set up the latest stable redis server in Ubuntu systems (using [Chris Lea's ppa](https://launchpad.net/~chris-lea/+archive/ubuntu/redis-server) or from source).

#### Requirements

None

#### Variables

##### Installation

* `redis_install_method`: [default: `'ppa'`]: The installation method to use (e.g. `src`)
* `redis_version`: [default: `4.0.7`]: Redis version to install (only applicable to `src` installs)

##### (Multiple) instances

* `redis_instance_name`: [default: `''`]: The name of the redis server instance, required when you want to run multiple instances

##### Configuration

* `redis_include`: [default: `[]`]: Other config files to include (e.g. `['/path/to/local.conf', '/path/to/other.conf']`) 
* `redis_loadmodules`: [default: `[]`]: Modules to load at startup (e.g. `['/path/to/my_module.so', '/path/to/other_module.so']`) (`>= 4.0.0` only)
* `redis_bind`: [default: `['127.0.0.1']`]: Listen for connections form the specified IP addresses
* `redis_protected_mode`: [default: `true`]: By default protected mode is enabled. You should disable it only if you are sure you want clients from other hosts to connect to Redis even if no authentication is configured, nor a specific set of interfaces are explicitly listed using the `bind` directive (`>= 3.2` only)
* `redis_port`: [default: `6379`]: Accept connections on the specified port
* `redis_tcp_backlog`: [default: `511`]: TCP `listen()` backlog. In high requests-per-second environments you need an high backlog in order to avoid slow clients connections issues
* `redis_unixsocket`: [optional]: The path for the Unix socket that will be used to listen for incoming connections (e.g. `/var/run/redis/{{ redis_instance_name_full }}.sock`)
* `redis_unixsocketperm`: [optional]: The Unix socket permissions (e.g. `755`)
* `redis_timeout`: [default: `0`]: Close the connection after a client is idle for `N` seconds
* `redis_tcp_keepalive`: [default: `300` on `>= 3.2.1` else `60`]: TCP keepalive, if non-zero, use `SO_KEEPALIVE` to send `TCP` `ACKs` to clients in absence of communication
* `redis_daemonize`: [default: `true`]: Whether or not to run as a daemon
* `redis_pidfile`: [default: `/var/run/redis/{{ redis_instance_service }}.pid`]: The path for the PID file that will be created on startup and removed on end
* `redis_loglevel`: [default: `notice`]: Server verbosity level (e.g. `debug`, `verbose`, `notice` or `warning`)
* `redis_logfile`: [default: `""`]: The log file name. Also the empty string can be used to force the server to log on the standard output. Note that if you use standard output for logging but `daemonize`, logs will be sent to `/dev/null`
* `redis_syslog_enabled`: [default: `false`]: Whether or not to enable logging to the system logger
* `redis_syslog_ident`: [default: `{{ redis_instance_name_full }}`]: The syslog identity
* `redis_syslog_facility`: [default: `local0`]: The syslog facility (e.g. `USER` or between `LOCAL0-LOCAL7`)
* `redis_databases`: [default: `16`]: The number of databases
* `redis_always_show_logo`: [default: `true`]: Force the pre-4.0 behavior and always show a ASCII art logo in startup logs (`>= 4.0.0` only)
* `redis_saves`: [default: `[{seconds: 900, changes: 1}, {seconds: 300, changes: 10}, {seconds: 60, changes: 10000}]`]: Saves declaration
* `redis_saves.{n}.seconds`: [required]: Will save the DB if the given number of seconds have passed
* `redis_saves.{n}.changes`: [required]: Will save the DB if the given number of write operations against the DB occurred
* `redis_stop_writes_on_bgsave_error`: [default: `true`]: Whether or not to stop writes on bgsave error
* `redis_rdbcompression`: [default: `true`]: Whether or not to compress string objects using `LZF` when dump `.rdb` databases
* `redis_rdbchecksum`: [default: `true`]: Whether or not to checksum (`CRC64`) databases
* `redis_dbfilename`: [default: `dump.rdb`]: The filename where to dump the database
* `redis_dir`: [default: `/var/lib/{{ redis_instance_name_full }}`]: The working directory
* `redis_slaveof`: [optional]: Master-Slave replication declaration
* `redis_slaveof.masterip`: [required]: Master ip address
* `redis_slaveof.masterport`: [required]: Master port
* `redis_masterauth`: [optional]: Master server password
* `redis_slave_serve_stale_data`: [default: `true`]: Whether or not serve stale data (when a slave loses its connection with the master, or when the replication is still in progress)
* `redis_slave_read_only`: [default: `true`]: Whether or not a slave instance should accept writes
* `redis_repl_diskless_sync`: [default: `false`]: Whether or not to use diskless sync
* `redis_repl_diskless_sync_delay`: [default: `5`]: When diskless sync is enabled, it is possible to configure the delay the server waits in order to spawn the child that transfers the RDB via socket to the slaves
* `redis_repl_ping_slave_period`: [default: `10`]: Slaves send PINGs to server in a predefined interval. Use this variable to change this interval
* `redis_repl_timeout`: [default: `60`]: Sets the replication timeout for: bulk transfer I/O during `SYNC`, from the point of view of slave (1), master timeout from the point of view of slaves (data, pings) (2), slave timeout from the point of view of masters (`REPLCONF` `ACK` pings) (3)
* `redis_repl_disable_tcp_nodelay`: [default: `false`]: Whether or not to disable `TCP_NODELAY` on the slave socket after `SYNC`
* `redis_repl_backlog_size`: [default: `1mb`]: Sets the replication backlog size
* `redis_repl_backlog_ttl`: [default: `3600`]: The amount of seconds that need to elapse, starting from the time the last slave disconnected, for the backlog buffer to be freed
* `redis_slave_priority`: [default: `100`]: The slave priority. A slave with a low priority number is considered better for promotion (used by [Sentinel](http://redis.io/topics/sentinel))
* `redis_min_slaves_to_write`: [default: `0`]: It is possible for a master to stop accepting writes if there are less than `N` slaves connected (`0` disables the feature)
* `redis_min_slaves_max_lag`: [default: `10`]: It is possible for a master to stop accepting writes if having a lag less or equal than `M` seconds (`0` disables the feature)
* `redis_slave_announce`: [default: `{}`]: [Slave announce](https://github.com/antirez/redis/blob/unstable/redis.conf#L446) declaration (`>= 3.2.2` only)
* `redis_slave_announce.ip`: [optional]: Slave announce ip address
* `redis_slave_announce.port`: [optional]: Slave announce port
* `redis_requirepass`: [optional]: Require clients to issue `AUTH <PASSWORD>` before processing any other commands
* `redis_command_renames`: [default: `[]`]: Renames declaration
* `redis_command_renames.{}.before`: [required]: The original command name (e.g. `CONFIG`)
* `redis_command_renames.{}.after`: [required]: The new command name (e.g. `""`, `b840fc02d524045429941cc15f59e41cb7be6c52`)
* `redis_maxclients`: [optional]: Set the max number of connected clients at the same time
* `redis_maxmemory`: [optional]: Don't use more memory than the specified amount of bytes
* `redis_maxmemory_policy`: [default: `noeviction`]: How the server will select what to remove when `redis_maxmemory` is reached (e.g. `volatile-lru`, `allkeys-lru`, `volatile-random`, `allkeys-random`, `volatile-ttl`)
* `redis_maxmemory_samples`: [default: `5`]: `LRU` and minimal `TTL` algorithms are not precise algorithms but approximated algorithms, so you can tune it for speed or accuracy. By default the server will check `N` keys and pick the one that was used less recently
* `redis_lazyfree_lazy_eviction`: [default: `false`]: (`>= 4.0.0` only)
* `redis_lazyfree_lazy_expire`: [default: `false`]: (`>= 4.0.0` only)
* `redis_lazyfree_lazy_server_del`: [default: `false`]: (`>= 4.0.0` only)
* `redis_slave_lazy_flush`: [default: `false`]: (`>= 4.0.0` only)
* `redis_appendonly`: [default: `false`]: Whether or not to use [Append Only File](http://redis.io/topics/persistence)
* `redis_appendfilename`: [default: `appendonly.aof`]: The name of the append only file
* `redis_appendfsync`: [default: `everysec`]: The [fsync](http://antirez.com/post/redis-persistence-demystified.html) setting that the server will use (e.g. `everysec`, `always` or `no`)
* `redis_no_appendfsync_on_rewrite`: [default: `false`]: Whether or not to disable `fsync` during AOF log rewrites
* `redis_auto_aof_rewrite_percentage`: [default: `100`]: If the current size is bigger than the specified percentage, an AOF log rewrite is triggered
* `redis_auto_aof_rewrite_min_size`: [default: `64mb`]: The minimal size for the AOF file to be rewritten
* `redis_aof_load_truncated`: [default: `true`]: If set to `true`, a truncated AOF file is loaded and the server starts emitting a log to inform the user of the event. Otherwise if the option is set to `false`, the server aborts with an error and refuses to start
* `redis_lua_time_limit`: [default: `5000`]: Max execution time of a Lua script in milliseconds
* `redis_slowlog_log_slower_than`: [default: `10000`]: Log queries slower than `M` microseconds. Note that a negative number disables the slow log, while a value of zero forces the logging of every command
* `redis_slowlog_max_len`: [default: `128`]: The maximum length of the queue of logged queries
* `redis_latency_monitor_threshold`: [default: `0`]: Latency monitoring subsystem samples different operations at runtime in order to collect data related to possible sources of latency. To enable use a values of `0` or more milliseconds
* `redis_notify_keyspace_events`: [default: `""`]: Takes as argument a string that is composed of zero or multiple characters. An empty string means that notifications are disabled
* `redis_hash_max_ziplist_entries`: [default: `512`]: Hashes are encoded using a memory efficient data structure when they have a small number of entries
* `redis_hash_max_ziplist_value`: [default: `64`]: Hashes are encoded using a memory efficient data structure when the biggest entry does not exceed a given threshold
* `redis_list_max_ziplist_entries`: [default: `512`]: Lists are encoded using a memory efficient data structure when they have a small number of entries (`< 3.2` only)
* `redis_list_max_ziplist_value`: [default: `64`]: List are encoded using a memory efficient data structure when the biggest entry does not exceed a given threshold (`< 3.2` only)
* `redis_list_max_ziplist_size`: [default: `-2`]: Lists are also encoded in a special way to save a lot of space. The number of entries allowed per internal list node can be specified as a fixed maximum size or a maximum number of elements (`>= 3.2` only)
* `redis_list_compress_depth`: [default: `0`]: Lists may also be compressed. Compress depth is the number of quicklist ziplist nodes from each side of the list to exclude from compression (`>= 3.2` only)
* `redis_set_max_intset_entries`: [default: `512`]: Sets have a special encoding in just one case: when a set is composed of just strings that happen to be integers in radix 10 in the range of 64 bit signed integers. The following configuration setting sets the limit in the size of the set in order to use this special memory saving encoding
* `redis_zset_max_ziplist_entries`: [default: `128`]: Sorted sets are encoded using a memory efficient data structure when they have a small number of entries
* `redis_zset_max_ziplist_value`: [default: `64`]: Sorted sets are encoded using a memory efficient data structure when the biggest entry does not exceed a given threshold
* `redis_hll_sparse_max_bytes`: [default: `3000`]: HyperLogLog sparse representation bytes limit. The limit includes the 16 bytes header. When an HyperLogLog using the sparse representation crosses this limit (in bytes), it is converted into the dense representation
* `redis_activerehashing`: [default: `true`]: Whether or not to enable activere hashing. use `false` if you have hard latency requirements and it is not a good thing in your environment that the server can reply from time to time to queries with 2 milliseconds delay. use `true` if you don't have such hard requirements but want to free memory asap when possible
* `redis_client_output_buffer_limits`: [default: `[{class: normal, hard_limit: 0, soft_limit: 0, soft_seconds: 0}, {class: slave, hard_limit: 256mb, soft_limit: 64mb, soft_seconds: 60}, {class: pubsub, hard_limit: 32mb, soft_limit: 8mb, soft_seconds: 60}]`]: Client output buffer limits declaration
* `redis_client_output_buffer_limits.{n}.class`: [required]: The class classes of clients (e.g. `normal`, `slave` or `pubsub`)
* `redis_client_output_buffer_limits.{n}.hard_limit`: [required]: A hard limit, forces an immediately disconnect (e.g. `256mb`)
* `redis_client_output_buffer_limits.{n}.soft_limit`: [required]: A soft limit, forces a disconnect when remains reached for `soft_seconds` (e.g. `64mb`)
* `redis_client_output_buffer_limits.{n}.soft_seconds`: [required]: (e.g. `60`)
* `redis_hz`: [default: `10`]: The frequency in which the server calls an internal function to perform many background tasks, like closing connections of clients in timeout, purging expired keys that are never requested, and so forth. The range is between `1` and `500`, however a value over `100` is usually not a good idea. Most users should use the default of `10` and raise this up to `100` only in environments where very low latency is required
* `redis_aof_rewrite_incremental_fsync`: [default: `true`]: When a child rewrites the AOF file, if the following option is enabled the file will be fsync-ed every 32 MB of data generated. This is useful in order to commit the file to the disk more incrementally and avoid big latency spikes
* `redis_lfu_log_factor`: [optional]: (e.g. `10`, `>= 4.0.0` only)
* `redis_lfu_decay_time`: [optional]: (e.g. `1`, `>= 4.0.0` only)
* `redis_active_defrag`: [optional]: Enabled active defragmentation (e.g. `true`, `>= 4.0.0` only)
* `redis_active_defrag_ignore_bytes`: [optional]: Minimum amount of fragmentation waste to start active defrag (e.g. `100mb`, `>= 4.0.0` only)
* `redis_active_defrag_threshold_lower`: [optional]: Minimum percentage of fragmentation to start active defrag (e.g. `10`, `>= 4.0.0` only)
* `redis_active_defrag_threshold_upper`: [optional]: Maximum percentage of fragmentation at which we use maximum effort (e.g. `100`, `>= 4.0.0` only)
* `redis_active_defrag_cycle_min`: [optional]: Minimal effort for defrag in CPU percentage (e.g. `25`, `>= 4.0.0` only)
* `redis_active_defrag_cycle_max`: [optional]: Maximal effort for defrag in CPU percentage (e.g. `75`, `>= 4.0.0` only)

##### Kernel

* `redis_limit_no_file`: [optional]: Call `ulimit -n` with this argument prior to invoking Redis itself (e.g. `65536`)
* `redis_vm_overcommit_memory`: [default: `false`]: Whether or not to set system overcommit policy, [see](http://redis.io/topics/faq#background-saving-is-failing-with-a-fork-error-under-linux-even-if-i39ve-a-lot-of-free-ram). Setting it to `1` is probably what you want
* `redis_transparent_hugepage`: [default: `false`]: Whether or not to disable transparent huge pages, [see](http://redis.io/topics/latency#redis-latency-problems-troubleshooting). Setting it to `never` is probably what you want

#### Dependencies

None

## Recommended

* `rc-local` ([see](https://github.com/Oefenweb/ansible-rc-local), when `redis_vm_overcommit_memory` or `redis_transparent_hugepage` are `false`)
* `sysfs` ([see](https://github.com/Oefenweb/ansible-sysfs), when `redis_vm_overcommit_memory` or `redis_transparent_hugepage` are `false`)

#### Example(s)

##### Single instance

```yaml
---
- hosts: all
  roles:
    - redis
```

##### Multiple instances

```yaml
---
- hosts: all
  roles:
    - role: redis
      redis_instance_name: 'write-through'
      redis_port: 6380

- hosts: all
  roles:
    - role: redis
      redis_instance_name: 'queue'
      redis_port: 6381
```

**Note** that multiple host sections are needed to flush handlers between role calls.

**Note** that mixing install method `ppa` and `src` won't work.

#### License

BSD

#### Author Information

Mischa ter Smitten (based on work of [Benno Joy](https://github.com/bennojoy))

#### Feedback, bug-reports, requests, ...

Are [welcome](https://github.com/Oefenweb/ansible-redis/issues)!
