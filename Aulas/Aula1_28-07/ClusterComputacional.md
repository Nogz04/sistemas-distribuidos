# Aula 1 - Sistemas Paralelos
 
## Sistemas FORTEMENTE acoplados - Cluster Computacional

> - Geograficamente na mesma localização
> - Hardware igual para todas as maquinas
> - Esquema de comunicação via fibra óptica
> - Sistema de nobreak robusto
> - Sistema de backup
> - Programação paralela


## Cluster Computacional - Características e Componentes

Um **cluster computacional** é um conjunto de computadores interligados que funcionam como uma única unidade de processamento. Abaixo estão os principais elementos citados em aula, com suas respectivas funções no contexto de clusters de alto desempenho (HPC).


## 📍 1. Geograficamente na mesma localização

- Todos os nós (computadores) do cluster estão fisicamente próximos.
- Reduz a latência de comunicação entre as máquinas.
- Facilita a manutenção e o gerenciamento do sistema.
- Exemplo: todos os nós em uma mesma sala ou data center.

---

## 💻 2. Hardware e sistema operacional igual para todas as máquinas

- Máquinas com especificações idênticas (CPU, RAM, disco).
- Garante uniformidade de desempenho.
- Facilita a divisão de tarefas em paralelo.
- Evita gargalos por desempenho desigual entre os nós.

---

## 🌐 3. Esquema de comunicação via fibra óptica

- Conexão entre nós usando **fibra óptica**:
  - Alta velocidade de transmissão.
  - Baixa latência.
  - Alta confiabilidade.
- Fundamental para tarefas que exigem troca rápida de dados entre os nós.

---

## 🔋 4. Sistema de nobreak robusto

- Garante funcionamento contínuo mesmo em caso de queda de energia.
- Evita:
  - Perda de dados.
  - Danos ao hardware.
  - Interrupção de tarefas em execução.
- Pode ser combinado com **geradores de energia** para maior autonomia.

---

## 💾 5. Sistema de backup

- Protege contra perda de dados e falhas críticas.
- Tipos comuns de backup:
  - Cópias regulares de dados dos usuários.
  - Backup de configurações do sistema.
  - Armazenamento em dispositivos externos ou na nuvem.
- Essencial para recuperação de desastres.

---

## 🧠 6. Programação paralela

- Permite dividir tarefas entre múltiplos nós do cluster.
- Aumenta a eficiência no processamento de grandes volumes de dados.
- Tecnologias utilizadas:
  - **MPI (Message Passing Interface)**: comunicação entre nós distintos.
  - **OpenMP**: paralelismo em múltiplos núcleos da mesma máquina.
  - **CUDA**: para processamento paralelo com GPUs.

---
