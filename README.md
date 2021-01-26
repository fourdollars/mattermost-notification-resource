 [![GitHub: fourdollars/mattermost-notification-resource](https://img.shields.io/badge/GitHub-fourdollars%2Fmattermostmattermost%E2%80%90notification%E2%80%90resource-lightgray.svg)](https://github.com/fourdollars/mattermost-notification-resource/) [![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT) [![Bash](https://img.shields.io/badge/Language-Bash-red.svg)](https://www.gnu.org/software/bash/) ![Docker](https://github.com/fourdollars/mattermost-notification-resource/workflows/Docker/badge.svg) [![Docker Pulls](https://img.shields.io/docker/pulls/fourdollars/mattermost-notification-resource.svg)](https://hub.docker.com/r/fourdollars/mattermost-notification-resource/)
# mattermost-notification-resource
[concourse-ci](https://concourse-ci.org/)'s mattermost-notification-resource
https://docs.mattermost.com/developer/webhooks-incoming.html

## Config 

### Resource Type

```yaml
resource_types:
- name: resource-mattermost-notification
  type: registry-image
  source:
    repository: fourdollars/mattermost-notification-resource
    tag: latest
```

or

```yaml
resource_types:
- name: resource-mattermost-notification
  type: registry-image
  source:
    repository: ghcr.io/fourdollars/mattermost-notification-resource
    tag: latest
```

### Resource

* webhook: **required**

```yaml
resources:
- name: notification
  type: resource-mattermost-notification
  icon: bell
  check_every: 87600h
  source:
    webhook: http://{your-mattermost-site}/hooks/xxx-generatedkey-xxx
```

### Example

* message: **required** if path is not provided.
* path: **required** if message is not provided.

```yaml
jobs:
- name: hello-world
  plan:
  - task: send-message
    config:
      platform: linux
      image_resource:
        type: registry-image
        source:
          repository: busybox
      outputs:
        - name: output
      run:
        path: sh
        args:
        - -exc
        - |
          echo "Hello World" > output/message.log
  - put: notification
    params:
      path: output
  - put: notification
    params:
      message: "Hi, here."
```
