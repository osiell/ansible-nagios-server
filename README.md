# Nagios server (including NSCA)

Ansible role to install and configure a Nagios server (with NSCA) on Debian
and Ubuntu platforms.

Nagios is installed from the standard APT repositories, and all the values of
the configuration options defined in this role are the default ones, excepting:

- the `check_external_commands` option is set to `1` to ensure that the NSCA
  server works well;
- the permissions of the `/var/lib/nagios3` and `/var/lib/nagios3/rw`
  directories are changed to allow write access for Apache and NSCA servers;

The Nagios web interface will be available on
[http://SERVER/nagios3/](http://SERVER/nagios3/)

## Supported Platforms

* Debian
    - Wheezy    (7)
    - Jessie    (8)
* Ubuntu
    - Trusty    (14.04)

## Example (Playbook)

Standard installation:

```yaml
- name: Nagios server
  hosts: nagios_server
  sudo: yes
  roles:
    - nagios-server
```

Installation with your configuration stored in one (or several) custom
directory, and an NSCA server password (the NSCA password used in the
*nagios-client* role must be identical):

```yaml
- name: Nagios server
  hosts: nagios_server
  sudo: yes
  roles:
    - nagios-server
  vars:
    - nagios_config_cfg_dirs:
        - /etc/nagios3/objects
    - nsca_config_password: PASSWORD
```

Installation with your configuration cloned from one (or several) Git
repository:

```yaml
- name: Nagios server
  hosts: nagios_server
  sudo: yes
  roles:
    - nagios-server
  vars:
    - nagios_config_cfg_dirs_git:
        - repo: 'https://SERVER/NAGIOS_CONFIG.git'
          dest: '/etc/nagios3/objects'
          version: 'default'
```

The same but from one (or several) Mercurial repository:

```yaml
- name: Nagios server
  hosts: nagios_server
  sudo: yes
  roles:
    - nagios-server
  vars:
    - nagios_config_cfg_dirs_hg:
        - repo: 'https://SERVER/NAGIOS_CONFIG'
          dest: '/etc/nagios3/objects'
          version: 'default'
```

See the *Variables* section below for details.

## Variables

```yaml
# Extra Nagios configuration options
nagios_htpasswd_users: []   # [{name: nagiosadmin, password: password}, {...}]
nagios_config_cfg_dirs_git: []  # [{repo: REPO, dest: DEST, version: VERSION}]
nagios_config_cfg_dirs_hg: []   # [{repo: REPO, dest: DEST, version: VERSION}]

# Standard Nagios configuration options
nagios_config_log_file: /var/log/nagios3/nagios.log
nagios_config_cfg_files:
    - /etc/nagios3/commands.cfg
nagios_config_cfg_dirs:
    - /etc/nagios3/conf.d
nagios_config_object_cache_file: /var/cache/nagios3/objects.cache
nagios_config_precached_object_file: /var/lib/nagios3/objects.precache
nagios_config_resource_file: /etc/nagios3/resource.cfg
nagios_config_status_file: /var/cache/nagios3/status.dat
nagios_config_status_update_interval: 10
nagios_config_check_external_commands: 1    # Nagios default configuration = 0
nagios_config_command_check_interval: -1
nagios_config_command_file: /var/lib/nagios3/rw/nagios.cmd
nagios_config_external_command_buffer_slots: 4096
nagios_config_lock_file: /var/run/nagios3/nagios3.pid
nagios_config_temp_file: /var/cache/nagios3/nagios.tmp
nagios_config_temp_path: /tmp
nagios_config_event_broker_options: -1
nagios_config_log_rotation_method: d
nagios_config_log_archive_path: /var/log/nagios3/archives
nagios_config_use_syslog: 1
nagios_config_log_notifications: 1
nagios_config_log_service_retries: 1
nagios_config_log_host_retries: 1
nagios_config_log_event_handlers: 1
nagios_config_log_initial_states: 0
nagios_config_log_external_commands: 1
nagios_config_log_passive_checks: 1
nagios_config_service_inter_check_delay_method: s
nagios_config_max_service_check_spread: 30
nagios_config_service_interleave_factor: s
nagios_config_host_inter_check_delay_method: s
nagios_config_max_host_check_spread: 30
nagios_config_max_concurrent_checks: 0
nagios_config_check_result_reaper_frequency: 10
nagios_config_max_check_result_reaper_time: 30
nagios_config_check_result_path: /var/lib/nagios3/spool/checkresults
nagios_config_max_check_result_file_age: 3600
nagios_config_cached_host_check_horizon: 15
nagios_config_cached_service_check_horizon: 15
nagios_config_enable_predictive_host_dependency_checks: 1
nagios_config_enable_predictive_service_dependency_checks: 1
nagios_config_soft_state_dependencies: 0
nagios_config_time_change_threshold: 900
nagios_config_auto_reschedule_checks: 0
nagios_config_auto_rescheduling_interval: 30
nagios_config_auto_rescheduling_window: 180
nagios_config_sleep_time: 0.25
nagios_config_service_check_timeout: 60
nagios_config_host_check_timeout: 30
nagios_config_event_handler_timeout: 30
nagios_config_notification_timeout: 30
nagios_config_ocsp_timeout: 5
nagios_config_perfdata_timeout: 5
nagios_config_retain_state_information: 1
nagios_config_state_retention_file: /var/lib/nagios3/retention.dat
nagios_config_retention_update_interval: 60
nagios_config_use_retained_program_state: 1
nagios_config_use_retained_scheduling_info: 1
nagios_config_retained_host_attribute_mask: 0
nagios_config_retained_service_attribute_mask: 0
nagios_config_retained_process_host_attribute_mask: 0
nagios_config_retained_process_service_attribute_mask: 0
nagios_config_retained_contact_host_attribute_mask: 0
nagios_config_retained_contact_service_attribute_mask: 0
nagios_config_interval_length: 60
nagios_config_check_for_updates: 1
nagios_config_bare_update_check: 0
nagios_config_use_aggressive_host_checking: 0
nagios_config_execute_service_checks: 1
nagios_config_accept_passive_service_checks: 1
nagios_config_execute_host_checks: 1
nagios_config_accept_passive_host_checks: 1
nagios_config_enable_notifications: 1
nagios_config_enable_event_handlers: 1
nagios_config_process_performance_data: 0
nagios_config_host_perfdata_file_processing_interval: 0
nagios_config_service_perfdata_file_processing_interval: 0
nagios_config_host_perfdata_file_processing_command: process-host-perfdata-file
nagios_config_service_perfdata_file_processing_command: process-service-perfdata-file
nagios_config_obsess_over_services: 0
nagios_config_ocsp_command: somecommand
nagios_config_obsess_over_hosts: 0
nagios_config_ochp_command: somecommand
nagios_config_translate_passive_host_checks: 0
nagios_config_passive_host_checks_are_soft: 0
nagios_config_check_for_orphaned_services: 1
nagios_config_check_for_orphaned_hosts: 1
nagios_config_check_service_freshness: 1
nagios_config_service_freshness_check_interval: 60
nagios_config_service_check_timeout_state: c
nagios_config_check_host_freshness: 0
nagios_config_host_freshness_check_interval: 60
nagios_config_additional_freshness_latency: 15
nagios_config_enable_flap_detection: 1
nagios_config_low_service_flap_threshold: 5.0
nagios_config_high_service_flap_threshold: 20.0
nagios_config_low_host_flap_threshold: 5.0
nagios_config_high_host_flap_threshold: 20.0
nagios_config_date_format: iso8601
nagios_config_p1_file: /usr/lib/nagios3/p1.pl
nagios_config_enable_embedded_perl: 1
nagios_config_use_embedded_perl_implicitly: 1
nagios_config_illegal_object_name_chars: "`~!$%^&*|'\"<>?,()="
nagios_config_illegal_macro_output_chars: "`~$&|'\"<>"
nagios_config_use_regexp_matching: 0
nagios_config_use_true_regexp_matching: 0
nagios_config_admin_email: root@localhost
nagios_config_admin_pager: pageroot@localhost
nagios_config_daemon_dumps_core: 0
nagios_config_use_large_installation_tweaks: 0
nagios_config_enable_environment_macros: 1
nagios_config_debug_level: 0
nagios_config_debug_verbosity: 1
nagios_config_debug_file: /var/log/nagios3/nagios.debug
nagios_config_max_debug_file_size: 1000000

# Standard NSCA configuration options
nsca_config_log_facility: daemon
nsca_config_check_result_path: False
nsca_config_pid_file: /var/run/nsca.pid
nsca_config_server_port: 5667
nsca_config_server_address: False
nsca_config_nsca_user: nagios
nsca_config_nsca_group: nogroup
nsca_config_nsca_chroot: False
nsca_config_debug: 0
nsca_config_command_file: "{{ nagios_config_command_file }}"
nsca_config_alternate_dump_file: /var/run/nagios/nsca.dump
nsca_config_aggregate_writes: 0
nsca_config_append_to_file: 0
nsca_config_max_packet_age: 30
nsca_config_password: False
nsca_config_decryption_method: 1

# Standard CGI configuration options
nagios_cgi_physical_html_path: /usr/share/nagios3/htdocs
nagios_cgi_url_html_path: /nagios3
nagios_cgi_show_context_help: 1
nagios_cgi_use_pending_states: 1
nagios_cgi_nagios_check_command: /usr/lib/nagios/plugins/check_nagios /var/cache/nagios3/status.dat 5 '/usr/sbin/nagios3'
nagios_cgi_use_authentication: 1
nagios_cgi_use_ssl_authentication: 0
nagios_cgi_default_user_name: False
nagios_cgi_authorized_for_system_information: nagiosadmin
nagios_cgi_authorized_for_configuration_information: nagiosadmin
nagios_cgi_authorized_for_system_commands: nagiosadmin
nagios_cgi_authorized_for_all_services: nagiosadmin
nagios_cgi_authorized_for_all_hosts: nagiosadmin
nagios_cgi_authorized_for_all_service_commands: nagiosadmin
nagios_cgi_authorized_for_all_host_commands: nagiosadmin
nagios_cgi_authorized_for_read_only: False
nagios_cgi_statusmap_background_image: False
nagios_cgi_color_transparency_index_r: 255
nagios_cgi_color_transparency_index_g: 255
nagios_cgi_color_transparency_index_b: 255
nagios_cgi_default_statusmap_layout: 5
nagios_cgi_default_statuswrl_layout: 4
nagios_cgi_statuswrl_include: False
nagios_cgi_ping_syntax: /bin/ping -n -U -c 5 $HOSTADDRESS$
nagios_cgi_refresh_rate: 90
nagios_cgi_result_limit: 100
nagios_cgi_escape_html_tags: 1
nagios_cgi_host_unreachable_sound: False
nagios_cgi_host_down_sound: False
nagios_cgi_service_critical_sound: False
nagios_cgi_service_warning_sound: False
nagios_cgi_service_unknown_sound: False
nagios_cgi_normal_sound: False
nagios_cgi_action_url_target: _blank
nagios_cgi_notes_url_target: _blank
nagios_cgi_lock_author_names: 1
nagios_cgi_enable_splunk_integration: False
nagios_cgi_splunk_url: False

# Apache configuration options
nagios_a2_config: True      # Set to False to not configure Apache
nagios_a2_vhost_file: 00-nagios.conf
nagios_a2_http_port: 80
nagios_a2_https_port: 443
nagios_a2_server_name: "{{ ansible_fqdn }}"
nagios_a2_server_admin: root@localhost
nagios_a2_ssl_cert_file: /etc/ssl/certs/ssl-cert-snakeoil.pem
nagios_a2_ssl_cert_keyfile: /etc/ssl/private/ssl-cert-snakeoil.key
```
