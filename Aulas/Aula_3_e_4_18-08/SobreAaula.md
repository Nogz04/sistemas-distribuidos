# Sobre a aula 18-08 - Threads

### Quando usar threads?

> Para realizar diversas tarefas concomitantemente, no caso, ao mesmo tempo.

### Quando não utilizar threads?

> Quando a tarefa é extremamente simples, com baixa complexidade (esforço computacional), e não necessita de threads.

> Quando o ganho de desempenho é mínimo ou quando o custo de gerenciar múltiplas threads excede os benefícios.

> Threads podem causar deadlocks, condições de corrida e liveness bugs se não forem sincronizadas corretamente. Em aplicações simples ou com acesso limitado a recursos compartilhados, a complexidade de implementar mecanismos de sincronização pode não valer a pena.

> Quando não está conseguindo sincronizar as threads e as tarefas que estão sendo gerenciadas por ela utilizam dados sequencias, por exemplo para fazer cálculos que outras tarefas irão utilizar o resultado (produto) da tarefa anterior.

## Perguntas provas

> "Por que não utilizar threads nesse código?".

> Benefícios de usar threads.

> Dificuldades com sincronização.
