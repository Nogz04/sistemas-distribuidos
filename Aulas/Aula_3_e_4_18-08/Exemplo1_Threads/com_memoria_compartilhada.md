# Com memória compartilhada

> - Duas ou mais threads acessam a **mesma estrutura de dados** (lista, dicionário, objeto)
> - Necessário o uso de **mecanismo de sincronização** (Java `synchronized`, C# `lock`, Python `threading.Lock`).
> - Mais eficientes em alguns casos, MAS REQUEREM **cuidado com concorrência** (deadlocks, race conditions).

