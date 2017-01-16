Repo Man
============

A utility for ensuring that a GitHub repository has the correct set of
labels and milestones. 
The owner can create a toml file to configure a list of labels and
milestones, and a list of mappings to rename older labels correctly.

Configuration
--------------

Given the following configuration file:

```toml
repositories = [ "example/fox", "example/wolf" ]

[[label]]
name = "bug"
color = "f29513"
mappings = [ "defect", "error" ]

[[label]]
name = "Jump In"
color = "123456"

[[milestone]]
name = "Accepted Major"
description = "Bucket for breaking changes to pull for next Major"
```

repo_man would create two labels, `bug` and `Jump In`, and would ensure
any existing issues labelled as `defect` or `error` were relabelled as
`bug`. It would then create an `Accepted Major` milestone.

Syntax
------

The config file consists of three arrays, `repositories`, `label` and `milestone`.

`repositories` is an array of `organization`/`repository` names.

A label may have the following keys: `name`; `color`.

A milestone may have the following keys: `name`; `description`; `state`;
and `due_on`. `state` and `due_date` are currently unimplemented.

There is a special key, `mappings`, that accepts an array of existing
labels or milestones that should be renamed to the current one. Renaming
is done by applying the new label and then removing the old one, so it
should be idempotent in the face of failed runs.

Running
--------

RepoMan expects you to have a configured `.netrc` file that contains an
API token for GitHub; see https://gist.github.com/technoweenie/1072829
for discussion. For example, mine contains:

```
machine api.github.com
  login thommay
  password my_api_key_for_github
```

To get the current set of labels and milestones for a repository to a
given file, run:
```
bundle exec bin/dump_labels REPOSITORY FILE
```

To apply a config file to a repository, run
```
bundle exec bin/repo_man -c config.toml
```

