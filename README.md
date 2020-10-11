# Label Approved

Label PRs that have been approved a number of times.

Inspired from [label-when-approved-action](https://github.com/abinoda/label-when-approved-action) but with support for PRs from forks.

## How to use

Install this GitHub action by creating a file in your repo at `.github/workflows/label-approved.yml`.

A minimal example could be:

```YAML
name: Label Approved

on:
  schedule:
    - cron: "0 0 * * *"

jobs:
  label-approved:
    runs-on: ubuntu-latest
    steps:
    - uses: tiangolo/label-approved@0.0.1
      with:
        token: ${{ secrets.GITHUB_TOKEN }}
```

This example uses the defaults configurations.

It will run every night and check all the open PRs, for each PR with the label `awaiting review`, it will check the approvals.

If there are 2 or more approvals, it will remove the label `awaiting review` and will add the label `approved-2`.

## Configuration

You can add different labels to apply.

And for each label, you can specify:

* `number`: the minimum number of approvals.
* `await_label`: a label to filter the PRs. In the example above it is `awaiting review` (the default when no configs are provided).
    * If `await_label` is omitted or `null`, it will apply to all open PRs.

These configs are passed as a JSON object, but as GitHub actions can only take strings as parameters, the JSON object has to be converted to a string.

Check the next example...

## Configuration Example

Here's an example with 3 labels to apply, each with its own config.

It's all inside of a single JSON config, passed as a multiline string.

In YAML (this format) you can use `>` to declare that a string has multiple lines.

```YAML
name: Label Approved

on:
  schedule:
    - cron: "0 0 * * *"

jobs:
  label-approved:
    runs-on: ubuntu-latest
    steps:
    - uses: tiangolo/label-approved@0.0.1
      with:
        token: ${{ secrets.GITHUB_TOKEN }}
        config: >
          {
            "approved-1":
              {
                "number": 1,
                "await_label": "awaiting review"
              },
            "omg 2 approved":
              {
                "number": 2,
                "await_label": "only 2"
              },
            "approvals in 3D":
              {
                "number": 3
              }
          }
```

**Note**: Have in mind that after the `>` the multiline has to have at least one more level of indentation than the key `config` above.

Here's what this config will do:

Check each open PR, and:

* Apply the label `approved-1` to open PRs with:
    * 1 approval (or more).
    * The label `awaiting review` (removing it afterwards).
* Apply the label `omg 2 approved` to open PRs with:
    * 2 approvals (or more).
    * The label `only 2` (removing it afterwards).
* Apply the label `approvals in 3D` to open PRs with:
    * 3 approvals (or more).
    * `await_label` was not declared, so, any open PR will match.

## Release Notes

### Latest Changes


## License

This project is licensed under the terms of the MIT license.
