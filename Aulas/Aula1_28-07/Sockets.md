# Aula 1 - Sockets

# Sockets TCP/IP: 

No coração de praticamente toda a comunicação que acontece na internet, desde o envio de um simples e-mail até a complexa transmissão de vídeos em tempo real, encontram-se os **sockets TCP/IP**. Eles são a interface de programação fundamental que permite que programas em diferentes computadores se comuniquem através de uma rede. Em essência, um socket funciona como um ponto de extremidade em uma conversa bidirecional, permitindo que dados sejam enviados e recebidos de forma confiável.

## O que é um Socket?

Imagine que a rede de computadores é um grande sistema postal. Para que duas pessoas possam trocar cartas, cada uma precisa de uma caixa de correio com um endereço único. Um socket, nesse cenário, é análogo a essa caixa de correio. Ele é identificado por uma combinação única de um **endereço IP** e um **número de porta**.

* **Endereço IP:** Identifica um computador específico na rede.
* **Número da Porta:** Identifica um processo ou serviço específico rodando naquele computador. Por exemplo, servidores web normalmente usam a porta 80 para o protocolo HTTP e a 443 para o HTTPS.

Ao utilizar essa combinação, os dados podem ser direcionados para a aplicação correta no computador de destino, garantindo que uma solicitação de página web chegue ao servidor web e não ao servidor de e-mail, por exemplo.

## O Papel do TCP/IP

Os sockets operam sobre o conjunto de protocolos TCP/IP (Transmission Control Protocol/Internet Protocol), que é a base da internet.

* **IP (Internet Protocol):** É responsável por endereçar e rotear os pacotes de dados para que eles cheguem ao computador de destino correto. Ele funciona como o carteiro que sabe encontrar o endereço da rua.
* **TCP (Transmission Control Protocol):** É um protocolo orientado à conexão, o que significa que ele estabelece uma conexão confiável entre o cliente e o servidor antes de qualquer troca de dados. O TCP garante que os pacotes de dados sejam entregues na ordem correta, sem erros e sem perdas. Se um pacote se perde no caminho, o TCP solicita a sua retransmissão. Pense no TCP como um serviço de entrega com rastreamento e confirmação de recebimento.

## O Fluxo de Comunicação TCP: Cliente e Servidor

A comunicação via sockets TCP/IP geralmente segue o modelo **cliente-servidor**. O servidor é um programa que "escuta" em uma porta específica por conexões de entrada. O cliente é um programa que inicia a comunicação, conectando-se ao servidor.

O processo de comunicação pode ser dividido nos seguintes passos:

### No Servidor:

1.  **`socket()`:** O servidor cria um objeto de socket.
2.  **`bind()`:** O servidor associa o socket a um endereço IP e a um número de porta específicos. Isso é como dizer: "Esta caixa de correio pertence a este endereço e está disponível para receber cartas".
3.  **`listen()`:** O servidor coloca o socket em modo de escuta, aguardando por tentativas de conexão de clientes. Ele define o número máximo de conexões que podem ficar em espera.
4.  **`accept()`:** Quando um cliente tenta se conectar, o servidor aceita a conexão. Essa função bloqueia a execução do programa até que uma conexão seja estabelecida e, então, retorna um novo socket que será usado para a comunicação com aquele cliente específico, liberando o socket original para continuar escutando por novas conexões.
5.  **`recv()` / `send()`:** O servidor usa o novo socket para receber dados do cliente (`recv()`) e enviar dados para o cliente (`send()`).
6.  **`close()`:** Quando a comunicação com o cliente termina, o servidor fecha o socket daquela conexão.

### No Cliente:

1.  **`socket()`:** O cliente cria um objeto de socket.
2.  **`connect()`:** O cliente usa o endereço IP e o número da porta do servidor para estabelecer uma conexão.
3.  **`send()` / `recv()`:** Uma vez conectado, o cliente pode enviar dados para o servidor (`send()`) e receber dados do servidor (`recv()`).
4.  **`close()`:** Quando a comunicação termina, o cliente fecha o socket.

## Comparação: Sockets TCP vs. Sockets UDP

Além do TCP, existe outro protocolo de transporte importante chamado **UDP (User Datagram Protocol)**. A escolha entre TCP e UDP depende das necessidades da aplicação.

| Característica      | Sockets TCP (Transmission Control Protocol)                                             | Sockets UDP (User Datagram Protocol)                                                                 |
| :------------------ | :-------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------- |
| **Conexão** | Orientado à conexão: estabelece uma conexão antes da transmissão de dados (handshake).    | Não orientado à conexão: envia os dados diretamente, sem estabelecer uma conexão prévia.              |
| **Confiabilidade** | Alta: garante a entrega ordenada e sem erros dos pacotes. Reenvia pacotes perdidos.       | Baixa: não há garantia de entrega, ordem ou ausência de erros. Pacotes perdidos não são reenviados. |
| **Velocidade** | Mais lento devido à sobrecarga do estabelecimento da conexão e da verificação de erros. | Mais rápido e com menor sobrecarga, pois não há mecanismos de confiabilidade.                         |
| **Casos de Uso** | Aplicações que exigem alta confiabilidade, como navegadores web (HTTP/HTTPS), e-mail (SMTP, POP3, IMAP) e transferência de arquivos (FTP). | Aplicações sensíveis ao tempo, onde a velocidade é mais crítica que a confiabilidade, como streaming de vídeo e áudio, jogos online e chamadas de voz sobre IP (VoIP). |

## Exemplo Prático: Cliente e Servidor TCP em Python

Para ilustrar o funcionamento dos sockets TCP, aqui está um exemplo simples de um servidor que ecoa as mensagens recebidas de um cliente, implementado em Python.

### Servidor (servidor.py):

```python
import socket

# Cria um socket TCP/IP
servidor = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Define o endereço e a porta do servidor
endereco_servidor = ('localhost', 12345)
print(f'Iniciando o servidor em {endereco_servidor[0]}:{endereco_servidor[1]}')
servidor.bind(endereco_servidor)

# Coloca o servidor em modo de escuta
servidor.listen(1)

while True:
    # Espera por uma conexão
    print('Aguardando por uma conexão...')
    conexao, endereco_cliente = servidor.accept()
    try:
        print(f'Conexão estabelecida com {endereco_cliente}')

        while True:
            # Recebe os dados do cliente
            dados = conexao.recv(1024)
            if dados:
                print(f'Recebido: {dados.decode()}')
                # Envia os dados de volta para o cliente (eco)
                print(f'Enviando de volta para {endereco_cliente}')
                conexao.sendall(dados)
            else:
                print(f'Nenhum dado recebido de {endereco_cliente}')
                break
    finally:
        # Fecha a conexão
        conexao.close()
