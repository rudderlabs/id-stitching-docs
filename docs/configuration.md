# Configuration

This package is configured with [dbt variables](https://docs.getdbt.com/reference/dbt-jinja-functions/var).

Set ID columns in `dbt_project.yml`:

```yaml
vars:
  id-columns: ('user_id', 'anonymous_id', 'email', 'phone', 'salesforce_id')
```

Your data warehouse is searched for tables that include multiple columns defined in `id-columns`. The `id-columns` variable is required. All other variables are optional.

---

## ID Columns

Specify columns that contain IDs.

```yaml
vars:
  id-columns: ('user_id', 'anonymous_id', 'email', 'phone', 'salesforce_id')
```

## Exclude IDs

Exclude specific IDs.

```yaml
vars:
  ids-to-exclude: ('user@company.com', '(555) 555-5555', '12345')
```

## Include Schemas

Include only specific warehouse schemas.

```yaml
vars:
  schemas-to-include: ('marketing_site', 'stripe', 'google_ads')
```

## Exclude Schemas

Exclude specific warehouse schemas.

```yaml
vars:
  schemas-to-exclude: ('test', 'steven')
```

## Include Tables

Include only specific tables.

```yaml
vars:
  tables-to-include: ('tickets', 'leads', 'identifies')
```

## Exclude Tables

Exclude specific tables.

```yaml
vars:
  tables-to-exclude: ('analytics', 'telemetry')
```
