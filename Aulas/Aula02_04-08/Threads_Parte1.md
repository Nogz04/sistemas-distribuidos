# Threads - Parte 1

## Thread Compartilhada:

> A Thread Compartilhada é quando várias threads compartilham os mesmos recursos ou dados (como variáveis globais ou objetos). Quando você diz que uma thread "chama um processo que depende de outro", isso pode estar se referindo a dependências entre threads que compartilham recursos, o que pode levar a condições de corrida e problemas de sincronização.

> Por exemplo, várias threads podem acessar e modificar o mesmo objeto, e se não forem sincronizadas corretamente, pode ocorrer acesso concorrente indevido, o que pode causar falhas no programa.

## Thread Simples:

> Uma Thread Simples é uma thread independente, ou seja, ela não depende de outras threads para realizar seu processo. A execução de cada thread é isolada, e ela não compartilha dados com outras threads.
> - Thread Simples é quando uma única thread executa uma tarefa sem dependência de outras threads. Não há compartilhamento de dados ou recursos entre threads.

## Anotações


# Comandos Utilizados com Threads e Suas Finalidades

## 🧵 **Principais Métodos e Comandos com Threads em Java**

### 1. **`start()`**
   - **Finalidade**: Inicia a execução da thread.
   - **Explicação**: Quando você cria uma instância de `Thread` e chama o método `start()`, a thread é colocada na fila para ser executada. Ela invoca o método `run()` em segundo plano.
   
   **Exemplo**:
   ```java
   new Thread(() -> {
       System.out.println("Thread iniciada!");
   }).start();
  ```

### 2. **`run()`**
   - **Finalidade**: Define o código que será executado pela thread.
   - **Explicação**: O método `run()` contém o código que será executado quando a thread for iniciada com `start()`. Se você sobrescrever esse método em uma subclasse de `Thread` ou ao passar um `Runnable`, o código dentro dele é executado na thread.
   
   **Exemplo**:
   ```java
   public class MinhaThread extends Thread {
       @Override
       public void run() {
           System.out.println("Executando na thread " + Thread.currentThread().getName());
       }
   }
   new MinhaThread().start();
```

### 3. **`sleep(long millis)`**
   - **Finalidade**: Coloca a thread em "pausa" por um determinado tempo (em milissegundos).
   - **Explicação**: Este método faz com que a thread atual durma por um número específico de milissegundos, liberando o processador para outras threads enquanto ela não está em execução. Note que é possível gerar exceções do tipo `InterruptedException` quando uma thread é interrompida durante o sono.

   **Exemplo**:
   ```java
   try {
       Thread.sleep(2000);  // A thread dorme por 2 segundos
   } catch (InterruptedException e) {
       e.printStackTrace();
   }
```

### 4. **`join()`**
   - **Finalidade**: Faz com que a thread principal aguarde a conclusão de outra thread.
   - **Explicação**: O método `join()` bloqueia a execução da thread que o chama até que a thread que o invocou termine sua execução. Útil quando você precisa que a thread principal espere a execução de outras threads antes de continuar.

   **Exemplo**:
   ```java
   Thread t1 = new Thread(() -> {
       System.out.println("Thread 1 executando.");
   });
   t1.start();
   try {
       t1.join();  // Espera a thread t1 terminar antes de continuar
   } catch (InterruptedException e) {
       e.printStackTrace();
   }
   System.out.println("Thread 1 finalizada.");
```

### 5. **`interrupt()`**
   - **Finalidade**: Interrompe a execução de uma thread.
   - **Explicação**: O método `interrupt()` envia um sinal para a thread, indicando que ela deve ser interrompida. Esse método não força imediatamente a thread a parar, mas ela deve verificar periodicamente se foi interrompida e reagir a isso (por exemplo, lançando uma `InterruptedException`).

   **Exemplo**:
   ```java
   Thread t1 = new Thread(() -> {
       try {
           Thread.sleep(5000);
       } catch (InterruptedException e) {
           System.out.println("Thread interrompida.");
       }
   });
   t1.start();
   t1.interrupt();  // Interrompe a thread
```

### 6. **`isAlive()`**
   - **Finalidade**: Verifica se a thread está viva ou ainda está em execução.
   - **Explicação**: Retorna `true` se a thread foi iniciada e não terminou a execução. Se a thread já terminou, retorna `false`.

   **Exemplo**:
   ```java
   Thread t1 = new Thread(() -> {
       // Lógica da thread
   });
   t1.start();

   if (t1.isAlive()) {
       System.out.println("A thread está viva.");
   }
```

### 7. **`currentThread()`**
   - **Finalidade**: Retorna a referência para a thread atualmente em execução.
   - **Explicação**: Esse método retorna a thread que está executando o código onde o método foi chamado. Ele pode ser útil para pegar o nome ou o ID da thread atual.

   **Exemplo**:
   ```java
   Thread t = Thread.currentThread();
   System.out.println("Thread atual: " + t.getName());
  ```

### 8. **`setPriority(int priority)`**
   - **Finalidade**: Define a prioridade de uma thread.
   - **Explicação**: O valor de prioridade pode variar de 1 a 10. As threads de alta prioridade têm mais chances de serem executadas antes das de baixa prioridade, mas isso depende do escalonador de threads do sistema operacional.

   **Exemplo**:
   ```java
   Thread t = new Thread(() -> {
       System.out.println("Thread com prioridade 10.");
   });
   t.setPriority(Thread.MAX_PRIORITY); // Define a prioridade como a máxima
   t.start();
```

### 9. **`yield()`**
   - **Finalidade**: Sugere ao escalonador que a thread atual libere a CPU temporariamente para outras threads.
   - **Explicação**: O método `yield()` é uma sugestão para o escalonador de threads, indicando que a thread está disposta a ceder sua execução, permitindo que outras threads com a mesma ou maior prioridade executem. No entanto, não há garantia de que isso acontecerá.

   **Exemplo**:
   ```java
   public class MinhaThread extends Thread {
       @Override
       public void run() {
           System.out.println("Thread começando...");
           Thread.yield();  // Cede a CPU para outras threads
           System.out.println("Thread continuando...");
       }
   }
   new MinhaThread().start();
```

## 🌟 **Resumo dos Comandos Importantes**

| Método               | Finalidade                                                                |
|----------------------|----------------------------------------------------------------------------|
| **`start()`**         | Inicia a execução de uma nova thread.                                      |
| **`run()`**           | Define o código que será executado pela thread.                            |
| **`sleep(long)`**     | Faz a thread "dormir" por um tempo especificado.                           |
| **`join()`**          | Faz a thread atual aguardar até que a outra thread termine.                |
| **`interrupt()`**     | Interrompe a execução de uma thread (não para imediatamente).              |
| **`isAlive()`**       | Verifica se a thread ainda está em execução.                               |
| **`currentThread()`** | Retorna a referência para a thread que está sendo executada no momento.    |
| **`setPriority()`**   | Define a prioridade da thread (de 1 a 10).                                 |
| **`yield()`**         | Sugere ao escalonador que ceda a CPU para outras threads.                  |


# Programação Concomitante e Não Concomitante

## 🧠 O que é Programação Concomitante?

### ✅ **Programação concomitante** (ou **concorrente**) é:
Quando **várias tarefas são executadas ao "mesmo tempo"**, dividindo o tempo do processador — mesmo que em um único núcleo.

- Exemplo: rodar 3 tarefas simultaneamente, alternando entre elas rapidamente.
- Pode ou não haver **paralelismo físico real** (vários núcleos).

---

### 🔁 Características da programação **concomitante**:

| Característica           | Descrição                                                                 |
|--------------------------|--------------------------------------------------------------------------|
| **Execução intercalada** | O processador alterna entre tarefas rapidamente                          |
| **Threads/processos**    | Usualmente envolve múltiplas threads                                     |
| **Melhor desempenho**    | Usa melhor os recursos, evita que o programa fique "parado" esperando    |
| **Complexidade**         | Mais difícil de programar: requer controle de acesso, sincronização etc. |

---

## 🚫 O que é Programação Não Concomitante?

### ❌ **Programação não concomitante** (ou sequencial) é:
Quando as tarefas são executadas **uma de cada vez, em sequência**, sem alternância ou sobreposição.

- Cada instrução é feita somente depois da anterior.
- Mais simples, mas menos eficiente em tarefas que poderiam ser feitas em paralelo.

---

### 📜 Exemplo prático para ilustrar:

#### ▶️ **Não concomitante (sequencial):**

```java
tarefa1();
tarefa2();
tarefa3();
```

<br>

### 🧵 Concomitante (com threads):

```java
new Thread(() -> tarefa1()).start();
new Thread(() -> tarefa2()).start();
new Thread(() -> tarefa3()).start();
```

- ### Pacote utilizado para utilização de Threads:
> java.util.concurrent


```java
 @author matheus.nogueira

 Criado em: 04/08/2025
```
