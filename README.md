# Users

Ansible role for managing users and groups on Linux hosts.

## Requirements

- [community.posix](https://galaxy.ansible.com/ui/repo/published/ansible/posix/) ansible collection

## Role Variables

### users_groups

Create and manage user groups on target hosts.

When a group is removed from this variable, the associated group on the hosts will be deleted.

Each entry in the users_groups list can contain the following keys:

- `name`: The name of the group to create or manage. **Required**.
- `gid`: The group ID number. If not specified, the system assigns the next available GID.
- `system`: Specifies whether the group is a system group. Default: `false`.
- `local`: Forces the use of "local" command alternatives on platforms that implement it. Default: `false`

### users_users

Create and manage users on target hosts.

When a user is removed from this variable, the associated user on the hosts will be deleted.

Each entry in the users_users list can contain the following keys:

- `name`: The username of the account to create or manage. **Required**.
- `password`: The encrypted user password.
- `update_password`: Specifies when to update the password. Can be `always` or `on_create`.
- `shell`: The user's login shell. Default: `/bin/bash`.
- `uid`: The user ID number. If not specified, the system assigns the next available UID.
- `comment`: The GECOS field.
- `home`: Path to the user's home directory.
- `create_home`: Create the home directory if it doesn't exist.
- `expires`: Account expiration date in epoch. Can be removed by specifying a `-1`.
- `group`: User's primary group name.
- `groups`: List of additional groups the user belongs to.
- `append`: Whether to append to the groups list or replace it. Default: `false`.
- `non_unique`: Allow duplicate UIDs. Default: `false`.
- `system`: Create a system account. Default: `false`.
- `local`: Forces the use of "local" command alternatives on platforms that implement it. Default: `false`.
- `ssh_key`: SSH public key to add to authorized_keys. Can be multiline.
- `ssh_key_options`: SSH options for the key.
- `ssh_comment`: A comment for the SSH key.
- `ssh_exclusive`: Remove all other keys from the authorized_keys file.

### users_remove_home

Defines whether the home directory will be deleted when the user is deleted. Default: `false`.

### users_remove_force

Forced deletion of a user, associated directories and groups. Default: `false`.

## Example Playbook

```yaml
- hosts: all
  vars:
    users_groups:
      - name: developers
        gid: 5001
      - name: admins
        gid: 599
        system: true
    users_users:
      - name: alice
        password: "{{ 'Secret123' | password_hash('sha512') }}"
        shell: /bin/zsh
        group: users
        groups: [developers]
        append: true
        update_password: on_create
        ssh_key: ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIADg2n4v733gUH/dC4PZyQlvpFdjQyF8thJwe4mfn8qm alice@example.com
        ssh_key_options: no-port-forwarding,no-agent-forwarding
      - name: bob
        groups: [admins]
        ssh_key: |
          ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDdv1ycIBDsz5tavDqyaG9qRFYcvOzRteOy534MvHDy7BHu/
          ikIOnuUAqT8axjrOxfkosheqTL9wTFIZWRxQJFKgFC7Z8BMAvq1SeU/InPBGJZHBy5LlKz7ZJiH32R1vNjJd4
          T51EXXr9FgdzjPFc4KkgMuMHFXqP/n7CF7MpNO461YernikpCxU4pmDSfEFFR2bsJkA3BH3EMT0TfhfEFeTlX
          +xNPUNGj5kbpoaz43lDTzNNflGHDoR8CcnSMTYNuHQAozecyg6gVsEpavPtvATKBj7rdbHpqhhvBRsA058FunJ
          0exTYyrxP9+z+gu1CErN1UT3vItDI25Ays6PsQxcC2WjBghxaF3MmRClM63xilvw/7km38X8nK03b/+cy3NwyZC
          7/FteW9mPs1wzkSp65Y+dkRLDofAsJASe1qK7M1/uq1fbCzb2USV7R4HgtYvyx8v14iScCCEKhu0Djm+HLrRq9
          Sc1l8IfjTkRsV2pCJe5QiA8PRp+iNBmc1gwDs=
        ssh_comment: bob@example.com
  roles:
    - users
```

## License

[MIT](LICENSE)

## Author Information

Created and maintained by Mikhail Vorontsov (@mephs) <mvorontsov@tuta.io>
