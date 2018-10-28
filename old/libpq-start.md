---
title: libpq 起步
date: 2018-07-09 19:08:18
tags:
- 数据库
- PostgreSQL
---

PostgreSQL 的 libpq C 库 起步

头文件 `libpq-fe.h`

采坑：BEGIN了，记得END！

```C++
#include <libpq-fe.h>

#include <iostream>
#include <stdlib.h>
#include <stdio.h>

int main()
{
    const char *conninfo = "host=localhost user=literal dbname=ldb";
    PGconn *conn = nullptr;

    conn = PQconnectdb(conninfo);
    if (PQstatus(conn) != CONNECTION_OK)
    {
        std::cerr<<"Connection to db failed"<<PQerrorMessage(conn);
        PQfinish(conn);
        exit(1);
    }

    PGresult *res = nullptr;

    res = PQexec(conn, "BEGIN");

    if(PQresultStatus(res) != PGRES_COMMAND_OK)
    {
        std::cerr<<"BEGIN command failed:"<<PQerrorMessage(conn);
        PQclear(res);
        PQfinish(conn);
        exit(1);
    }

    PQclear(res);

    char * paramValues[1];
    char str[10];
    paramValues[0] = str;
    for(int i = 10; i < 100; ++i)
    {
        sprintf(paramValues[0], "%d", i);
        res = PQexecParams(conn,
                            "INSERT INFO test VALUES($1, \'program\')",
                            1,
                            NULL,
                            paramValues,
                            NULL,
                            NULL,
                            0);
        ExecStatusType est = PQresultStatus(res);
        std::cout<< PQresStatus(est)<<std::endl;

        PQclear(res);
    }
    res = PQexec(conn, "END");
    PQclear(res);

    PQfinish(conn);

    return 0;
}
```

