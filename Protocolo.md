## Protocolo cliente-servidor para transferência de arquivos

### 1. Introdução

Este documento especifica um protocolo da camada de aplicação com arquitetura
cliente-servidor para transferência e execução de arquivos. Os serviços desse
protocolo rodam sobre TCP, que foi escolhido em virtude de oferecer transferência
confiável de dados. A porta utilizada pelo protocolo é a 30000.

- Porta padrão: 30000
- Arquitetura: Cliente-Servidor
- Protocolo na camada de transporte: TCP
- Padrão de caracteres: ASCII


### 2. Cliente e Servidor

A arquitetura utilizada neste protocolo é a *Cliente-Servidor*. Desta forma, para
que o cliente possa enviar algum comando ao servidor será necessário que uma
conexão TCP (protocolo da camada de trasporte escolhido) já esteja estabelecida
entre os dois hosts.


#### 2.1 O cliente

A função básica do cliente é enviar arquivos para o servidor. O servidor irá
aceitar arquivos no formatos Node ou JavaScript (.js) enviados pelo do usuário,
interpretar os arquivos e retornar uma resposta relativa à interpretação do
arquivo e adicionalmente, uma mensagem relatando o sucesso ou erro no envio no
arquivo e em sua execução.

<!-- Como os dados devem ser enviados estão definidos no item X (colocar o formato do JSON) -->

#### 2.2 O servidor

As funções básicas do servidor são aceitar conexões de múltiplos clientes, aceitar
os comandos dos clientes definidos neste documento e arquivos que serão enviados.
Ele também irá executar os arquivos (se estiverem no formato adequado) e retornar
o resultado da execução, além de uma resposta relativa ao estado do envio e execução
(sucesso ou erro).


### 3. Definição de comandos

Como a conexão entre o cliente e servidor já estará concreta, a comunicação entre
os dois hosts será através de comandos e retornos descritos a seguir.

Os comandos descritos nessa seção, são exclusivamente enviados pelo cliente ao
servidor. Os retornos descritos nessa seção são retornos enviados exclusivamente
do servidor para o cliente.

- **USER** - Comando para pedir autorização para conectar no servidor
- **SEND** - Comando para enviar um arquivo .js para execução no servidor
- **RUN** - Comando utilizado para executar o último arquivo enviado ao servidor
- **QUIT** - Comando utilizado para encerrar a conexão


### 3.1 - USER

Este comando é utilizado para a que o usuário faça a autenticação no servidor para
obter acesso (restrito de acordo com suas permissões) ao servidor.
O nome do usuário e sua senha deveram, obrigatoriamente, ser formados por caracteres
alfanuméricos.

#### 3.1.1 - Formato

```
user <login>:<password>
```
Ex: ```user meuUser:1234```


#### 3.1.1 - Retornos

- 100 - Conectado;  
- 101 - Não foi possível conectar;
- 102 - Usuário/senha incorretos;


### 3.2 - SEND

Comando utilizado para enviar arquivos para execução no servidor. Podem ser
enviados outros arquivos, porém, só será executado um arquivo por vez (neste caso,
o último a ser enviado) no formato .js (JavaScript) por vez. Desta forma, caso
exista alguns arquivos que necessitem estar presentes durante a execução do
arquivo .js, estes deverão ser enviados antes do arquivo .js principal.

<!-- O servidor executará o último arquivo enviado pelo cliente. Desta forma, caso
este arquivo não tenha o formato .js ele não será executado. Caso o arquivo (.js)
que o cliente deseja executar no servidor necessite de alguns outros arquivos
de diferentes formatos, eles deverão ser enviados anteriormente ao arquivo .js
principal. -->

#### 3.2.1 - Formato

send <path_to_file>

Ex: send toto.js

#### 3.2.1 - Retornos

- 200 - Arquivo recebido
- 201 - Erro ao receber o arquivo

### 3.3 - RUN

Comando utilizado para executar o último arquivo do usuário enviado ao servidor.
Este comando poderá retornar, além de informações referentes a execução, o
resultado da aplicação executada.

#### 3.3.1 - Formato

run

#### 3.3.2 - Retornos

- 300 - Execução concluída com sucesso
- 301 - Erro ao executar o arquivo - verifique a sintaxe
- 302 - Erro: Arquivo não encontrado
- 303 - Erro: Arquivo em formato incompatível
- 304 - Erro: Dependências não encontradas
- 305 - Resultado

#### 3.3.3 - 305 - Resultado

DEFINIR

### 3.3 - QUIT

Comando utilizado para encerrar a conexão entre servidor e o cliente.

#### 3.3.1 - Formato

quit <parametro>

onde <parametro> pode ter valor 1 ou 2, sendo assim:

- quit 1 : Encerra a aplicação sem obter resultados
- quit 2 : Espera pelo resultado para encerrar a aplicação

				Aborta a execução e encerra a conexão

				Termina a execução, envia os retornos, e deleta os arquivos

Quantos clientes podem conectar no servidor por vez?
Quantos arquivos em paralelo podem ser executados?

### 4. Descrição dos retornos

- 100 - Conectado
- 101 - Não foi possível conectar;
- 102 - Usuario/senha incorretos;
- 200 - Arquivo recebido;
- 201 - Erro ao receber o arquivo;
- 300 - Execução concluída com sucesso;
- 301 - Erro ao executar o arquivo - verifique a sintaxe
- 302 - Erro: Arquivo não encontrado
- 303 - Erro: Arquivo em formato incompatível
- 304 - Erro: Dependências não encontradas
- 305 - Resultado

### 5. Observações

-  Quando a execução de um arquivo .js encerrar, o servidor envia os retornos
cabíveis, e deleta os arquivos;
- Caso o arquivo no formato .js dependa de outros arquivos para execução, estes
deverão ser enviados através do comando "send" para o servidor, sendo que o arquivo
.js principal deverá, obrigatoriamente, ser o último a ser envido.
- Cada usuário poderá executar apenas uma aplicação (arquivo .js) no servidor.
- O máximo de clientes conectados no servidor é 4.
- Caso a conexão entre cliente-servidor seja perdida, todo o processo deverá ser
reiniciado. Desta forma, todos os arquivos deverão ser enviados novamente do
cliente ao servidor para inicar a execução.
