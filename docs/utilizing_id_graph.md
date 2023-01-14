# Utilizing the ID Graph

Our customer Amy's email address is `amy@gmail.com`. We can query `id_graph` to get all of Amy's IDs.

```sql
SELECT *
FROM id_stitching.id_graph
WHERE rudder_id = (
  SELECT rudder_id
  FROM id_stitching.id_graph
  WHERE node = 'amy@gmail.com'
)
```

| rudder_id | node | label|
| --- | --- | --- |
| 48 | amy@gmail.com | email |
| 48 | 203984629341 | user_id |
| 48 | 162e704a-ea35-4076-9f52-50376c25710f | anonymous_id |
| 48 | 5c9b4cb0-c8d4-48f6-8c5c-c58fac94d77f | anonymous_id |
| 48 | 5556667777 | phone |
| 48 | 00530000003xqAb | salesforce_id |
| 48 | cus_4QFOF3xrvB | stripe_id |

Since we know Amy's anonymous IDs, we can join our website pageviews table (`website.pages`) with `id_graph` to see all of the pages that Amy has viewed, including pages viewed before signup or login.

```sql
SELECT
  rudder_id,
  anonymous_id,
  user_id,
  context_traits_email,
  path,
  url,
  referrer,
  timestamp
FROM website.pages AS p
INNER JOIN id_stitching.id_graph AS i
  ON p.anonymous_id = i.node
WHERE i.rudder_id = 48
ORDER BY timestamp
```

| rudder_id | anonymous_id | user_id | context_traits_email | path | url | referrer | timestamp |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 48 | 162e704a-ea35-4076-9f52-50376c25710f | NULL | NULL | / | https://company.com | $direct | 2022-11-18 23:02:35.231+00 |
| 48 | 162e704a-ea35-4076-9f52-50376c25710f | null | null | /signup | https://company.com/signup | $direct | 2022-11-18 23:02:42.432+00 |
| 48 | 162e704a-ea35-4076-9f52-50376c25710f | 203984629341 | amy@gmail.com | /dashboard | https://company.com/dashboard | $direct | 2022-11-18 23:03:12.244+00 |
| 48 | 162e704a-ea35-4076-9f52-50376c25710f | 203984629341 | amy@gmail.com | /settings | https://company.com/settings | $direct | 2022-11-18 23:03:54.173+00 |

Although the first two events contain only `anonymous_id` as an identifier, we can use `id_graph` to attribute them to Amy, enriching our understanding of her user journey.
