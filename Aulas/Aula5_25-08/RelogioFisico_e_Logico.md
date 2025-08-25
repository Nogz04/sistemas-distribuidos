# Relógios Físicos e Lógicos em Computação

## 1. Relógios Físicos ⏰

### O que são:
Um **relógio físico** é o relógio real de um computador ou dispositivo. Ele mede o tempo real (tempo “do mundo”) usando hardware.

- Medido em segundos, milissegundos, etc.
- Pode ser obtido pelo sistema operacional (ex.: `System.currentTimeMillis()` em Java).

### Problema:
Em sistemas distribuídos, cada máquina tem um relógio físico diferente. Pequenas diferenças podem causar inconsistências, porque não existe um “tempo absoluto” perfeito.

#### Exemplo:
- Computador A registra: evento X ocorreu às 10:00:01  
- Computador B registra: evento Y ocorreu às 09:59:59  

Na realidade, X pode ter acontecido **depois** de Y, mas os relógios físicos estão desincronizados.

---

## 2. Relógios Lógicos ⏰ 🕹️

### O que são:
Um **relógio lógico** não mede tempo real, mas sim a **ordem de eventos**. Ele ajuda a saber qual evento aconteceu **antes ou depois** em sistemas distribuídos, mesmo sem relógio físico sincronizado.

- Criado por Leslie Lamport (1978) → chamado **Relógio de Lamport**.
- Cada processo mantém um contador `L`.
- A cada evento, o relógio lógico do processo aumenta.
- Mensagens enviadas de um processo para outro carregam o valor do relógio lógico.

### Funcionamento básico:
1. Cada processo tem um contador `L`.
2. Evento local: `L = L + 1`
3. Enviando mensagem, inclui `L` na mensagem.
4. Recebendo mensagem:  
   `L = max(L, L_mensagem) + 1`

#### Exemplo numérico:

- Processo A envia mensagem com `L_msg = 2`  
- Processo B tinha `L = 0`  
- Atualiza: `L = max(0, 2) + 1 = 3`  

Agora B sabe que o evento de A (2) aconteceu antes do recebimento (3).

---

## Diferença entre Relógio Físico e Lógico

| Relógio Físico | Relógio Lógico |
|----------------|----------------|
| Mede tempo real (horas:minutos:segundos) | Mede ordem de eventos |
| Pode estar errado ou desincronizado | Não depende de tempo real |
| Útil para timestamp real | Útil para sistemas distribuídos para manter consistência |

---

## Exemplos práticos

### a) Sistema distribuído (bancos)
- Dois caixas eletrônicos processam transações.
- Relógios físicos podem registrar transações fora de ordem.
- Relógios lógicos garantem que **depósito ocorreu antes de saque**, independentemente do relógio físico.

### b) Versionamento de arquivos (Git)
- Cada commit tem timestamp físico.
- Git usa grafo acíclico (DAG) para manter **ordem lógica dos commits**.
- Mesmo se dois commits têm o mesmo timestamp, a ordem lógica é preservada.

### c) Protocolos de redes (Paxos, Raft)
- Relógios lógicos ajudam a eleger líderes e aplicar logs na ordem correta.

---

## No geral:

- **Relógio físico** → mede tempo real, mas pode estar diferente em cada máquina.
- **Relógio lógico** → apenas um contador que cresce com eventos, garantindo **ordem causal** entre processos.
- É normal que o valor de L de um processo ultrapasse outro, desde que a ordem causal seja preservada.

---

## Exemplo de código em JAVA de relógio físico na prática:

```java

class Processo {
    private int id;
    private int L = 0; // Relógio lógico inicial

    public Processo(int id) {
        this.id = id;
    }

    public int eventoLocal() {
        L++; // L = L + 1
        System.out.println("Processo " + id + " evento local. L = " + L); 
        return L;
    }

    public int enviarMensagem() {
        L++; // L = L + 1
        System.out.println("Processo " + id + " envia mensagem. L = " + L); 
        return L; // valor enviado na mensagem
    }

    public void receberMensagem(int L_msg) {
        L = Math.max(L, L_msg) + 1;
        System.out.println("Processo " + id + " recebe mensagem. L atualizado = " + L);
    }
}

public class SimulacaoDistribuida {
    public static void main(String[] args) {
        Processo A = new Processo(1);
        Processo B = new Processo(2);

        // Passo 1: evento local em A
        A.eventoLocal(); 
        // A: L = 1

        // Passo 2: A envia mensagem para B
        int L_msg = A.enviarMensagem(); 
        // A: L = 2 (valor enviado)

        // Passo 3: B recebe mensagem
        B.receberMensagem(L_msg); 
        // B: L = Math.max(0,2)+1 = 3

        // Passo 4: evento local em B
        B.eventoLocal(); 
        // B: L = 4

        // Passo 5: B envia mensagem para A
        int L_msg2 = B.enviarMensagem();
        // B: L = 5 (valor enviado)

        // Passo 6: A recebe mensagem de B
        A.receberMensagem(L_msg2);
        // A: L = Math.max(2,5)+1 = 6

        // Passo 7: evento local em A
        A.eventoLocal(); 
        // A: L = 7
    }
}


```

**Ordem dos valores que foram atribuídos a L (Relógio lógico) de A e B ao decorrer dos eventos**

| Passo | Evento          | Processo | L final |
| ----- | --------------- | -------- | ------- |
| 1     | Evento local    | A        | 1       |
| 2     | Envia mensagem  | A        | 2       |
| 3     | Recebe mensagem | B        | 3       |
| 4     | Evento local    | B        | 4       |
| 5     | Envia mensagem  | B        | 5       |
| 6     | Recebe mensagem | A        | 6       |
| 7     | Evento local    | A        | 7       |

---

## 🚫 3. Exclusão Mútua (Mutual Exclusion)

### O que é:
Exclusão mútua garante que **apenas um processo por vez acesse uma seção crítica** (um recurso compartilhado, como uma impressora ou banco de dados) em sistemas concorrentes ou distribuídos.

### Propriedades importantes:
1. **Mutualidade:** no máximo 1 processo na seção crítica.  
2. **Progresso:** se nenhum processo está na seção crítica, algum processo que queira entrar será autorizado.  
3. **Espera limitada (bounded waiting):** nenhum processo fica esperando para sempre.

### Exemplo Java:

```java
class RecursoCompartilhado {
    public synchronized void acessar(int id) {
        System.out.println("Processo " + id + " entrou na seção crítica");
        try {
            Thread.sleep(1000); // simula operação
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("Processo " + id + " saiu da seção crítica");
    }
}

public class ExclusaoMutuaDemo {
    public static void main(String[] args) {
        RecursoCompartilhado recurso = new RecursoCompartilhado();

        Runnable tarefa1 = () -> recurso.acessar(1);
        Runnable tarefa2 = () -> recurso.acessar(2);

        new Thread(tarefa1).start();
        new Thread(tarefa2).start();
    }
}

```
### Synchronized em Java:

***O que é?*** <br>

`synchronized` é uma palavra-chave do Java usada para **controlar o acesso de múltiplas threads a um recurso compartilhado**.

- Ele garante que **apenas uma thread por vez** execute um bloco de código ou método sincronizado.
- É a maneira mais simples de implementar **exclusão mútua** em Java.


# 🗳️ 4. Eleição de Líder (Leader Election)

## O que é:

Em sistemas distribuídos, às vezes é necessário **eleger um processo como coordenador ou líder** para tomar decisões ou gerenciar recursos.

- Por exemplo, apenas um servidor coordena backups.
- A eleição ocorre quando: <br>
  1 - Nenhum líder existe.<br>
  2 - O líder atual falha.<br>

---

## Algoritmo clássico: Bully Algorithm

1 -  Cada processo tem um **ID único**. <br>
2 - Um processo detecta que o líder morreu e **inicia eleição**.<br>
3 - Ele envia mensagens para todos os processos com **ID maior**.<br>
4 - Se ninguém responde, ele se torna o líder.<br>
5 - Se algum processo maior responde, este processo maior assume a eleição.<br>

---

## Exemplo Java simplificado:

```java
class Processo {
    int id;
    boolean lider = false;

    public Processo(int id) {
        this.id = id;
    }

    public void iniciarEleicao(Processo[] processos) {
        boolean existeMaior = false;
        for (Processo p : processos) {
            if (p.id > this.id) {
                System.out.println("Processo " + this.id + " envia mensagem para " + p.id);
                existeMaior = true;
            }
        }
        if (!existeMaior) {
            lider = true;
            System.out.println("Processo " + id + " é o líder!");
        }
    }
}

public class EleicaoDemo {
    public static void main(String[] args) {
        Processo[] processos = {new Processo(1), new Processo(2), new Processo(3)};

        processos[0].iniciarEleicao(processos); // Processo 1 inicia eleição
    }
}
