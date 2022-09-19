# Cron Expression

`*` = always. It is a wildcard for every part of the cron schedule expression.

So `* * * * *` means every minute of every hour of every day of every month and every day of the week.

```
 * * * * *  command to execute
 ┬ ┬ ┬ ┬ ┬
 │ │ │ │ │
 │ │ │ │ │
 │ │ │ │ └───── day of week (0 - 7) (0 to 6 are Sunday to Saturday, or use names; 7 is Sunday, the same as 0)
 │ │ │ └────────── month (1 - 12)
 │ │ └─────────────── day of month (1 - 31)
 │ └──────────────────── hour (0 - 23)
 └───────────────────────── min (0 - 59)
 ```
 
 ## Links
 
 - https://serverfault.com/questions/162388/what-does-five-asterisks-in-a-cron-file-mean
