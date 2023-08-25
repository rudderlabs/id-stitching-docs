# Usage

1. Run `queries`, `edges` and `check_edges`.
2. Run `edges` enough times to stitch all IDs. Query `check_edges` to determine if `edges` must be run again.
3. Run `id_graph`.

---

## dbt Core

### Shell Script

Run the included `run.sh` shell script:

```bash
./dbt_packages/id_stitching/run.sh
```

The loop can be edited to run `edges` the necessary number of times.

```bash
#!/bin/sh

dbt run --full-refresh --select queries edges check_edges

for i in {1..5}
do
  dbt run --select edges
done

dbt run --select id_graph
```

### Python Script

A Python script can be used to automatically run `edges` to necessary number of times by evaluating `check_edges` in a while loop expression.

```python
from os import system
from sqlalchemy import create_engine, text


engine = create_engine("dialect+driver://username:password@host:port/database")

system("dbt run --full-refresh --select queries edges check_edges")

with engine.connect() as connection:
    while connection.execute(text("SELECT rows_to_update FROM id_stitching.check_edges")).first()[0]:
        system("dbt run --select edges")

system("dbt run --select id_graph")
```

## dbt Cloud

Create a job with the following commands:

```bash
dbt run --full-refresh --select queries edges check_edges
dbt run --select edges
dbt run --select edges
dbt run --select edges
dbt run --select edges
dbt run --select id_graph
```

Add or remove runs of `edges` using the `check_edges` view.
