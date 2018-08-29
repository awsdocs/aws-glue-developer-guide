# Time\-Based Schedules for Jobs and Crawlers<a name="monitor-data-warehouse-schedule"></a>

You can define a time\-based schedule for your crawlers and jobs in AWS Glue\. The definition of these schedules uses the Unix\-like  [cron](http://en.wikipedia.org/wiki/Cron) syntax\. You specify time in [Coordinated Universal Time \(UTC\)](http://en.wikipedia.org/wiki/Coordinated_Universal_Time), and the minimum precision for a schedule is 5 minutes\.

## Cron Expressions<a name="CronExpressions"></a>

Cron expressions have six required fields, which are separated by white space\. 

**Syntax**

```
cron(Minutes Hours Day-of-month Month Day-of-week Year)
```


| **Fields** | **Values** | **Wildcards** | 
| --- | --- | --- | 
|  Minutes  |  0–59  |  , \- \* /  | 
|  Hours  |  0–23  |  , \- \* /  | 
|  Day\-of\-month  |  1–31  |  , \- \* ? / L W  | 
|  Month  |  1–12 or JAN\-DEC  |  , \- \* /  | 
|  Day\-of\-week  |  1–7 or SUN\-SAT  |  , \- \* ? / L  | 
|  Year  |  1970–2199  |  , \- \* /  | 

**Wildcards**
+ The **,** \(comma\) wildcard includes additional values\. In the `Month` field, `JAN,FEB,MAR` would include January, February, and March\.
+ The **\-** \(dash\) wildcard specifies ranges\. In the `Day` field, 1–15 would include days 1 through 15 of the specified month\.
+ The **\*** \(asterisk\) wildcard includes all values in the field\. In the `Hours` field, **\*** would include every hour\.
+ The **/** \(forward slash\) wildcard specifies increments\. In the `Minutes` field, you could enter **1/10** to specify every 10th minute, starting from the first minute of the hour \(for example, the 11th, 21st, and 31st minute\)\.
+ The **?** \(question mark\) wildcard specifies one or another\. In the `Day-of-month` field you could enter **7**, and if you didn't care what day of the week the seventh was, you could enter **?** in the Day\-of\-week field\.
+ The **L** wildcard in the `Day-of-month` or `Day-of-week` fields specifies the last day of the month or week\.
+ The **W** wildcard in the `Day-of-month` field specifies a weekday\. In the `Day-of-month` field, `3W` specifies the day closest to the third weekday of the month\.

**Limits**
+ You can't specify the `Day-of-month` and `Day-of-week` fields in the same cron expression\. If you specify a value in one of the fields, you must use a **?** \(question mark\) in the other\.
+ Cron expressions that lead to rates faster than 5 minutes are not supported\. 

**Examples**  
When creating a schedule, you can use the following sample cron strings\.


| Minutes | Hours | Day of month | Month | Day of week | Year | Meaning | 
| --- | --- | --- | --- | --- | --- | --- | 
|  0  |  10  |  \*  |  \*  |  ?  |  \*  |  Run at 10:00 am \(UTC\) every day  | 
|  15  |  12  |  \*  |  \*  |  ?  |  \*  |  Run at 12:15 pm \(UTC\) every day  | 
|  0  |  18  |  ?  |  \*  |  MON\-FRI  |  \*  |  Run at 6:00 pm \(UTC\) every Monday through Friday  | 
|  0  |  8  |  1  |  \*  |  ?  |  \*  |  Run at 8:00 am \(UTC\) every first day of the month  | 
|  0/15  |  \*  |  \*  |  \*  |  ?  |  \*  |  Run every 15 minutes  | 
|  0/10  |  \*  |  ?  |  \*  |  MON\-FRI  |  \*  |  Run every 10 minutes Monday through Friday  | 
|  0/5  |  8–17  |  ?  |  \*  |  MON\-FRI  |  \*  |  Run every 5 minutes Monday through Friday between 8:00 am and 5:55 pm \(UTC\)  | 

For example to run on a schedule of every day at 12:15 UTC, specify:

```
cron(15 12 * * ? *)   
```