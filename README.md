# maven_find_version

This ansible module finds artifact version in multiple provided repositories
using python regular expressions and returns it for you to use further in
your role or playbook.

## Use case:
When you can't rely on 'exactness' of artifact version or repo name, provided
to you, but you got list of repos and regex for version (and also you are also
sure that there will be only one matching version in your list of repos)

## Usage:
```yaml
- hosts: localhost
  tasks:
    - name: Find some unknown version
      maven_find_version:
        maven: http://repo.foobar.org/
        group_id: org.foobar.backend
        artifact_name: backend
        version: "^.*-15$"
        repos:
          - releases
          - stage
      register: versions

    - name: Retreive maven artifact using given parameters
      maven_artifact:
        repository_url: "{{ versions.maven }}{{ versions.repo }}"
        artifact_id: "{{ versions.artifact_id }}"
        group_id: "{{ versions.group_id }}"
        version: "{{ versions.version }}"
        extension: jar
        dest: /tmp/job/
```

## Possible improvements:
* HTTP auth
* Actually retreiving artifacts or invoking `maven_artifact` module after finding version
* Better error handling

Anyway, issues and pull requests of any kind are welcome.