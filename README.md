# Debug-SQL-Django
## How to show SQL logs in a Django project

Django is an awesome framework to build app quickly. But it may be diffiuclt to debug when things go wrong.
The most efficient way debug data related problem is to debug with sql statement. Here is the method shared by
Peter Coles (https://github.com/mrcoles)

1.Turn debug to true in settings.py , which default to false

```
DEBUG = True
```

2.Add the SQL debug code just after the line executed

```
from django.db import connection; import re
for i, query in enumerate(connection.queries):
    sql = re.split(r'(SELECT|FROM|WHERE|GROUP BY|ORDER BY|INNER JOIN|LIMIT)', query['sql'])
    if not sql[0]: sql = sql[1:]
    sql = [(' ' if i % 2 else '') + x for i, x in enumerate(sql)]
    print('\n### {} ({} seconds)\n\n{};\n'.format(i, query['time'], '\n'.join(sql)))
```

reference: http://mrcoles.com/how-view-django-orm-sql-queries/