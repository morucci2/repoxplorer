# RepoXplorer

RepoXplorer is small stats and charts utility for GIT repositories.
Its aims purpose is to ease visualization of stats for one or
more projects.

RepoXplorer is based on ElasticSearch and Pecan. Once the service is
started only a web browser is needed to access the user interface.

As lot of projects are composed of multiple sub-projects RepoXplorer let's
you define how a project is composed and then compute stats across multiple
sub-projects.

Furthermore it is possible to define author identities by listing
author' emails and then avoid duplicated author in computed stats.

## How to install

First install repoxplorer in a virtualenv.

```Shell
virtualenv ~/repoxplorer
. ~/repoxplorer/bin/activate
pip install -r requirements.txt
python setup.py install
```

Install Elasticsearch. Here we use an already "ready to use" Docker
container.

```Shell
~/repoxplorer/bin/el-start.sh
```

Start the RepoXplorer web app.

```Shell
uwsgi --http-socket :8080 --pecan ~repoxplorer/local/share/repoxplorer/config.py
```

## Index a GIT project

Edit /usr/local/etc/projects.yaml

```YAML
---
- Barbican:
   - name: barbican
     uri: https://github.com/openstack/barbican
     branch: master
   - name: python-barbicanclient
     uri: https://github.com/openstack/python-barbicanclient
     branch: master
```

Start the GIT indexer

```Shell
python ~repoxplorer/bin/repoxplorer-indexer
```

## Sanitize author identities

In the example below all contributions for John Doe will be stacked if
the author email field of the GIT commit object is one of the defined
emails.

Edit /usr/local/etc/idents.yaml

```YAML
---
- name: John Doe
  emails:
    - john.doe@server
    - jdoe@server
```