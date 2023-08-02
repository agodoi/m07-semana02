# Semana 01 - Arquitetura de Computação em Nuvem com API

Neste encontro iremos apresentar os principais serviços utilizados e o funcionamento de uma aplicação simples na nuvem. Desta forma conseguimos compreender como podemos distribuir, armazenar e analisar dados na nuvem eficientemente.

Iremos também abordar as regiões e como escolher adequadamente o local de armazenamento dos seus dados e dos clientes de forma segura e confiável.

Deve-se resgatar o conceito de API aplicando uma prática usando FLASK (minimal application), utilizando o EC2 da instrução anterior, e realizar diversas requisições na API.


<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://s33046.pcdn.co/wp-content/uploads/2021/03/representational-state-transfer-diagram_gray-e1615546557211.png">
   <img alt="Arquitetura Geral" src="[YOUR-DEFAULT-IMAGE](https://s33046.pcdn.co/wp-content/uploads/2021/03/representational-state-transfer-diagram_gray-e1615546557211.png)">
  </picture>

Nessa aula, vamos usar uma instância EC2 com:

* Flask (backend):
  
  Framework que utiliza a linguagem Python para criar aplicativos Web. Muito útil para a criação de API RESTful, que por sua vez, utiliza os métodos padrão do protocolo HTTP (como GET, POST, PUT, DELETE) ou CRUD (Create, Read, Update and Delete).

<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://miro.medium.com/v2/resize:fit:2000/format:webp/1*nFxyDwJ2DEH1G5PMKPMj1g.png">
   <img alt="Arquitetura gunicorn" src="[YOUR-DEFAULT-IMAGE](https://miro.medium.com/v2/resize:fit:2000/format:webp/1*nFxyDwJ2DEH1G5PMKPMj1g.png)">
</picture>

* Gunicorn (backend):

  É uma abreviação de Green Unicorn. Trata-se de um servidor HTTP WSGI (Web Server Gateway Interface) para Python. Ele é usado para executar aplicativos da web Python, como aplicativos Django ou Flask, em produção de forma eficiente e confiável. Para executar um aplicativo Python com o Gunicorn, você normalmente precisa criar um arquivo de configuração, especificando o número de processos ou trabalhadores que o servidor deve utilizar. O Gunicorn então gerencia esses processos e cuida da comunicação entre o servidor da web e o aplicativo Python. Esse aplicativo será chamado de *index.py* mais para frente.

<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://miro.medium.com/v2/resize:fit:2000/format:webp/1*nFxyDwJ2DEH1G5PMKPMj1g.png">
   <img alt="Arquitetura gunicorn" src="[YOUR-DEFAULT-IMAGE](https://miro.medium.com/v2/resize:fit:2000/format:webp/1*nFxyDwJ2DEH1G5PMKPMj1g.png)">
  </picture>


* NGINGX (deployment):

  É um servidor web open source de alta performance que entrega o conteúdo estático de um site de forma rápida e fácil de configurar. É famoso entre grandes empresas da área de TI e concorre direto com o Apache. Não é preciso se preocupar com a quantidade de conexões simultâneas (concorrência) feitas no site, pois ele é altamente elástico. A função dele é receber as requisições do seu site e encaminhá-las para o Gunicorn. Veja o gráfico para entender sua arquitetura.

  <picture>
   <source media="(prefers-color-scheme: light)" srcset="https://cdn-media-1.freecodecamp.org/images/RooSvbKlAWsOjkz8JPactXH-GPf4Pe6DC3Ue">
   <img alt="Arquitetura NGINX" src="[YOUR-DEFAULT-IMAGE](https://cdn-media-1.freecodecamp.org/images/RooSvbKlAWsOjkz8JPactXH-GPf4Pe6DC3Ue)">
  </picture>

# Objetivo dessa prática

Vamos criar uma API *index.py* que será executado com a ajuda do *gunicorn* pela porta 8000 e estará roteado pelo *nginx* pela porta 80.

# Pré-requisitos

### Roteie o sinal de Internet do seu celular para o seu computador, pois é normal o proxy local da rede bloquear a conexão SSH.

### Tenha uma conta AWS "logada", seja acadêmica, seja particular.

# Passo-01

Crie uma instância EC2 no console AWS (particular ou acadêmica) com as seguintes características:

## Free tier (nível gratuito)
## Salve a chave SSH (sugestão de nome "FlaskServerUbuntu") no seu Desktop (deixe no diretório Desktop). Chave tipo PEM.
## Sistema operacional Ubuntu
## Regras de entrada:
### HTTPS
### HTTP
### SSH
### TCP personalizado, digite porta 8000 (importante esse passo)

# Passo-02

Abra o terminal Power Shell ou CMD do Windows ou qualquer outro terminal que suporte a conexão SSH (Secure Shell) e cole a linha de acesso da instância do seu EC2.
Você consegue essa linha de acesso clicando em CONECTAR que está no seu console AWS, na opção EC2. Exemplo de linha de acesso:

*ssh -i "FlaskServerUbuntu.pem" ubuntu@ec2-35-175-138-163.compute-1.amazonaws.com*

A sua chave PEM que você fez download da AWS precisa estar no mesmo diretório que você está apontado no seu terminal Power Shell.

# Passo-03

Se a conexão SSH com o seu EC2 tiver ocorrido com sucesso, você terá instanciado um computador virtual na infraestrutura da AWS e terá as seguintes impressões de mensagens na seu terminal:

**Welcome to Ubuntu 22.04.2 LTS (GNU/Linux 5.19.0-1025-aws x86_64)**

**>Documentation:  https://help.ubuntu.com**

**>Management:     https://landscape.canonical.com**

**>Support:        https://ubuntu.com/advantage**

**System information as of Wed Aug  2 19:08:03 UTC 2023 [...]**

e você estará parado num diretório raiz do tipo **ubuntu@ip-172-31-93-207:~$** onde o IP será exclusivo da sua instância e não igual a esse.

# Passo-04:

Atualize sua instância EC2 com os seguintes comandos:

### sudo apt-get update [enter]

### sudo apt-get install python3.10-venv [enter]

# Passo-05:

Crie um diretório e subdiretório dentro dele:

### mkdir cashman-flask-project [enter]

### mkdir cashman [enter]

# Passo-06:

Instale um ambiente virtual chamado *venv* para você criar as dependências python exclusivas para essa prática.
Daqui para frente, mantenha-se no path **cashman-flask-project/cashman**

### python3 -m venv venv

# Passo-07:

Vamos ativar o seu ambiente virtual:

### source venv/bin/activate [enter]

# Passo-08:

Instale o Flask no seu ambiente virtual:

### pip install flask

# Passo-09:

Crie e edite a aplicação que chamará **index.py**

### sudo nano index.py

E adicione o seguinte código dentro do **index.py**. Copie e cole esse código, cuidado com os aninhamentos. Dê um Ctrl+S para Salvar e Ctrl+X para sair do editor.

from flask import Flask, jsonify, request

app = Flask(__name__)

incomes = [
    { 'description': 'salary', 'amount': 12.200 }
]

@app.route('/incomes')

def get_incomes():

    return jsonify(incomes)


@app.route('/incomes', methods=['POST'])

def add_income():

    incomes.append(request.get_json())
  
    return '', 204


if __name__ == "__main__":
  
    app.run()

# Passo-10:

Caso queira conferir como ficou a sua edição, faça:

### cat index.py [enter]

# Passo-11:

Caso queira testar o seu código index.py, faça:

### python3 index.py [enter]

# Passo-12:

Agora vamos instalar o servidor python *gunicorn*:

### pip install gunicorn [enter]

# Passo-13:

Agora vamos criar o script de boot no seu EC2: 

### sudo nano /etc/systemd/system/cash.service

E copie e cole esse script no seu arquivo cash.service:

[Unit]

Description=Gunicorn serve para instanciar uma simples aplicacao cashman.service

After=network.target

[Service]

User=ubuntu

Group=www-data

WorkingDirectory=/home/ubuntu/cashman-flask-project

ExecStart=/home/ubuntu/cashman-flask-project/venv/bin/gunicorn -b localhost:8000 cashman.index:app

Restart=always

[Install]

WantedBy=multi-user.target

# Passo-14:

Agora execute o serviço cashman.index:

### sudo systemctl start cash.service

Caso altere alguma coisa no cash.service, faça:

### sudo systemctl daemon-reload

Caso queira iniciar automaticamente o serviço cash.service no booting da sua instância EC2, faça:

### sudo systemctl enable cash.service

# Passo-15:

Caso queira testar localmente como está sua aplicação, faça:

### curl localhost:8000/incomes

# Passo-16:

Falta instalar o NGINX no seu ambiente virtual, então faça:

### sudo apt-get nginx


# Passo-17:

Agora vamos ajustar o servidor NGINX para endereçar a nossa aplicação:

# sudo nano /etc/nginx/sites-available/default

Adicione essas linhas logo no começo, após os comentários

#Please see /usr/share/doc/nginx-doc/examples/ for more detailed examples.
#Default server configuration

upstream flaskhelloworld {
  server 127.0.0.1:80;
}

e dentro do location, comente esse linha:

try_files $uri $uri/ =404;

e adicione essa:

proxy_pass http://flaskhelloworld;

ficando assim:

location / {
    proxy_pass http://flaskhelloworld;
}


# Passo-17:

Vamos iniciar o servidor:

### sudo systemctl start nginx

# Passo-18:

Agora está na hora de testar a sua API. Então faça:

Copie seu link externo do EC2 (que está no console da AWS, em EC2, na opção **DNS IPv4 público**) e cole no navegador acrescido de */incomes*

Exemplo: **ec2-54-161-205-18.compute-1.amazonaws.com/incomes**

Quando fizer isso, você fará um GET e deverá retornar o seguinte:


