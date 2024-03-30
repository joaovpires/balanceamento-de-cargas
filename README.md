# Atividade balanceamento de cargas - Tutorial

Passo 1: Criar 3 instâncias EC2 Ubuntu na AWS.
Ex: backend1, backend2 e frontend

Com a chave .pem de autenticação criada, adicione-a no terminal, clicando em "Ações" e carregando o arquivo da chave.
Em seguida, digite `chmod 400` 

Para conectar a instância, utilize `ssh -i “nome-da-chave.pem” ubuntu@IPPUBLICOFRONT`

## 2 - Instalando o nginx na instância do front-end

Use `sudo apt get update` e `sudo apt-get install nginx` para realizar a instalação, em seguida `sudo service nginx restart`

Para testar, abra no navegador inserindo o IP público do front, substituindo o protocolo https pelo http na url. Deverá aparecer uma página com o título "Welcome to Nginx"

## 3 - Arquivo de configuração

Agora iremos criar um arquivo 'load-balancer.conf, pelos comandos:<br>
`cd /etc/nginx/conf.d`<br>
`sudo nano load-balancer.conf`<br>
Após isso, cole esse template e altere os valores em 'x' para os valores reais de IP das instâncias dos seus servidores e do balanceador.

upstream backend {
	server XXX.XX.XX.XX:5000; # backend1 - IP privado
	server XXX.XX.XX.XX:5000; # backend2 - IP privado
}

server {
	listen 80;
	server_name XX.XXX.XX.XXX; # frontend - IP publico
	location / {
		proxy_pass http://backend;
	}
}

