# Task Instructions:
# 1. Write an INTERSECT query:
Write a SQL query using the INTERSECT operator to retrieve the common records between the "students" and "teachers" tables. The result should include the columns course_id, student_id, student_name, teacher_id, and teacher_name for the common courses.
```sql
SELECT * FROM dcistudents
INTERSECT
SELECT * FROM dciteachers
```
# 2. Write an INNER JOIN query:
Write a SQL query using the INNER JOIN operation to retrieve records from both the "students" and "teachers" tables where there is a match based on the common column course_id. Include columns from both tables.
```sql
SELECT *
FROM dcistudents s
INNER JOIN dciteachers t ON s.course_id = t.course_id
```
## check the performance of intersect and inner join using explain and analyze
```sql
"HashSetOp Intersect  (cost=0.00..40.80 rows=200 width=230) (actual time=0.035..0.036 rows=0 loops=1)"
"  ->  Append  (cost=0.00..36.00 rows=640 width=230) (actual time=0.018..0.027 rows=11 loops=1)"
"        ->  Subquery Scan on ""*SELECT* 1""  (cost=0.00..16.40 rows=320 width=230) (actual time=0.018..0.020 rows=7 loops=1)"
"              ->  Seq Scan on dcistudents  (cost=0.00..13.20 rows=320 width=226) (actual time=0.012..0.013 rows=7 loops=1)"
"        ->  Subquery Scan on ""*SELECT* 2""  (cost=0.00..16.40 rows=320 width=230) (actual time=0.005..0.005 rows=4 loops=1)"
"              ->  Seq Scan on dciteachers  (cost=0.00..13.20 rows=320 width=226) (actual time=0.004..0.005 rows=4 loops=1)"
"Planning Time: 0.098 ms"
"Execution Time: 0.068 ms"

"Hash Join  (cost=17.20..49.12 rows=512 width=452) (actual time=0.025..0.029 rows=7 loops=1)"
"  Hash Cond: (s.course_id = t.course_id)"
"  ->  Seq Scan on dcistudents s  (cost=0.00..13.20 rows=320 width=226) (actual time=0.008..0.009 rows=7 loops=1)"
"  ->  Hash  (cost=13.20..13.20 rows=320 width=226) (actual time=0.009..0.010 rows=4 loops=1)"
"        Buckets: 1024  Batches: 1  Memory Usage: 9kB"
"        ->  Seq Scan on dciteachers t  (cost=0.00..13.20 rows=320 width=226) (actual time=0.003..0.004 rows=4 loops=1)"
"Planning Time: 0.078 ms"
"Execution Time: 0.047 ms"
```
# 3.Write an EXCEPT query:
Write a SQL query using the EXCEPT operator to retrieve records from the "students" table that do not exist in the "teachers" table. Include columns student_id, student_name, and course_id.
```sql
SELECT * FROM dcistudents
EXCEPT
SELECT * FROM dciteachers
```
# 4. Write a RIGHT OUTER JOIN query:
Write a SQL query using the RIGHT OUTER JOIN operation to retrieve all records from the "teachers" table and the matching records from the "students" table based on the common column course_id. Include columns from both tables.
```sql
SELECT *
FROM dcistudents s
RIGHT OUTER JOIN dciteachers t ON s.course_id = t.course_id 
```
## check the difference between the result of expect and right outer join as they are not same. 
```sql
"HashSetOp Except  (cost=0.00..40.80 rows=200 width=230) (actual time=0.027..0.028 rows=7 loops=1)"
"  ->  Append  (cost=0.00..36.00 rows=640 width=230) (actual time=0.014..0.021 rows=11 loops=1)"
"        ->  Subquery Scan on ""*SELECT* 1""  (cost=0.00..16.40 rows=320 width=230) (actual time=0.013..0.015 rows=7 loops=1)"
"              ->  Seq Scan on dcistudents  (cost=0.00..13.20 rows=320 width=226) (actual time=0.009..0.010 rows=7 loops=1)"
"        ->  Subquery Scan on ""*SELECT* 2""  (cost=0.00..16.40 rows=320 width=230) (actual time=0.004..0.005 rows=4 loops=1)"
"              ->  Seq Scan on dciteachers  (cost=0.00..13.20 rows=320 width=226) (actual time=0.004..0.004 rows=4 loops=1)"
"Planning Time: 0.073 ms"
"Execution Time: 0.060 ms"


"Hash Left Join  (cost=17.20..49.12 rows=512 width=452) (actual time=0.028..0.031 rows=7 loops=1)"
"  Hash Cond: (t.course_id = s.course_id)"
"  ->  Seq Scan on dciteachers t  (cost=0.00..13.20 rows=320 width=226) (actual time=0.010..0.011 rows=4 loops=1)"
"  ->  Hash  (cost=13.20..13.20 rows=320 width=226) (actual time=0.009..0.009 rows=7 loops=1)"
"        Buckets: 1024  Batches: 1  Memory Usage: 9kB"
"        ->  Seq Scan on dcistudents s  (cost=0.00..13.20 rows=320 width=226) (actual time=0.003..0.004 rows=7 loops=1)"
"Planning Time: 0.095 ms"
"Execution Time: 0.052 ms"
```
