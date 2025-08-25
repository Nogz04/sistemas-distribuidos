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

## 3. Diferença entre Relógio Físico e Lógico

| Relógio Físico | Relógio Lógico |
|----------------|----------------|
| Mede tempo real (horas:minutos:segundos) | Mede ordem de eventos |
| Pode estar errado ou desincronizado | Não depende de tempo real |
| Útil para timestamp real | Útil para sistemas distribuídos para manter consistência |

---

## 4. Exemplos práticos

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

## 5. Resumo Intuitivo

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
