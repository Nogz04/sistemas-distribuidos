# Aula 1 - Sistemas Distribuídos

São sistemas compostos por **múltiplos computadores independentes**, conectados por uma rede, que **cooperam** entre si para realizar tarefas. Cada máquina tem sua própria memória e sistema operacional.

## Sistemas fracamente acoplados - Grid Computacional

> - Geograficamente Distribuídos
> - Hardware e sistema computacional diferentes nas máquinas
> - Esquema de comunicação modelo TCP/IP
> - Programação distribuída
> - Arquitetura de Sistemas
>   - cliente-servidor
>   - ponto a ponto
> - Comunicação -> troca
>   - string -> mensagem
>   - objetos -> serialização
> - Sincronismo
>   - relogio
>   - monitor ou semáforo
>   - ?
> - Tolerância a falhas
---

## Grid Computacional - Sistemas Distribuídos Fracamente Acoplados

O **Grid Computacional** é um tipo de sistema distribuído composto por diversos computadores autônomos, geograficamente distribuídos, heterogêneos (diferentes), e interconectados por redes, com o objetivo de compartilhar recursos computacionais de forma coordenada.

## 🧩 1. Sistemas Fracamente Acoplados

- Os computadores que compõem o grid **não estão fortemente conectados** nem sincronizados constantemente.
- Cada nó pode:
  - Ter seu próprio sistema operacional.
  - Ser administrado independentemente.
  - Pertencer a organizações diferentes.
- São ligados apenas pelo interesse comum de **compartilhar poder computacional**.

---

## 🌍 2. Geograficamente Distribuídos

- Os nós do grid estão localizados em diferentes lugares:
  - Diferentes prédios, cidades ou países.
- Cada local pode ter seus próprios objetivos e políticas de uso.
- Ideal para aplicações que não exigem sincronismo constante entre os nós.

---

## 🧠 3. Hardware e sistemas computacionais diferentes nas máquinas

- As máquinas do grid podem ter:
  - **Arquiteturas diferentes** (Intel, AMD, ARM).
  - **Sistemas operacionais variados** (Linux, Windows, macOS).
- O sistema de grid deve ser capaz de **abstrair essas diferenças** e integrar tudo como um recurso unificado.

---

## 🌐 4. Esquema de comunicação modelo TCP/IP

- A troca de informações entre as máquinas ocorre através da **Internet ou redes locais**, utilizando:
  - **TCP/IP (Transmission Control Protocol / Internet Protocol)**.
- Comunicação baseada em mensagens, arquivos, ou serviços web.
- Menor velocidade e maior latência em comparação a clusters locais (fibra óptica).

**📶 O que é o Modelo TCP/IP?**

> O **modelo TCP/IP** é um conjunto de protocolos que define **como os dados são transmitidos pela rede**, incluindo a Internet. Ele tem 4 camadas principais:

> 1. **Camada de Aplicação** – onde os programas rodam (ex: navegadores, e-mails).
> 2. **Camada de Transporte** – responsável por **entregar os dados corretamente** (ex: TCP ou UDP).
> 3. **Camada de Internet** – define **endereçamento e roteamento** (IP).
> 4. **Camada de Acesso à Rede** – lida com o hardware da rede.

**🔗 Como isso se relaciona com Sockets, IP e Portas?**

| Conceito       | Função                                                  | Exemplo                     |
|----------------|----------------------------------------------------------|-----------------------------|
| **Socket**     | Interface de comunicação usada por programas para trocar dados | `socket(AF_INET, SOCK_STREAM)` |
| **IP**         | Identifica **a máquina** na rede                          | `192.168.0.10`              |
| **Porta lógica** | Identifica **a aplicação ou serviço** na máquina         | `80` para HTTP, `443` para HTTPS |
| **TCP/UDP**    | Define **como os dados são transportados**                | TCP = confiável, UDP = rápido |

**📬 Juntos formam um endereço único na rede:**
```text
[IP]:[Porta] → Exemplo: 192.168.0.10:80
```
---

## 5. 🧠 Programação Distribuída

**Programação distribuída** é o desenvolvimento de software que **roda em múltiplos computadores interconectados**, que trabalham juntos para executar uma tarefa ou um conjunto de tarefas.

### Características:
- Cada parte do programa pode estar executando em uma **máquina diferente**, com comunicação via rede.
- Requer atenção a problemas como:
  - **Sincronização**
  - **Latência de comunicação**
  - **Falhas de rede**
  - **Tolerância a falhas**

### Ferramentas comuns:
- **MPI (Message Passing Interface)**: troca de mensagens entre nós.
- **RMI (Remote Method Invocation)**: invocação de métodos em objetos remotos (Java).
- **gRPC / REST**: comunicação entre serviços via HTTP/2 ou JSON.
- **Middleware**: camada intermediária que abstrai a complexidade da rede (ex: CORBA, Apache Kafka, RabbitMQ).

### Aplicações:
- Simulações científicas.
- Divisão de tarefas complexas em várias máquinas (ex: renderização, análise de dados).
- Ambientes como **grid computacional**, onde várias organizações compartilham recursos.

---

## 6. 🏗️ Arquitetura de Sistemas Distribuídos

Arquitetura é o modelo de como os nós (computadores) **se organizam e se comunicam** em um sistema distribuído. Os dois modelos mais comuns são:

### 🖥️ Arquitetura Cliente-Servidor

- Um ou mais **clientes** fazem requisições a um **servidor**, que processa e responde.
- O servidor é um ponto central que **fornece serviços** (ex: banco de dados, arquivos, APIs).
  
#### Vantagens:
- Estrutura simples e centralizada.
- Fácil de gerenciar e controlar acesso.

#### Desvantagens:
- Ponto único de falha (se o servidor cair, o sistema para).
- Escalabilidade limitada (o servidor pode ficar sobrecarregado).

#### Exemplos:
- Web (navegador cliente + servidor web).
- Aplicações em nuvem.
- Sistemas de login, banco de dados.

---

### 🔄 Arquitetura Ponto a Ponto (P2P)

- Todos os nós têm **função equivalente**: podem ser tanto clientes quanto servidores.
- Os recursos são compartilhados entre os próprios participantes, sem um servidor central.

#### Vantagens:
- **Alta escalabilidade**.
- **Tolerância a falhas** (sem ponto central).
- Uso eficiente de recursos distribuídos.

#### Desvantagens:
- Mais complexidade na organização e segurança.
- Difícil controle central.

#### Exemplos:
- Torrent (BitTorrent).
- Blockchain.
- Computação voluntária (como SETI@home, Folding@home).

---

## 🔄 Comunicação entre Processos

Em sistemas distribuídos, os processos (programas) estão **em máquinas diferentes**, então precisam **se comunicar pela rede** para trabalhar em conjunto.

### 📤 Como os dados são trocados?

#### ➤ Comunicação via Strings (Mensagens)

- O método mais simples: enviar **mensagens de texto** entre processos.
- Exemplo: `"PROCESSO1: tarefa concluída"`.

#### ➤ Comunicação via Objetos (Serialização)

- Quando os dados são mais complexos (listas, objetos, estruturas), é necessário **serializar**.
- **Serialização** é o processo de transformar um objeto em **uma sequência de bytes** que pode ser enviada pela rede.
- No destino, os bytes são **desserializados** para recuperar o objeto original.

📦 **Exemplos de formatos de serialização:**
- **JSON** – fácil de ler e muito usado em APIs.
- **Protocol Buffers (Protobuf)** – mais compacto e eficiente.
- **Pickle (Python)** – serialização de objetos Python.

---

## ⏱️ Sincronismo entre Processos

O sincronismo garante que os processos **coordenem suas ações corretamente**, mesmo que estejam executando **em lugares e tempos diferentes**.

### 🕰️ 1. Relógio (Sincronismo Temporal)

- Cada máquina tem seu **próprio relógio**, que pode estar adiantado ou atrasado.
- Isso causa problemas, por exemplo, quando é necessário saber **qual evento aconteceu primeiro**.

📌 **Soluções:**
- **NTP (Network Time Protocol):** sincroniza os relógios das máquinas com servidores de tempo.
- **Relógios de Lamport:** usa contadores lógicos para ordenar eventos.
- **Relógios vetoriais:** fornece uma noção de **causalidade** entre eventos em processos diferentes.

---

### 🧵 2. Monitor ou Semáforo (Sincronismo de Acesso)

Quando vários processos acessam um mesmo recurso (ex: arquivo ou variável), é preciso **controlar quem acessa e quando**.

#### 🔐 Soluções:

- **Semáforos:** estrutura que **bloqueia ou permite** acesso a uma região crítica.
- **Monitores:** estrutura de alto nível que garante **exclusão mútua automaticamente**, com variáveis internas protegidas.

🛑 **Exemplo prático:**
Imagine dois processos tentando gravar no mesmo arquivo. Sem controle, os dados podem se misturar. Com um semáforo, apenas um processo pode escrever por vez.

---

### 📬 3. Fila de Mensagens (Message Queue)

A **message queue** é uma forma eficiente e segura de **sincronizar e comunicar processos de forma assíncrona**.

#### Como funciona:
- Um processo **produtor** envia uma mensagem para a fila.
- Um processo **consumidor** retira a mensagem da fila quando estiver pronto.
- Eles **não precisam estar online ao mesmo tempo**.

📦 **Exemplos de ferramentas:**
- **RabbitMQ**
- **Apache Kafka**
- **Amazon SQS**

✅ **Benefícios:**
- Permite desacoplamento entre sistemas.
- Ajuda na **tolerância a falhas** (as mensagens ficam salvas até serem processadas).
- Mantém a **ordem de execução**.

---

## 🛡️ Tolerância a Falhas

Em um sistema distribuído, é comum que **algumas partes falhem**. A tolerância a falhas garante que o sistema **continue funcionando**, mesmo que um ou mais componentes parem.

### Estratégias comuns:

#### 🔁 Redundância:
- Ter **múltiplas máquinas** com a mesma função.
- Se uma falhar, outra assume automaticamente.

#### 🧪 Replicação de Dados:
- **Cópias dos dados** são mantidas em locais diferentes.
- Isso evita perda de informações em caso de falhas físicas.

#### 📡 Reenvio e Monitoração:
- Sistemas que falharam podem **reprocessar mensagens** ou **retomar de onde pararam**.
- "Watchdogs" e "heartbeats" verificam se os processos estão vivos.

✅ **Objetivo final:** manter o sistema **disponível, confiável e seguro**, mesmo diante de problemas.

---
