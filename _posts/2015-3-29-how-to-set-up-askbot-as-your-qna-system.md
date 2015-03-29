**NOTE: this assumes you can access PyPI using your own user**

Askbot is a Django so you'll need python. If you want to build your own python, make sure configure it with ssl support. In my case I tried to build python 2.7.8 with ssl support, configure --with-ssl doesn't work, so I have
to hack setup.py to include ssl support, just make sure the include and libs
directory are correctly listed.

## Install Askbot

Assuming your python is installed in directory ~/python/2.7.8

```
    PYTHON=~/python/2.7.8/bin/python

    # install pip and setuptools
    # note this command will install it to /${ROOT}/python/2.7.8/
    ${PYTHON} get-pip.py

    # install virtualenv
    pip install virtualenv

    # create a virtual environment
    ~/python/2.7.8/bin/virtualenv ~/python_virtualenv_for_askbot/

    alias vpyt=~/python_virtualenv_for_askbot/bin/python
    alias vpip=~/python_virtualenv_for_askbot/bin/pip

    # install askbot
    vpip install askbot
```

Initially I had error installing setuptools_hg because pip can't seem to find
it, after some googling the root problem is caused by some dependency issue of
django, the solution is to explicitly install setuptools_hg first:

```
    vpip install setuptools_hg
```

## Install Postgres

I actually already have postgres 9.0.3 installed in ~/, if you don't just follow the postgres website for how to install it. After thtat, you need to install python binding for postgres:
```
    vpip install psycopg2
```

it will fail if pg_config is not known, if so, just do this:

```
    export PATH=$PATH:~/postgresql/9.0.3/bin/
```

to initialise postgres with a working directory /var/pgdata-9.0.3, do this:

```
    mkdir /var/pgdata-9.0.3
    ~/postgresql/9.0.3/bin/initdb -D /var/pgdata-9.0.3
```

Then config postgres by editing __/var/pgdata-9.0.3/postgresql.conf__:

```
wal_level = hot_standby
wal_sync_method = fsync
max_wal_senders = 2
wal_keep_segments = 8
listen_address = '*'
```

Then edit pg_hba.conf to add permissions, refer to postgres document for details.

## Start Postgres
```
~/postgresql/9.0.3/bin/pg_ctl -D /var/gpgdata-9.0.3 -l /var/pgdata-9.0.3/server.log start
```

Create user and passwd
```
~/postgresql/9.0.3/bin/createuser first_user -E -W
```

**NOTE: for some reason this command doesn't set password correctly, I had to run a sql command to reset password for __first_user__ user:**
```
~/postgresql/9.0.3/bin/createdb qna -U first_user

~/postgresql/9.0.3/bin/psql qna -U first_user

ALTER ROLE first_user WITH PASSWORD 'passwd'
```

## Configure Askbot

```
    mkdir /var/qna_askbot
    ../bin/askbot-setup

    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:~/postgresql/9.0.3/lib/
    vpyt manage.py collectstatic

    # initialize database for askbot
    vpyt manage.py syncdb
    vpyt manage.py migrate askbot
    vpyt manage.py migrate django_authopenid
    vpyt manage.py runserver `hostname -i`:8999
```

Then you should be able to test your qna site by connecting via your browser
```
http://127.0.0.1:8999/
```

According to Askbot, the first user created on the site will be admin

