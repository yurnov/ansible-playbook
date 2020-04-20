# Ansible Playbook Docker Image

Executes ansible-playbook command against an externally mounted set of Ansible playbooks inspired and forked from [philm/ansible_playbook](https://github.com/philm/ansible_playbook/):

```
docker run --rm -it -v PATH_TO_LOCAL_PLAYBOOKS_DIR:/ansible/playbooks yurnov/ansible-playbook PLAYBOOK_FILE
```

For example, assuming your project's structure follows [best practices](http://docs.ansible.com/ansible/playbooks_best_practices.html#directory-layout), the command to run ansible-playbook from the top-level directory would look like:

```
docker run --rm -it -v $(pwd):/ansible/playbooks yurnov/ansible-playbook playbook.yml
```

Ansible playbook variables can simply be added after the playbook name.

## External folders

Image has folder `/app` for mounting and interacting with any local task. Do not forget to add `-c local`. Example:

```
docker run --rm -it -v $(pwd):/ansible/playbooks -v ~/app:/app yurnov/ansible-playbook playbook.yml -c local
``` 
## SSH Keys

If Ansible is interacting with external machines, you'll need to mount an SSH key pair for the duration of the play:

```
docker run --rm -it \
    -v ~/.ssh/id_rsa:/root/.ssh/id_rsa \
    -v ~/.ssh/id_rsa.pub:/root/.ssh/id_rsa.pub \
    -v $(pwd):/ansible/playbooks \
    yurnov/ansible-playbook playbook.yml
```

## Ansible Vault

If you've encrypted any data using [Ansible Vault](http://docs.ansible.com/ansible/playbooks_vault.html), you can decrypt during a play by either passing **--ask-vault-pass** after the playbook name, or pointing to a password file. For the latter, you can mount an external file:

```
docker run --rm -it -v $(pwd):/ansible/playbooks \
    -v ~/.vault_pass.txt:/root/.vault_pass.txt \
    yurnov/ansible-playbook playbook.yml --vault-password-file /root/.vault_pass.txt
```                    

Note: the Ansible Vault executable is embedded in this image. To use it, specify a different entrypoint:

```
docker run --rm -it -v $(pwd):/ansible/playbooks --entrypoint ansible-vault yurnov/ansible-playbook encrypt FILENAME
```
## Name convention

Two Ansible version maitained, 2.8 (branch `2.8`), and 2.9 (branch `master`), all commit with minor uplitf tagged with same tag, as Ansible version, like `v2.9.6` or `v2.8.10` (latest version in time of this comment)

## Difference against philm/ansible_playbook

* Alpine updated to recent version (3.11.5 in time of latest commit)
* Volume /app added to store local data
* Ansible updated to latest version (2.9.7)
