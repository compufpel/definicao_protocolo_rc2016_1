# Protocolo Trabalho de Redes de Computadores 2016/1

## 1. Características Gerais

- Protocolo na camada de transporte: TCP/IP
- Porta: 30000

## 2. Definições de comandos.

### 2.1 Comando: User
{command:”user”, login:”<login>”, password:”<key>”, send:”<IP>”,  receive”:<IP>”}

Comando para pedir autorização para conectar no servidor.

### 2.2 Comando: User-return
{command:”user”, return:”<codigo>”, send:”<IP>”,  receive”:<IP>”}

Retorna ao cliente que ele pode enviar outros comandos.

#### 2.2.1 códigos de retorno:

100 - Login aceito.
101 - Login ou senha inválido.
102 - Servidor não respondeu.

### 2.3 Comando: Help
{command:”help” , send:”<IP>”, receive”:<IP>”}

Pede para o servidor todos comandos existentes do protocolo.

### 2.4 Comando: Help-return
{command:”help-return”, return:”<lista de comandos>”, send:”<IP>”, receive”:<IP>”}

Retorna todos comandos existentes do protocolo do servidor.

### 2.5 Comando: Put
{command:”put”, path:”<caminho do arquivo>”, send:”<IP>”, receive”:<IP>”}

Envia um arquivo a ser executado no servidor.

### 2.6 Put-return:
{command:”put-return”, return:”<codigo do que aconteceu>”,mensage:”<mensagem de retorno do compilador>”, send:”<IP>”, receive”:<IP>”}

Retorna um arquivo do servidor e a mensagem referente a sua compilação.

#### 2.6.1 Códigos de retorno:

200 - Compilado com sucesso.
201 - Erro de compilação.
202 - Dependência detectada.
203 - O servidor parou de responder.
204 - Conexão perdida.

### 2.7 Comando: status
{command:”status”, send:”<IP>”, receive”:<IP>”}

Pede um status do servidor

### 2.8 Comando: status-return
{command:”status-return”, rerurn:”<codigo do que aconteceu>”, send:”<IP>”, receive”:<IP>”}

Retorna um status do servidor.

#### 2.8.1 Códigos de retorno:

300 - Servidor online.
301 - Servidor não respondendo.

## 3. Referencias

- JSON (JavaScript Object Notation)
http://www.json.org/
https://pt.wikipedia.org/wiki/JSON
