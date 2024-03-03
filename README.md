# Users

Manage users and groups on Linux nodes.

**!!Users with uid less than 1000 would not be removed!!**

## Requirements

- [community.posix](https://galaxy.ansible.com/ui/repo/published/ansible/posix/) ansible collection

- [jmespath](https://pypi.org/project/jmespath/) python package

## Role Variables

| Name | Type | Default | Description |
| --- | --- | --- | --- |
| `users_groups` | list | `[]` |  |
| `users_groups[*].name` | str | `""` | **Required**. Name of the group. |
| `users_groups[*].gid` | int | `null` | GID to set for the group. |
| `users_groups[*].system` | bool | `false` | Group will be created as a system group. |
| `users_groups[*].local` | bool | `false` | Forces the use of “local” command alternatives on platforms that implement it. |
| `users_users` | list | `[]` |  |
| `users_users[*].name` | str | `""` | **Required**. Name of the user. |
| `users_users[*].password` | str | `""` | Set the user’s password to the provided encrypted hash. |
| `users_users[*].shell` | str | `/bin/bash` | Set the user’s shell. |
| `users_users[*].uid` | int | `null` | User ID of the user account. |
| `users_users[*].comment` | str | `""` | Description of the user account. |
| `users_users[*].home` | str | `""` | Set the user’s home directory. |
| `users_users[*].expires` | float | `null` | An expiry time for the user in epoch. Can be removed by specifying a negative value. |
| `users_users[*].group` | str | `""` | Sets the user’s primary group. |
| `users_users[*].groups` | list/str | `""` | List of additional user groups. By default, the user is removed from all other groups. Configure `append` to modify this. |
| `users_users[*].append` | bool | `false` | Add the user to the groups specified in `groups`. |
| `users_users[*].create_home` | bool | `true` | Create user's home directory. |
| `users_users[*].update_password` | str | `always` | `always` will update passwords if they differ.<br> `on_create` will only set the password for newly created users. |
| `users_users[*].non_unique` | bool | `false` | Allows to change UID to a non-unique value. |
| `users_users[*].system` | bool | `false` | User will be created as a system account. |
| `users_users[*].local` | bool | `false` | Forces the use of “local” command alternatives on platforms that implement it. |
| `users_users[*].ssh_key` | str | `""` | SSH public key. Can be multiline. |
| `users_users[*].ssh_key_options` | str | `""` | SSH key options. Ignored when `ssh_key` is empty. |
| `users_users[*].ssh_comment` | str | `""` | Rewrite the comment on the publik key. Ignored when `ssh_key` is empty. |
| `users_users[*].ssh_exclusive` | bool | `false` | Remove all other keys from the authorized_keys file. Ignored when `ssh_key` is empty. |
| `users_remove_force` | bool | `false` | Forces removal of the user and associated directories. |
| `users_remove_home` | bool | `false` | Remove directories associated with the user. |

## Example Playbook

```yaml
---
- hosts: all
  vars:
    users_groups:
      - name: test-group-gid
        gid: 5000
      - name: test-group-test
      - name: test-group-local
        local: true
      - name: test-group-system
        system: true
    users:
      - name: test-user-system
        append: true
        comment: "test user for ansible"
        create_home: true
        group: "test-group-gid"
        update_password: always
        system: true
        groups:
          - games
      - name: test-user-local
        update_password: on_create
        local: true
        shell: /bin/zsh
        uid: 2005
        ssh_key: |
          ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDdv1ycIBDsz5tavDqyaG9qRFYcvOzRteOy534MvHDy7BHu/
          ikIOnuUAqT8axjrOxfkosheqTL9wTFIZWRxQJFKgFC7Z8BMAvq1SeU/InPBGJZHBy5LlKz7ZJiH32R1vNjJd4
          T51EXXr9FgdzjPFc4KkgMuMHFXqP/n7CF7MpNO461YernikpCxU4pmDSfEFFR2bsJkA3BH3EMT0TfhfEFeTlX
          +xNPUNGj5kbpoaz43lDTzNNflGHDoR8CcnSMTYNuHQAozecyg6gVsEpavPtvATKBj7rdbHpqhhvBRsA058FunJ
          0exTYyrxP9+z+gu1CErN1UT3vItDI25Ays6PsQxcC2WjBghxaF3MmRClM63xilvw/7km38X8nK03b/+cy3NwyZC
          7/FteW9mPs1wzkSp65Y+dkRLDofAsJASe1qK7M1/uq1fbCzb2USV7R4HgtYvyx8v14iScCCEKhu0Djm+HLrRq9
          Sc1l8IfjTkRsV2pCJe5QiA8PRp+iNBmc1gwDs=
        ssh_key_options: from=10.9.0.1,127.0.0.1
  roles:
    - users
```

## License

[MIT License](LICENSE)

## Author Information

- [RusMephist](https://github.com/RusMephist)
