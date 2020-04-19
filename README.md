# artis3n.tailscale

This role initializes a [Tailscale][] node.

## Requirements

You must supply a `tailscale_auth_key` variable, which can be generated under your Tailscale account at <https://login.tailscale.com/admin/authkeys>.

## Role Variables

### tailscale_auth_key

**Required**

An [ansible-vault encrypted variable][ansible-vault] containing a Tailscale Node Authorization auth key.

A Node Authorization auth key can be generated under your Tailscale account at <https://login.tailscale.com/admin/authkeys>.

### release_stability

**Default**: `stable`

Whether to use the Tailscale stable or unstable track.

Stable:

> Stable releases. If you're not sure which track to use, pick this one.

Unstable:

> The bleeding edge. Pushed early and often. Expect rough edges!

## Dependencies

None

## Example Playbook

You **must** include the `tailscale_auth_key` variable.
We cannot force you to use an [encrypted variable][ansible-vault], but please use an encrypted variable.

`--authkey` is not currently on the Tailscale stable branch.
Thus, you must specify `release_stablility: unstable` to use this role.
Once Tailscale releases a stable build with `authkey`, this will be updated.

```yaml
- name: Servers
  hosts: all
  roles:
    - role: artis3n.tailscale
      vars:
        release_stability: unstable
        # Fake example encrypted by ansible-vault
        tailscale_auth_key: !vault |
          $ANSIBLE_VAULT;1.2;AES256;tailscale
          32616238303134343065613038383933333733383765653166346564363332343761653761646363
          6637666565626333333664363739613366363461313063640a613330393062323161636235383936
          37373734653036613133613533376139383138613164323661386362376335316364653037353631
          6539646561373535610a643334396234396332376431326565383432626232383131303131363362
          3537
```

## License

MIT

## Author Information

Ari Kalfus ([@artis3n](https://www.artis3nal.com/)) <dev@arti3nal.com>

## Development and Contributing

This GitHub repository uses a dedicated "test" Tailscale account to authenticate Tailscale during CI runs.
Each Docker container creates a new authorized machine in that test account.
The machines are manually cleaned up every so often.

If you are interested in contributing to this repository, you must create a [Tailscale account][] and generate a [Node Authorization auth key][auth key].

Then, choose a password to encrypt with.
Write the password in a `.ci-vault-pass` file at the project root.

Then, run the following Ansible command to encrypt the auth key:

```bash
ansible-vault encrypt_string --vault-id tailscale@.ci-vault-pass '[AUTH KEY VALUE HERE]' --name 'tailscale_auth_key'
```

This will generate an encrypted string for you to set in the `molecule/default/converge.yml` playbook.

[ansible-vault]: https://docs.ansible.com/ansible/latest/user_guide/vault.html#encrypt-string-for-use-in-yaml
[auth key]: https://login.tailscale.com/admin/authkeys
[tailscale]: https://tailscale.com/
[tailscale account]: https://login.tailscale.com/start