# Trabalho socket

Estes sockets fazem parte das aulas de redes de computadores!
Enjoy meus alunos!

Alunos!!!
***
## Sobre o trabalho

Documente com print e coloque aqui as respostas

### Simple server TCP :

1) Subir o tcp server simple explicar os estados da conexão, bind, listen etc.
 ![Captura de Tela (17)](https://github.com/morganananana/Fundatec-2024-Aula-Socket/assets/130008630/b798e6c0-da8d-491b-901a-43844230b836)
***
2) Executar o programa de cliente simple server tcp e verificar os estados da conexão.
![Captura de Tela (18)](https://github.com/morganananana/Fundatec-2024-Aula-Socket/assets/130008630/b79e6888-1675-411f-9ff7-4c77de2430e3)
***
3) Analise o código fonte
```python
 #echo-server.py
 importa o socket
 import socket
 #passa um endereço de ip
HOST = "127.0.0.1"  # Standard loopback interface address (localhost)
 #Número da porta
PORT = 65432  # Port to listen on (non-privileged ports are > 1023)
 #cria um novo objeto de soquete e o tipo
with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
    # Vinculado ao endereço e porta 
    s.bind((HOST, PORT))
    print("Bind")
     #coloca o soquete em um estado passivo
    s.listen()
    print("Listen")
     #bloqueia e espera por uma conexão de entrada
    conn, addr = s.accept()
    print("Connected")
     #imprime uma mensagem informando que uma conexão foi estabelecida e mostra o endereço do cliente
    with conn:
        print(f"Connected by {addr}")
         #loop infinito que continua recebendo dados do cliente através do soquete
        while True:
            data = conn.recv(1024)
            print(str(data))
             #se não houver mais dados a serem recebidos, o loop é interrompido
            if not data:
                break
             #dados recebidos são enviados de volta para o cliente
            conn.sendall(data)
```
***
4) Analise usando o wireshark explicando os pacotes.
![Captura de Tela (19)](https://github.com/morganananana/Fundatec-2024-Aula-Socket/assets/130008630/3bf36115-6285-48d0-b6b8-ec5bb3fd8531)
***
5) Diferencie a conexão UDP de TCP
   
TCP =
Tipo de conexão: Requer uma conexão estabelecida antes de transmitir dados.
Sequência de dados: Pode sequenciar dados, permitindo enviar um pedido específico.
Retransmissão de dados: Pode retransmitir dados caso os pacotes não cheguem ao destino.
Entrega: A entrega é garantida, garantindo que todos os dados cheguem ao destino na ordem correta.
Verificação de erros: Realiza uma verificação minuciosa de erros para garantir que os dados cheguem em seu estado pretendido.
Difusão: Não é compatível com difusão.

UDP=
Tipo de conexão: Nenhuma conexão é necessária para iniciar e terminar uma transferência de dados.
Sequência de dados: Não pode sequenciar ou organizar os dados, eles são transmitidos na ordem em que foram enviados.
Retransmissão de dados: Não há retransmissão de dados. Dados perdidos não podem ser recuperados.
Entrega: A entrega não é garantida, o que significa que alguns dados podem ser perdidos durante a transmissão.
Verificação de erros: Realiza uma verificação mínima de erros, cobrindo o básico, mas pode não impedir todos os erros.
Difusão: Compatível com difusão, permitindo enviar pacotes para todos os dispositivos em uma rede local.
***
### Simple server UDP :

1) Subir o tcp server simple explicar os estados da conexão, bind, listen etc.
![Captura de Tela (23)](https://github.com/morganananana/Fundatec-2024-Aula-Socket/assets/130008630/eceecf4e-3c38-40d0-a924-92c73361439d)
***
2) Executar o programa de cliente simple server tcp e verificar os estados da conexão.
![Captura de Tela (25)](https://github.com/morganananana/Fundatec-2024-Aula-Socket/assets/130008630/74837b03-f155-42f8-9365-54ad3b13a495)
***
3) Analise o código fonte
``` phyton
import socketserver

class MyUDPHandler(socketserver.BaseRequestHandler):
    """
    This class works similar to the TCP handler class, except that
    self.request consists of a pair of data and client socket, and since
    there is no connection the client address must be given explicitly
    when sending data back via sendto().
    """

    def handle(self):
        # recebe os dados e o soquete do cliente
        data = self.request[0].strip() #obtém dados
        socket = self.request[1] #obtém o soquete
        # imprime o endereço IP do cliente e os dados recebidos
        print("{} wrote:".format(self.client_address[0]))
        print(data)
        # envia os dados de volta para o cliente   
        socket.sendto(data.upper(), self.client_address)
# configura o servidor UDP para executar no HOST e PORT 
if __name__ == "__main__":
    HOST, PORT = "localhost", 65431
    with socketserver.UDPServer((HOST, PORT), MyUDPHandler) as server:
          # inicia o servidor para sempre 
        server.serve_forever()
```
***
4) Analise usando o wireshark explicando os pacotes.
![Captura de Tela (22)](https://github.com/morganananana/Fundatec-2024-Aula-Socket/assets/130008630/e9be5ddb-d087-43a0-b068-dd09e053aa42)
***

### Multiserver TCP :
1) Subir o tcp server simple explicar os estados da conexão, bind, listen etc.
 ![Captura de Tela (23)](https://github.com/morganananana/Fundatec-2024-Aula-Socket/assets/130008630/259f9035-85f2-4be2-b709-85d738d71e7a)
***
2) Executar o programa de cliente simple server tcp e verificar os estados da conexão.
![Captura de Tela (25)](https://github.com/morganananana/Fundatec-2024-Aula-Socket/assets/130008630/5f7b8340-8bc4-4013-881d-a11821be1979)
***
3) Analise o código fonte
 ```python
import socketserver


class MyTCPHandler(socketserver.BaseRequestHandler):
    """
    The RequestHandler class for our server.

    It is instantiated once per connection to the server, and must
    override the handle() method to implement communication to the
    client.
  
   def handle(self):
        # self.request é o soquete TCP conectado ao cliente
        self.data = self.request.recv(1024).strip()  # Recebe os dados do cliente
        print("{} escreveu:".format(self.client_address[0]))
        print(self.data)
        # Envia de volta os mesmos dados, mas em maiúsculas
        self.request.sendall(self.data.upper())

if __name__ == "__main__":
    HOST, PORT = "localhost", 65432

    # Cria o servidor, vinculando ao localhost na porta especificada
    server = socketserver.TCPServer((HOST, PORT), MyTCPHandler)

    # Ativa o servidor; isso continuará em execução até você
    # interromper o programa com Ctrl-C
    server.timeout = 10  # Define um tempo limite de 10 segundos
    server.serve_forever()  # Inicia o servidor para sempre ou até ser interrompido
```
***
4) Analise usando o wireshark explicando os pacotes.
   ![Captura de Tela (26)](https://github.com/morganananana/Fundatec-2024-Aula-Socket/assets/130008630/ab22de6d-0190-4dfa-9aa5-fa5436ad0925)
 ***
5) Explique as diferenças de multi conexões e porque a cada conexão a porta "muda". Demonstre a mudança de porta usando o Wireshark
   
Cada conexão TCP ou UDP é identificada por um par único de endereços IP e números de porta, conhecidos como "tupla de endereço". Quando ocorrem múltiplas conexões em um mesmo servidor, cada conexão precisa ter uma identificação única, e isso é feito através da combinação do endereço IP e número de porta de origem do cliente com o endereço IP e número de porta de destino do servidor.
Conexão multipla:
![Captura de Tela (27)](https://github.com/morganananana/Fundatec-2024-Aula-Socket/assets/130008630/e2781c55-147a-4ec9-8906-2bdc238217a3)
Conexão UDP:
![Captura de Tela (28)](https://github.com/morganananana/Fundatec-2024-Aula-Socket/assets/130008630/a4a80107-366c-4752-95e9-b90424b83a13)
***
### Conexão com máquina remota do colega :

1) Subir o tcp server na máquina do colega, verificar o IP da máquina (certifique que ele esteja na mesma rede que você)
   Meu =
 ![Captura de Tela (29)](https://github.com/morganananana/Fundatec-2024-Aula-Socket/assets/130008630/c6629aa8-f38e-424d-b69a-81155cf72b98)
  Colega =
![WhatsApp Image 2024-04-18 at 20 46 55](https://github.com/morganananana/Fundatec-2024-Aula-Socket/assets/130008630/fff794a1-821e-41a3-b762-7b650a431695)

   ***
3) Executar o programa cliente na sua máquina, não esqueça de modificar o IP para a máquina do seu colega.
   ![Captura de Tela (30)](https://github.com/morganananana/Fundatec-2024-Aula-Socket/assets/130008630/5b056af1-7409-488d-9f40-dadf73a1de92)
***
3) Demonstre com imagens que a conexão teve sucesso.
   ![Captura de Tela (31)](https://github.com/morganananana/Fundatec-2024-Aula-Socket/assets/130008630/049aa99a-f842-4639-90c0-20ec0f99ec73)
***
4) Usando wireshark mostra conexão filtrando pela portas.
![Captura de Tela (32)](https://github.com/morganananana/Fundatec-2024-Aula-Socket/assets/130008630/606917ce-cc94-4545-8d0f-d1ce28e38c85)

***
Exemplo colocando código

```python
# echo-client.py

import socket

HOST = "127.0.0.1"  # The server's hostname or IP address
PORT = 65432  # The port used by the server

with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
    s.connect((HOST, PORT))
    s.sendall(b"Hello, world")
    data = s.recv(1024)

print(f"Received {str(data)}")
```



