Ansible Cassandra role
=====

[![Build Status](https://travis-ci.org/deimosfr/ansible-scylladb.svg?branch=master)](https://travis-ci.org/deimosfr/ansible-cassandra)

This role installs and configures Cassandra on a server.

Requirements
------------

This role requires Ansible 2.0 or higher and platform requirements are listed
in the metadata file.

Role Variables
--------------

The variables that can be passed to this role and a brief description about
them are as follows.

```yaml
## Install repos and version
cassandra_version: 3.0
cassandra_apt_repo: "deb http://debian.datastax.com/community {{cassandra_version}} main"
cassandra_apt_key: 'https://debian.datastax.com/debian/repo_key'
cassandra_apt_pkg: 'dsc30'

# service
cassandra_manage_service: true

## Cassandra configuration
# limits
cassandra_manage_limits: true
cassandra_limits:
  memlock: 'unlimited'
  nofile: 100000
  as: 'unlimited'
  nproc: 8096

# sysctl
cassandra_manage_sysctl: true
cassandra_sysctl:
  vm.max_map_count: 131072

# defaults
cassandra_server_defaults:

# main config
cassandra_main_config:
  cluster_name: 'Test Cluster'
  num_tokens: 256
  hinted_handoff_enabled: true
  max_hint_window_in_ms: 10800000 # 3 hours
  hinted_handoff_throttle_in_kb: 1024
  max_hints_delivery_threads: 2
  hints_flush_period_in_ms: 10000
  max_hints_file_size_in_mb: 128
  batchlog_replay_throttle_in_kb: 1024
  authenticator: AllowAllAuthenticator
  authorizer: AllowAllAuthorizer
  role_manager: CassandraRoleManager
  roles_validity_in_ms: 2000
  permissions_validity_in_ms: 2000
  partitioner: org.apache.cassandra.dht.Murmur3Partitioner
  data_file_directories:
    - /var/lib/cassandra/data
  commitlog_directory: /var/lib/cassandra/commitlog
  disk_failure_policy: stop
  commit_failure_policy: stop
  key_cache_size_in_mb:
  key_cache_save_period: 14400
  row_cache_size_in_mb: 0
  row_cache_save_period: 0
  counter_cache_size_in_mb:
  counter_cache_save_period: 7200
  saved_caches_directory: /var/lib/cassandra/saved_caches
  commitlog_sync: periodic
  commitlog_sync_period_in_ms: 10000
  commitlog_segment_size_in_mb: 32
  seed_provider:
    - class_name: org.apache.cassandra.locator.SimpleSeedProvider
      parameters:
        - seeds: "{{ansible_default_ipv4.address}}"
  concurrent_reads: 32
  concurrent_writes: 32
  concurrent_counter_writes: 32
  concurrent_materialized_view_writes: 32
  memtable_allocation_type: heap_buffers
  index_summary_capacity_in_mb:
  index_summary_resize_interval_in_minutes: 60
  trickle_fsync: false
  trickle_fsync_interval_in_kb: 10240
  storage_port: 7000
  ssl_storage_port: 7001
  listen_address: localhost
  start_native_transport: true
  native_transport_port: 9042
  start_rpc: false
  rpc_address: localhost
  rpc_port: 9160
  rpc_keepalive: true
  rpc_server_type: sync
  thrift_framed_transport_size_in_mb: 15
  incremental_backups: false
  snapshot_before_compaction: false
  auto_snapshot: true
  tombstone_warn_threshold: 1000
  tombstone_failure_threshold: 100000
  column_index_size_in_kb: 64
  batch_size_warn_threshold_in_kb: 5
  batch_size_fail_threshold_in_kb: 50
  compaction_throughput_mb_per_sec: 16
  compaction_large_partition_warning_threshold_mb: 100
  sstable_preemptive_open_interval_in_mb: 50
  read_request_timeout_in_ms: 5000
  range_request_timeout_in_ms: 10000
  write_request_timeout_in_ms: 2000
  counter_write_request_timeout_in_ms: 5000
  cas_contention_timeout_in_ms: 1000
  truncate_request_timeout_in_ms: 60000
  request_timeout_in_ms: 10000
  cross_node_timeout: false
  endpoint_snitch: SimpleSnitch
  dynamic_snitch_reset_interval_in_ms: 600000
  dynamic_snitch_badness_threshold: 0.1
  request_scheduler: org.apache.cassandra.scheduler.NoScheduler
  server_encryption_options:
    internode_encryption: none
    keystore: conf/.keystore
    keystore_password: cassandra
    truststore: conf/.truststore
    truststore_password: cassandra
  client_encryption_options:
    enabled: false
    optional: false
    keystore: conf/.keystore
    keystore_password: cassandra
  internode_compression: all
  inter_dc_tcp_nodelay: false
  tracetype_query_ttl: 86400
  tracetype_repair_ttl: 604800
  gc_warn_threshold_in_ms: 1000
  enable_user_defined_functions: false
  enable_scripted_user_defined_functions: false
  windows_timer_interval: 1

# rack dc
cassandra_rack_dc:
  dc: 'dc1'
  rack: 'rack1'

# topology
cassandra_topology_properties:
  default: 'DC1:r1'

# commitlog archiving properties
cassandra_commitlog_arch_properties:
  archive_command: ''
  restore_command: ''
  restore_directories: ''
  restore_point_in_time: ''
  precision: 'MICROSECONDS'

cassandra_kvm_options:
  - '-XX:+UseParNewGC'
  - '-XX:+UseConcMarkSweepGC'
  - '-XX:+CMSParallelRemarkEnabled'
  - '-XX:SurvivorRatio=8'
  - '-XX:MaxTenuringThreshold=1'
  - '-XX:CMSInitiatingOccupancyFraction=75'
  - '-XX:+UseCMSInitiatingOccupancyOnly'
  - '-XX:CMSWaitDuration=10000'
  - '-XX:+CMSParallelInitialMarkEnabled'
  - '-XX:+CMSEdenChunksRecordAlways'
  - '-XX:+CMSClassUnloadingEnabled'
  - '-XX:+PrintGCDetails'
  - '-XX:+PrintGCDateStamps'
  - '-XX:+PrintHeapAtGC'
  - '-XX:+PrintTenuringDistribution'
  - '-XX:+PrintGCApplicationStoppedTime'
  - '-XX:+PrintPromotionFailure'
  - '-XX:+UseGCLogFileRotation'
  - '-XX:NumberOfGCLogFiles=10'
  - '-XX:GCLogFileSize=10M'
```

Examples
========

```yaml
# Roles
- name: cassandra
  hosts: cassandra
  user: root
  roles:
    - deimosfr.cassandra
```

Dependencies
------------

None

License
-------

GPL

Author Information
------------------

Pierre Mavro / deimosfr
