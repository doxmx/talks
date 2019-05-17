# Pre-Tasks

## Virtualenv

```shell
workon ansible2.7-molecule
```

## ARA

```shell
python -m ara.setup.env
export ANSIBLE_CALLBACK_PLUGINS=/home/luis7238/.virtualenvs/ansible2.7-molecule/lib/python2.7/site-packages/ara/plugins/callbacks
export ANSIBLE_ACTION_PLUGINS=/home/luis7238/.virtualenvs/ansible2.7-molecule/lib/python2.7/site-packages/ara/plugins/actions
ara-manage runserver
```

## Docker

```shell
docker ps
docker stop instance
docker ps -a
docker rm instance
docker images
docker run --rm --privileged -v /:/host solita/ubuntu-systemd setup
```

## Playbooks

```shell
ls -la /home/luis7238/workspace/repos/elastic_stack/roles/elastic_stack-existing/molecule /home/luis7238/workspace/repos/elastic_stack/roles/elastic_stack-existing/.yamllint
rm /home/luis7238/workspace/repos/elastic_stack/roles/elastic_stack-existing/molecule
rm /home/luis7238/workspace/repos/elastic_stack/roles/elastic_stack-existing/.yamllint
```

## Slides
```shell
cd /home/luis7238/workspace/repos/luiscachog.io/source/content/presentations
PATH=$PATH:~/.node_global_modules/bin
/home/luis7238/workspace/repos/luiscachog.io/source/content/presentations

```

# Tasks

```shell
molecule --help
cd /home/luis7238/workspace/repos/elastic_stack/roles/
cd elastic_stack-initial
tree . -a
cd ../elastic_stack-existing
tree . -a
molecule init scenario --role-name elastic_stack-existing
tree . -a
molecule list
cat .yamllint
cd ../elastic_stack-role
tree . -a
cd ../elastic_stack-lint
molecule lint
cd ../elastic_stack
tree . -a
less molecule/default/molecule.yml
less molecule/default/playbook.yml
molecule create
molecule list
molecule converge
molecule list
molecule idempotence
molecule lint
```