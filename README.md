Ansible Role for ElasticSearch
==============================

Ansible role for manage [Elasticsearch](www.elasticsearch.org)

Source: [github](https://github.com/echiu64/ansible-elasticsearch)

* Install and configure Elasticsearch;

#### Tested Platforms
* Amazon AMI 2014.09, 2015.03

May work on CentOS 6,7 and RHEL 6,7

#### Known Issues

* proxy support via nginx not supported yet

#### Dependencies

* None

#### Variables

Typical variables for use
```yaml
# Version
elasticsearch_version_major                     # default: 1.5
elasticsearch_version                           # default: "latest" or must fit into version_major, e.g. 1.5.2

# JDK version
elasticsearch_yum_java_pkg                          # java package to use, default: java-1.7.0-openjdk
elasticsearch_java_home                             # default: /usr/java/latest

# JVM tuning and additional options
elasticsearch_heap_size 
elasticsearch_jvm_direct_size 
elasticsearch_java_opts

# Networking
elasticsearch_network_host: 127.0.0.1           # Listen the host
elasticsearch_http_port: 9200                   # Listen the port fot HTTP traffic
elasticsearch_http_cors_enabled: yes            # Enable CORS
elasticsearch_http_cors_allow_origin: "*"       # Set allowed origins
elasticsearch_http_cors_allow_methods: OPTIONS, HEAD, GET, POST, PUT, DELETE
elasticsearch_max_open_files: 65535
elasticsearch_gateway_type: local

# Proxy
lasticsearch_proxy: no                           # Enable nginx as elasticsearch proxy
elasticsearch_proxy_hostname:                     # Listen a hostname (leave empty for skip)
elasticsearch_proxy_port: 80                      # Listen a port
elasticsearch_proxy_auth: no                      # Enable HTTP Auth
elasticsearch_proxy_auth_users: []                # Setup users for HTTP Auth. Example:
                                                  # elasticsearch_proxy_auth_users:
                                                  # - { name: username, password: userpassword }

# Logging
elasticsearch_logging_level                         # Log level or es.logger
elasticsearch_logging_rootlogger                    # Appenders for rootlogger e.g. "console, file"
elasticsearch_logging_indices_recovery              # Log level for indices.recovery logger
elasticsearch_logging_discovery                     # Log level for discovery logger

# Application configuration
# plugins
elasticsearch_plugindir: "{{elasticsearch_home}}/plugins"
elasticsearch_plugins: []                    # Manage elasticsearch plugins (install/remove)
                                             # Ex. elasticsearch_plugins:
                                             #       - name: <plugin name>
                                             #         url: <optional plugin url>
                                             #         dir: <optional plugin dir>
                                             #         remove: yes # Optional the plugin will be removed
                                             #         refresh: true # optional, plugin will be fresh install (remove then install)

# Restart on package upgrade
elasticsearch_restart_on_upgrade                    # set to true if you want to restart on package upgrade 
```

Advanced tuning, see elasticsearch documentation for more information
```yaml
elasticsearch_bootstrap_mlockall:
elasticsearch_cluster_name:
elasticsearch_gateway_expected_nodes:
elasticsearch_gateway_recover_after_nodes:
elasticsearch_gateway_recover_after_time:
elasticsearch_http_enabled:
elasticsearch_http_max_content_length:
elasticsearch_index_mapper_dynamic:
elasticsearch_index_number_of_replicas:
elasticsearch_index_number_of_shards:
elasticsearch_index_query_bool_max_clause_count:
elasticsearch_max_locked_memory:
elasticsearch_network_bind_host:
elasticsearch_network_publish_host:
elasticsearch_node_data:
elasticsearch_node_master:
elasticsearch_node_max_local_storage_nodes:
elasticsearch_node_name:
elasticsearch_node_rack:
elasticsearch_plugin_mandatory:
elasticsearch_recovery_concurrent_streams:
elasticsearch_recovery_max_size_per_sec:
elasticsearch_recovery_node_concurrent_recoveries:
elasticsearch_recovery_node_initial_primaries_recoveries:
elasticsearch_script_disable_dynamic
elasticsearch_script_groovy_sandbox_enabled
elasticsearch_transport_tcp_compress:
elasticsearch_transport_tcp_port:
elasticsearch_use_gc_logging:
```

#### Usage

Add `ansible-elasticsearch` to your roles and setup the variables above in your playbook file

e.g.
```yaml
---
- hosts: tag_Name_EC_TEST
  vars:
    - elasticsearch_java_home: /usr/lib/jvm/jre-1.7.0
    - elasticsearch_http_port: 8443
    - elasticsearch_plugins:
        - name: "elasticsearch/elasticsearch-cloud-aws/2.5.0"
          refresh: true
        - name: "royrusso/elasticsearch-HQ"
          refresh: true
        - name: "elasticsearch/marvel/latest"
          refresh: true
  remote_user: ec2-user
  roles:
    - elasticsearch

```

#### License
Apache 2.0 - see LICENSE file

Portions copyright by Stouts under MIT LICENSE (see LICENSE-MIT)
In particular the files elasticsearch.yml.j2, elasticsearch.j2 and plugins.yml was copied from [Stouts.elasticsearch](https://github.com/Stouts/Stouts.elasticsearch)

