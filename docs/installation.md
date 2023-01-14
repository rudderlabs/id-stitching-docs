# Installation

Check [dbt Hub](https://hub.getdbt.com/rudderlabs/id_stitching/latest/) for the latest version number.

Include the following in your `packages.yml` file:

```yaml
packages:
  - package: rudderlabs/id_stitching
    version: {LATEST_VERSION}
```

Run `dbt deps` to install the package.

For more information on using packages in your dbt project, check out the [dbt documentation](https://docs.getdbt.com/docs/build/packages/). 
