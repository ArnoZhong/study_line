## MySQL 5.6 参数设置

### [mysqld]

```ini
;临时表的内存缓存大小
tmp_table_size=0M

max_connections =2100
innodb_buffer_pool_size=4G
innodb_file_per_table=1
innodb_use_sys_malloc =0
innodb_undo_tablespaces=64
innodb_open_files=1024
table_open_cache=1024
innodb_autoextend_increment=128
innodb_max_dirty_pages_pct=90
innodb_log_file_size =128M
innodb_log_buffer_size=16M
innodb_log_files_in_group=8
innodb_flush_log_at_trx_commit=2
enforce-gtid-consistency=true

```



