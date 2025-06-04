---
title: Conda配置修改
subtitle:
date: 2025-06-04T15:59:36+08:00
slug: a8661a0
draft: false
author:
  name:
  link:
  email:
  avatar:
description:
keywords:
license:
comment: false
weight: 0
tags:
  - draft
categories:
  - draft
hiddenFromHomePage: false
hiddenFromSearch: false
hiddenFromRelated: false
hiddenFromFeed: false
summary:
resources:
  - name: featured-image
    src: featured-image.jpg
  - name: featured-image-preview
    src: featured-image-preview.jpg
toc: true
math: false
lightgallery: false
password:
message:
repost:
  enable: true
  url:

# See details front matter: https://fixit.lruihao.cn/documentation/content-management/introduction/#front-matter
---

<!--more-->


## 通过conda info 或者 conda config --show 查看自己的conda配置信息

```shell
(base) zzh@GPU-Server-L20:~$ conda info

     active environment : base
    active env location : /opt/anaconda3
            shell level : 1
       user config file : /home/zzh/.condarc
 populated config files : /opt/anaconda3/.condarc
                          /home/zzh/.condarc
          conda version : 24.11.3
    conda-build version : 24.9.0
         python version : 3.12.2.final.0
                 solver : libmamba (default)
       virtual packages : __archspec=1=zen4
                          __conda=24.11.3=0
                          __cuda=12.8=0
                          __glibc=2.35=0
                          __linux=5.15.0=0
                          __unix=0=0
       base environment : /opt/anaconda3  (read only)
      conda av data dir : /opt/anaconda3/etc/conda
  conda av metadata url : None
           channel URLs : https://mirror.lzu.edu.cn/anaconda/cloud/conda-forge/linux-64
                          https://mirror.lzu.edu.cn/anaconda/cloud/conda-forge/noarch
                          https://mirror.lzu.edu.cn/anaconda/pkgs/main/linux-64
                          https://mirror.lzu.edu.cn/anaconda/pkgs/main/noarch
                          https://mirror.lzu.edu.cn/anaconda/pkgs/r/linux-64
                          https://mirror.lzu.edu.cn/anaconda/pkgs/r/noarch
                          https://mirror.lzu.edu.cn/anaconda/pkgs/msys2/linux-64
                          https://mirror.lzu.edu.cn/anaconda/pkgs/msys2/noarch
                          https://repo.anaconda.com/pkgs/main/linux-64
                          https://repo.anaconda.com/pkgs/main/noarch
                          https://repo.anaconda.com/pkgs/r/linux-64
                          https://repo.anaconda.com/pkgs/r/noarch
          package cache : /srv/conda_pkgs
       envs directories : /opt/anaconda3/envs
                          /home/zzh/.conda/envs
               platform : linux-64
             user-agent : conda/24.11.3 requests/2.32.3 CPython/3.12.2 Linux/5.15.0-140-generic ubuntu/22.04.5 glibc/2.35 solver/libmamba conda-libmamba-solver/24.9.0 libmambapy/1.5.8 aau/0.4.4 c/6RGbBPBNDWQHXq6gOb3zcQ s/LjKb7u_F5oo0rEu3feD6-A e/Pbj6KNwzj9T_jvUfEWtz1g
                UID:GID : 1003:1003
             netrc file : None
           offline mode : False

(base) zzh@GPU-Server-L20:~$ conda config --show
add_anaconda_token: True
add_pip_as_python_dependency: True
aggressive_update_packages:
  - ca-certificates
  - certifi
  - openssl
allow_conda_downgrades: False
allow_cycles: True
allow_non_channel_urls: False
allow_softlinks: False
allowlist_channels: []
always_copy: False
always_softlink: False
always_yes: None
anaconda_anon_usage: True
anaconda_upload: None
auto_activate_base: True
auto_stack: 0
auto_update_conda: True
bld_path: 
changeps1: True
channel_alias: https://conda.anaconda.org
channel_priority: strict
channel_settings: []
channels:
  - conda-forge
  - defaults
  - https://repo.anaconda.com/pkgs/main
  - https://repo.anaconda.com/pkgs/r
client_ssl_cert: None
client_ssl_cert_key: None
clobber: False
conda_build: {}
console: classic
create_default_packages: []
croot: /home/zzh/conda-bld
custom_channels:
  anaconda/pkgs/main: https://mirror.lzu.edu.cn
  anaconda/pkgs/r: https://mirror.lzu.edu.cn
  anaconda/pkgs/msys2: https://mirror.lzu.edu.cn
  conda-forge: https://mirror.lzu.edu.cn/anaconda/cloud
  pytorch: https://mirror.lzu.edu.cn/anaconda/cloud
custom_multichannels:
  defaults: 
    - https://mirror.lzu.edu.cn/anaconda/pkgs/main
    - https://mirror.lzu.edu.cn/anaconda/pkgs/r
    - https://mirror.lzu.edu.cn/anaconda/pkgs/msys2
  local: 
debug: False
default_channels:
  - https://mirror.lzu.edu.cn/anaconda/pkgs/main
  - https://mirror.lzu.edu.cn/anaconda/pkgs/r
  - https://mirror.lzu.edu.cn/anaconda/pkgs/msys2
default_python: 3.12
default_threads: None
denylist_channels: []
deps_modifier: not_set
dev: False
disallowed_packages: []
download_only: False
dry_run: False
enable_private_envs: False
env_prompt: ({default_env}) 
envs_dirs:
  - /opt/anaconda3/envs
  - /home/zzh/.conda/envs
envvars_force_uppercase: True
error_upload_url: https://conda.io/conda-post/unexpected-error
execute_threads: 1
experimental: []
extra_safety_checks: False
fetch_threads: 5
force: False
force_32bit: False
force_reinstall: False
force_remove: False
ignore_pinned: False
json: False
local_repodata_ttl: 1
migrated_channel_aliases: []
migrated_custom_channels: {}
no_lock: False
no_plugins: False
non_admin_enabled: True
notify_outdated_conda: True
number_channel_notices: 5
offline: False
override_channels_enabled: True
path_conflict: clobber
pinned_packages: []
pip_interop_enabled: False
pkgs_dirs:
  - /srv/conda_pkgs
proxy_servers: {}
quiet: False
register_envs: True
remote_backoff_factor: 1
remote_connect_timeout_secs: 9.15
remote_max_retries: 3
remote_read_timeout_secs: 60.0
repodata_fns:
  - current_repodata.json
  - repodata.json
repodata_threads: None
repodata_use_zst: True
report_errors: None
restore_free_channel: False
rollback_enabled: True
root_prefix: /opt/anaconda3
safety_checks: warn
sat_solver: pycosat
separate_format_cache: False
shortcuts: True
shortcuts_only: []
show_channel_urls: True
signing_metadata_url_base: None
solver: libmamba
solver_ignore_timestamps: False
ssl_verify: True
subdir: linux-64
subdirs:
  - linux-64
  - noarch
target_prefix_override: 
trace: False
track_features: []
unsatisfiable_hints: True
unsatisfiable_hints_check_depth: 2
update_modifier: update_specs
use_index_cache: False
use_local: False
use_only_tar_bz2: None
verbosity: 0
verify_threads: 1
```

在其中主要修改以下`package cache : /srv/conda_pkgs`，这是conda的包缓存目录，如果你没有这个目录的权限，在下载包时是会报错的.

```
(base) zzh@GPU-Server-L20:~$ conda config --add pkgs_dirs $HOME/.conda/pkgs
(base) zzh@GPU-Server-L20:~$ conda info

     active environment : base
    active env location : /opt/anaconda3
            shell level : 1
       user config file : /home/zzh/.condarc
 populated config files : /opt/anaconda3/.condarc
                          /home/zzh/.condarc
          conda version : 24.11.3
    conda-build version : 24.9.0
         python version : 3.12.2.final.0
                 solver : libmamba (default)
       virtual packages : __archspec=1=zen4
                          __conda=24.11.3=0
                          __cuda=12.8=0
                          __glibc=2.35=0
                          __linux=5.15.0=0
                          __unix=0=0
       base environment : /opt/anaconda3  (read only)
      conda av data dir : /opt/anaconda3/etc/conda
  conda av metadata url : None
           channel URLs : https://mirror.lzu.edu.cn/anaconda/cloud/conda-forge/linux-64
                          https://mirror.lzu.edu.cn/anaconda/cloud/conda-forge/noarch
                          https://mirror.lzu.edu.cn/anaconda/pkgs/main/linux-64
                          https://mirror.lzu.edu.cn/anaconda/pkgs/main/noarch
                          https://mirror.lzu.edu.cn/anaconda/pkgs/r/linux-64
                          https://mirror.lzu.edu.cn/anaconda/pkgs/r/noarch
                          https://mirror.lzu.edu.cn/anaconda/pkgs/msys2/linux-64
                          https://mirror.lzu.edu.cn/anaconda/pkgs/msys2/noarch
                          https://repo.anaconda.com/pkgs/main/linux-64
                          https://repo.anaconda.com/pkgs/main/noarch
                          https://repo.anaconda.com/pkgs/r/linux-64
                          https://repo.anaconda.com/pkgs/r/noarch
          package cache : /home/zzh/.conda/pkgs
                          /srv/conda_pkgs
       envs directories : /opt/anaconda3/envs
                          /home/zzh/.conda/envs
               platform : linux-64
             user-agent : conda/24.11.3 requests/2.32.3 CPython/3.12.2 Linux/5.15.0-140-generic ubuntu/22.04.5 glibc/2.35 solver/libmamba conda-libmamba-solver/24.9.0 libmambapy/1.5.8 aau/0.4.4 c/6RGbBPBNDWQHXq6gOb3zcQ s/YZsa4iE5lOo0aS0DZhjuYA e/Pbj6KNwzj9T_jvUfEWtz1g
                UID:GID : 1003:1003
             netrc file : None
           offline mode : False
```

即可添加成功，这只是其中一种方法，你还可以直接修改conda的配置文件。
```shell
vim ~/.condarc
```