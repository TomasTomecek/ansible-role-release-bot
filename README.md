# ansible-role-release-bot

Run [release-bot](https://github.com/user-cont/release-bot) for your project in a podman container.


Requirements
------------

[ansible-bender](https://github.com/TomasTomecek/ansible-bender) needs to be present on your system.


Role Variables
--------------

```yaml
---
# content of the configuration file for release bot
# for more info, see https://github.com/user-cont/release-bot#private-repository
release_bot_conf:
  repository_name: "ansible-role-release-bot"
  repository_owner: "TomasTomecek"
  github_username: ""
  # time in seconds bot should sleep
  refresh_interval: 600

  github_token: ""
  github_app_installation_id: ""
  github_app_id: ""
  # this is the path within the container, you should not change this
  github_app_cert_path: "/github-app.private-key.pem"

# needs to be fedora, we are using the `dnf` module
base_image_name: registry.fedoraproject.org/fedora:29
image_name: release-bot-{{ release_bot_conf.repository_name }}

# working directory
build_dir_path: '{{ playbook_dir }}/build-dir'
# extra arguments to `ansible-bender build`, e.g. `--no-cache`
extra_bender_args: ''

# configuration for the container image build process
image_build:
  # argument for `pip install {{ release_bot_source }}`
  # you can install bot from git master 'git+https://github.com/user-cont/release-bot.git'
  release_bot_source: release-bot
  # a path to your Github app certificate
  github_app_cert_path: ""
  # a path to .pypirc file
  pypirc_path: ""

container_name: release-bot-{{ release_bot_conf.repository_name }}-cont
# a path where the unit should be placed
systemd_units_path: ~/.config/systemd/user/
```

Example Playbook
----------------

```yaml
---
- hosts: localhost
  connection: local
  vars:
    image_build:
      release_bot_source: release-bot
      github_app_cert_path: "path/to/app-key.pem"
      pypirc_path: "path/to/pypirc"
  roles:
  - ansible-role-release-bot
```

License
-------

MIT
