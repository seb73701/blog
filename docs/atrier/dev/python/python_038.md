# Scheduling All Kinds of Recurring Jobs with Python

Let’s explore all the libraries for running cron jobs, deferred tasks, recurring tasks or any other scheduled jobs in Python

With Python there’s always a lot of libraries and options for solving any particular problem and running scheduled or recurring jobs is no exception. Whether you want to run simple deferred task, bunch of scheduled jobs or manage cron tabs, there’s specialized library for that in Python. So, in this article I will give you an overview of all the options available to help you choose the right tool for the task at hand, as well as their use cases, including intro and basic examples to get you started quickly!

![python](https://miro.medium.com/max/700/1*lef4_aZjg8tvMCa7TfkPww.jpeg)

## The Builtin Solution

Before exploring any external libraries, let’s first check what we have in Pythons standard library. Most of the time, Python standard library will contain solution to whatever problem you might have and if the problem is running deferred jobs like with Linux `at` command, then grabbing `sched` module might be the way to go.

`sched` is a very simple module, which can be used to schedule one-off tasks for some specified time - so, it's important to realise, that this is not recurring job (like cron job). It works on all platforms, which might seem obvious, but will not necessarily be the case with all the libraries shown later.

One of the use cases for such deferred task can be scheduled shutdown or if you are’re working with network connections or firewall you can create one-time job to revert changes in case you mess up and lock yourself out of the system.

Enough talking, let’s see an example:

```python
import sched
import threading
import time

scheduler = sched.scheduler(time.time, time.sleep)

def some_deferred_task(name):
    print('Event time:', time.time(), name)

print('Start:', time.time())

now = time.time()
#      delay in seconds -----v  v----- priority
event_1_id = scheduler.enter(2, 2, some_deferred_task, ('first',))
event_2_id = scheduler.enter(2, 1, some_deferred_task, ('second',))  # If first 2 events run at the exact same time, then "second" is ran first
event_3_id = scheduler.enter(5, 1, some_deferred_task, ('third',))

# Start a thread to run the events
t = threading.Thread(target=scheduler.run)
t.start()

# Event has to be canceled in main thread
scheduler.cancel(event_2_id)

# Terminate the thread when tasks finish
t.join()

# Output:
# Start: 1604045721.7161775
# Event time: 1604045723.718353 first
# Event time: 1604045726.7194896 third
```

source : https://gist.github.com/MartinHeinz/cb3eaddb5afd20ee50c6c5c10cd3d134#file-sched-py

The code above defines scheduler, which is used to create (`.enter`) events to be executed at later time. Each event (call to `.enter`) receives 4 arguments, which are - delay in seconds ( in how many seconds will the event happen?), priority, name of the function to be called and optional function arguments. The priority argument doesn't matter most of the time, but can be very important if 2 events are scheduled to happen at exactly the same time, yet they have to be executed sequentially. In that case, the event with highest priority (lowest number) goes first.

In this code snippet we can also see that `.enter` method returns event ID. These IDs can be used to cancel events as demonstrated with `scheduler.cancel(event_2_id)`.

To not block the main thread of the program, we also used `threading.Thread` to start the scheduler and called `.join()` on it to gracefully terminate when it's done with all the tasks.

## Full Power of Crontab
There’s quite a few libraries for running recurring jobs using Python, but let’s start with the one that gives you the full cron “experience”. This library is called `python-crontab` and can be installed with `pip install python-crontab`.

`python-crontab`, unlike other libraries and modules listed here, creates and manages actual real crontabs on Unix systems and tasks on Windows. Therefore, it's not emulating behavior of these operating system tools, but rather leveraging them and using what's already there.

For an example here, let’s see some practical use case. Common reason for running recurring tasks can be checking of status of database server. This can be generally done by connecting to and logging into database and running dummy query like `SELECT 1`, just like so:

```python
from crontab import CronTab

# user=True denotes the current user
cron = CronTab(user=True)
job = cron.new(command='PGPASSWORD=test psql -U someuser -d somedb -c "SELECT 1" -h localhost')
job.setall("*/5 * * * *")

if cron[0].is_valid():  # If syntax is valid, write to crontab
    cron.write()

# crontab -l  # Check real crontab from shell
# */5 * * * * PGPASSWORD=test psql -U someuser -d somedb -c "SELECT 1" -h localhost
```

source : https://gist.github.com/MartinHeinz/c185394b1d3a990dd4740b666fc74eaf#file-python-crontab1-py

As I previously mentioned, `python-crontab` provides the real cron "experience", which includes the generally disliked cron syntax. To set the schedule, one uses `.setall` method to set all the fields. Before setting the schedule however, we need to create the crontab using `CronTab()` and specify the owning user. If `True` is passed in, ID of user executing the program will be used. We also have to create an individual job (`.new()`) in this crontab passing in `command` to be executed and optionally also a `comment`.

When we have the crontab and its job ready we need to write it, but it’s good idea to check its syntax using `.is_valid()` before we do so.

Another basic database admin task is creation of periodic backups, that can be also done easily with `python-crontab`, this time with little different syntax:

```python
with CronTab(user='root') as cron:  # with context manager cron.write() is called automatically
    job = cron.new(
        command='PGPASSWORD=test pg_dump -U someuser -d somedb -h localhost --column-inserts --data-only > backup.sql',
        comment="Perform database backup"
    )
    job.every(2).days()
    job.hour.on(1)

    # job.every_reboot()
    # job.hour.every(10)
    # job.month.during('JAN', 'FEB')  # Powerful but confusing/hard to parse syntax
    # job.minute.during(15, 45).every(5)

# crontab -l
# 0 1 */2 * * PGPASSWORD=test pg_dump -U someuser -d somedb -h localhost --column-inserts --data-only > backup.sql # Perform database backup
```


source : https://gist.github.com/MartinHeinz/4b01007516e564093497cf61d44a34b1#file-python-crontab2-py

If you’re not super comfortable with cron syntax, this library also provides declarative syntax, which is shown in the example above. This syntax is in my opinion very confusing and even harder to read and use than normal cron syntax, so I’d recommend to stick with cron syntax or choose different library (see next section).

Apart from different syntax we can also see usage of Python context manager, which allows us to omit the `.write` method shown previously. One more thing to keep in mind is, that if you decide to run cron jobs as `root` user (not recommended), as shown above, then you will have to run the program with `sudo`.

This library has also other useful features apart from basic creation and management of crontabs. One of them being listing and inspecting both user and system crontabs, as well as lookup based on criteria like command or comment of the specific job:


```python
from crontabs import CronTabs

for cron in CronTabs():  # Get list of all user and system crontabs
    if cron.user:
        print(f'{cron.user} has following cron jobs:')
    else:
        print(f'{cron.filen} has following cron jobs:')
    for job in cron.crons:
        print(f'   {job.command}')

# martin has following cron jobs:
#    PGPASSWORD=test psql -U someuser -d somedb -c "SELECT 1" -h localhost
#    PGPASSWORD=test pg_dump -U someuser -d somedb -h localhost --column-inserts --data-only > backup.sql
# /etc/cron.d/anacron has following cron jobs:
#    [ -x /etc/init.d/anacron ] && if [ ! -d /run/systemd/system ]; then /usr/sbin/invoke-rc.d anacron start >/dev/null; fi
# /etc/cron.d/popularity-contest has following cron jobs:
#    test -x /etc/cron.daily/popularity-contest && /etc/cron.daily/popularity-contest --crond
# ...

jobs = CronTabs().all.find_command('psql')  # lookup for all jobs running specific command

for job in jobs:
    print(job)

# */5 * * * * PGPASSWORD=test psql -U someuser -d somedb -c "SELECT 1" -h localhost
```

source : https://gist.github.com/MartinHeinz/5d284905ebc89b406dfbf65fefb1f5a8#file-python-crontab3-py

As I mentioned in previous section, not all libraries shown here work exactly the same way on all platforms. `python-crontab` works on Linux and Windows, but on Windows only user crontabs (Windows tasks) are supported.

## If You Really Hate Cron Syntax

We’ve seen how to schedule job with declarative syntax with `python-crontab` in previous section, but it wasn't really readable or user friendly. If you're looking for the most user friendly, most popular library with very simple interface, then `schedule` is library for you.

`schedule` is based on an article [Rethinking Cron](https://adam.herokuapp.com/past/2010/4/13/rethinking_cron/) which describes some of the cron problems and weaknesses and this library does a good job at solving them.

The biggest complaint with cron is definitely its terse and hard to write syntax, so let’s see how `schedule` addresses that:

```python
import schedule

def task():
    return ...

def task_with_args(value):
    return ...

schedule.every(2).hours.do(task)
schedule.every().sunday.at("01:00").do(task)
schedule.every().hour.at(":15").do(task)
schedule.every(15).to(30).seconds.do(task)  # Randomly between every 15 to 30 seconds
schedule.every().minute.do(task_with_args, "some value")

# Grouping jobs with tags
schedule.every().day.at("09:00").do(task).tag('daily', 'morning')
schedule.every().day.at("18:30").do(task).tag('daily', 'evening')
schedule.every().hour.do(task).tag('hourly')

schedule.clear('daily-tasks')

# No explicit "month" schedule
# No explicit "range" (during 10-14h; from Jan to Mar) schedule
```

source : https://gist.github.com/MartinHeinz/fab40017b9866d78fec2417ef4981951#file-schedule_recurring-py

The first 5 scheduled jobs above don’t really need much of an explanation. The code is very human-readable a quite self-explanatory. The interface only contains a few function for days (`.monday()`) and times (`.seconds()`, .`hours()`, ...), which makes it very easy to use.

Apart from the simple scheduling, the interface contains also `.tag()` method for grouping the jobs by tag. This can be useful for example for cancelling whole groups of jobs (with `.clear()`).

One downside of having such simple interface is the lack of explicit month or range scheduling, e.g. scheduling jobs during 10–14h or from Jan to Mar isn’t really possible.

Aside from recurring jobs, you can also use `schedule` to run one-off tasks and achieve the same effect as with `sched`, but with nicer syntax:

```pyhton
# Execute one-off (deferred job)
def deferred_job():
    # Do stuff...
    return schedule.CancelJob


schedule.every().day.at('01:00').do(deferred_job)

while True:
    schedule.run_pending()
    time.sleep(1)

# To run in background - https://github.com/mrhwick/schedule/blob/master/schedule/__init__.py#L63
```

source : https://gist.github.com/MartinHeinz/62d21e912730c536a51e0e4d4dd5cd35#file-schedule_oneoff-py

Apart from the deferred job, this code snippet also shows that we need to keep the thread alive for the jobs to run. That’s because this library doesn’t create actual `cron` or `at` jobs. If you don't want to block the main thread of your program like in the example above, you can also run it in background as shown [here](https://github.com/mrhwick/schedule/blob/master/schedule/__init__.py#L63).

## All The Features You Might Ever Need

All the previously mentioned tools have their pros and cons, some specific features and design that makes them good for some specific use cases. If you, however need to run both deferred and periodic jobs, need to store jobs in database, need builtin logging features, etc., then most likely none of the above mentioned tools are going to cut it.


The most feature rich and powerful library for scheduling jobs of any kind in Python is definitely [APScheduler](https://github.com/agronholm/apscheduler), which stands for Advanced Python Scheduler.

It ticks all the boxes, when it comes to features mention above and these kind of features require extensive configuration, so let’s see how APScheduler does it:

```python
jobstores = {
    'mongo': MongoDBJobStore(),  # MongoDBJobStore requires PyMongo installed - `pip install pymongo`
    'default': SQLAlchemyJobStore(url='sqlite:///jobs.sqlite')  # SQLAlchemyJobStore requires SQLAlchemy installed, Recommended to use PostgreSQL
}
executors = {
    'default': ThreadPoolExecutor(20),
    'processpool': ProcessPoolExecutor(5)
}
job_defaults = {
    'coalesce': False,
    'max_instances': 3
}
scheduler = BackgroundScheduler(jobstores=jobstores,
                                executors=executors,
                                job_defaults=job_defaults,
                                timezone=utc,
                                daemon=True)  # Without daemonic mode, the main thread would exit. You can also keep it alive with infinite loop.
```

source : https://gist.github.com/MartinHeinz/e79b8fb071f8429d3c002108ac4de560#file-apscheduler_config-py

This code snippet shows sample configuration, which can be used to setup SQLite and MongoDB job stores, which house the scheduled jobs. It shows configuration of executors which handle running of jobs — here we specify the size of our pools. We also specify some job defaults, such as number of job instances that can run in parallel. All the configs are passed to scheduler, which is used to manage jobs.

```python
def task():
    return ...

# trigger -> can also be 'cron' or 'date'
# misfire_grace_time -> seconds after the designated runtime that the job is still allowed to be run
# max_instances -> max number of concurrent instances
scheduler.add_job(task, trigger='interval', seconds=5, misfire_grace_time=600, max_instances=5)
# 'interval' trigger can take any args from https://apscheduler.readthedocs.io/en/latest/modules/triggers/interval.html#module-apscheduler.triggers.interval

scheduler.add_job(task, trigger='cron', month='jan-apr', day_of_week='mon-fri', hour=15, minute=30, end_date='2021-01-30')
scheduler.add_job(task, CronTrigger.from_crontab('0 17 * * sat,sun'))
# 'cron' trigger can take any args from https://apscheduler.readthedocs.io/en/latest/modules/triggers/cron.html#module-apscheduler.triggers.cron

# Simulates 'at' - deferred jobs
scheduler.add_job(task, trigger='date', run_date=datetime(2020, 12, 24, 17, 30, 0))
# 'date' trigger can take any args from https://apscheduler.readthedocs.io/en/latest/modules/triggers/date.html#module-apscheduler.triggers.date

scheduler.print_jobs(jobstore="default")

scheduler.start()

# Pending jobs:
#     task (trigger: interval[0:01:00], pending)
#     task (trigger: cron[month='jan-apr', day_of_week='mon-fri', hour='15', minute='30'], pending)
#     task (trigger: cron[month='*', day='*', day_of_week='sat,sun', hour='17', minute='0'], pending)
#     task (trigger: date[2020-12-24 17:30:00 UTC], pending)
```

source : https://gist.github.com/MartinHeinz/9fc44b9b537d14cf05f45dbfb30d661b#file-apscheduler_jobs-py

Next comes the creation of our jobs using .add_job() method. It takes quite a few arguments, first of them being function to be ran. Next is the trigger type, which can be interval, cron or date. Interval schedules jobs to run periodically in said interval. Cron is just good old cron-like scheduler, which allows for classic and keyword argument-based scheduling arguments. Finally, date trigger create onetime jobs at specific date and time.

One more important argument to `.add_job()` is `misfire_grace_time`, which provides `anacron`-like feature, or in other words - in case your job doesn't run because scheduler is down, scheduler will try to run it when it's back up, as long as the `misfire_grace_time` hasn't been exceeded.

Scheduled jobs are generally annoying to debug. APScheduler tries to alleviate that with ability to easily configure logging levels as well an ability to add listeners to scheduler events — e.g. when job is executed or when job fails. You can see such listener and log sample log output below:

```python
# Catching scheduler events:
import logging

logging.basicConfig(level=logging.INFO)

def my_listener(event):
    if event.exception:
        logging.warning('Job failed...')
    else:
        logging.info('Job was executed...')


scheduler.add_listener(my_listener, EVENT_JOB_EXECUTED | EVENT_JOB_ERROR)

# ...
# INFO:apscheduler.scheduler:Scheduler started
# INFO:apscheduler.executors.default:Running job "task (trigger: interval[0:00:05], next run at: 2020-10-30 09:51:11 UTC)" (scheduled at 2020-10-30 09:51:11.222311+00:00)
# INFO:apscheduler.executors.default:Job "task (trigger: interval[0:00:05], next run at: 2020-10-30 09:51:16 UTC)" executed successfully
# INFO:root:Job was executed...
```

source : https://gist.github.com/MartinHeinz/08bc035ee1fbd932c099482175718011#file-apscheduler_logging-py

## For The Gevent Users

Last and maybe actually the least (desirable solution) is to use Gevent. Not because Gevent is bad, but because it isn’t really built for running scheduled tasks. If you’re, however already using Gevent in your application, it might make sense to use it to schedule jobs too.

If you aren’t familiar with Gevent, then Gevent is a concurrency library based on coroutines. It uses Greenlets to provide pseudo-concurrency for running multiple tasks in single OS thread. For a better understanding, let’s see a basic example:

```python
# Plain Gevent without scheduler
import gevent
from gevent import monkey

# patches stdlib (including socket and ssl modules) to cooperate with other greenlets
monkey.patch_all()

import requests

# Note that we're using HTTPS, so
# this demonstrates that SSL works.
urls = [
    'https://www.google.com/',
    'https://www.twitter.com/',
    'https://www.python.org/'
]

def head_size(url):
    print(f'Starting {url}')
    data = requests.get(url).content
    print(f'{url}: {len(data)}')

jobs = [gevent.spawn(head_size, _url) for _url in urls]

gevent.wait(jobs)

# Output:
# Starting https://www.google.com/
# Starting https://www.twitter.com/
# Starting https://www.python.org/
# https://www.google.com/: 14381
# https://www.python.org/: 50202
# https://www.twitter.com/: 42702
```

source : https://gist.github.com/MartinHeinz/1513e398416998d52970ca0e90175f8c#file-gevent_example-py

This example shows how we can query multiple URLs in parallel using Gevent and its `gevent.spawn`. In the output above, you can see that all 3 jobs that were created started at the same(-ish) time and returned data later.

To perform the same task, but scheduled in the future, we can do the following:

```python
# deferred job (one-off)
def schedule(delay, func, *args, **kw_args):
    gevent.spawn_later(0, func, *args, **kw_args)
    gevent.spawn_later(delay, schedule, delay, func, *args, **kw_args)

schedule(30, head_size, urls[0])

# periodic job
# this will drift...
def run_regularly(function, interval, *args, **kwargs):
    while True:
        gevent.sleep(interval)
        function(*args, **kwargs)


run_regularly(head_size, 30, urls[0])

# Output:
# Starting https://www.google.com/
# https://www.google.com/: 14449
# 30 sec later...
# Starting https://www.google.com/
# https://www.google.com/: 14435
# ...
```

source : https://gist.github.com/MartinHeinz/7b616cd70722c3d10611c868659bacbb#file-gevent_jobs-py

Above we can see both example for running one-off jobs as well as periodic ones. Both of these solutions are kind of a hack/trick and should only be considered if you’re already using Gevent in your application. It’s also important to mention that above `run_regularly` function will slowly start to drift, even if we account for runtime of tasks. Therefore, you should preferably use `GeventSchedule` available in APScheduler library instead, as it's a more robust solution.

## Conclusion

Running deferred or periodic jobs is very general task and you won’t find one library or tool that does it all perfectly, but I hope this article gave you a decent overview of what is available. Regardless of whichever tool you choose for your particular use case, it’s important to keep in mind the more general best practices, like for example: add comments to your cron jobs for clarity, avoid using `root` user (principle of least privilege), don't put passwords into your crontabs, etc. Also, if you're are going to use actual cron jobs, you might also want to leverage `/etc/cron.daily`, `/etc/cron.weekly` and `/etc/cron.mothly` to keep things organized and tidy. Last but not least, looking into `anacrontab` might also be useful if you need to ensure that your jobs will run even when the machine goes offline for a bit.

Sources :
+ https://martinheinz.dev/blog/39?utm_source=tds&utm_medium=referral&utm_campaign=blog_post_39
+ https://towardsdatascience.com/scheduling-all-kinds-of-recurring-jobs-with-python-b8784c74d5dc