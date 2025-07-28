# Aula 1 - Sistemas Distribuídos

São sistemas compostos por **múltiplos computadores independentes**, conectados por uma rede, que **cooperam** entre si para realizar tarefas. Cada máquina tem sua própria memória e sistema operacional.

## Sistemas fracamente acoplados - Grid Computacional

> - Geograficamente Distribuídos
> - Hardware e sistema computacional diferentes nas máquinas
> - Esquema de comunicação modelo TCP/IP

---

## Grid Computacional - Sistemas Distribuídos Fracamente Acoplados

O **Grid Computacional** é um tipo de sistema distribuído composto por diversos computadores autônomos, geograficamente distribuídos, heterogêneos (diferentes), e interconectados por redes, com o objetivo de compartilhar recursos computacionais de forma coordenada.

## 🧩 Sistemas Fracamente Acoplados

- Os computadores que compõem o grid **não estão fortemente conectados** nem sincronizados constantemente.
- Cada nó pode:
  - Ter seu próprio sistema operacional.
  - Ser administrado independentemente.
  - Pertencer a organizações diferentes.
- São ligados apenas pelo interesse comum de **compartilhar poder computacional**.

---

## 🌍 Geograficamente Distribuídos

- Os nós do grid estão localizados em diferentes lugares:
  - Diferentes prédios, cidades ou países.
- Cada local pode ter seus próprios objetivos e políticas de uso.
- Ideal para aplicações que não exigem sincronismo constante entre os nós.

---

## 🧠 Hardware e sistemas computacionais diferentes nas máquinas

- As máquinas do grid podem ter:
  - **Arquiteturas diferentes** (Intel, AMD, ARM).
  - **Sistemas operacionais variados** (Linux, Windows, macOS).
- O sistema de grid deve ser capaz de **abstrair essas diferenças** e integrar tudo como um recurso unificado.

---

## 🌐 Esquema de comunicação modelo TCP/IP

- A troca de informações entre as máquinas ocorre através da **Internet ou redes locais**, utilizando:
  - **TCP/IP (Transmission Control Protocol / Internet Protocol)**.
- Comunicação baseada em mensagens, arquivos, ou serviços web.
- Menor velocidade e maior latência em comparação a clusters locais (fibra óptica).

---
