# SQL Template
```sql
WITH cohorts AS (
  SELECT user_id,
         DATE_TRUNC('month', first_activity) as cohort_month
  FROM user_activity_summary
),
cohort_data AS (
  SELECT cohort_month,
         DATE_TRUNC('month', activity_date) as activity_month,
         COUNT(DISTINCT user_id) as active_users
  FROM cohorts c
  JOIN user_activities ua ON c.user_id = ua.user_id
  GROUP BY cohort_month, activity_month
)
SELECT * FROM cohort_data
ORDER BY cohort_month, activity_month;
```
