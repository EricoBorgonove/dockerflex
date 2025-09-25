# Requisitos

Docker e docker-compose instalados.

# O que inclui

Imagens do php7, mysql-5.7 e nginx prontas para uso.

A imagem do php7 inclui: 
* OpenSSL
* Client do Postgres
* Chromium (necessário para Test)
* PDO (mysql e postgres)
* Extensões do PHP: zip, exif, calendar, bcmath, intl, libpng/jpeg
* Node e NPM
* Nano, cron
* Composer
* etc.

# Instalação

1. Clone o projeto
````sh
git clone https://github.com/flexpeak/docker-flex.git
````
2. Entre na pasta do projeto
````sh
cd docker-flex
````
3. Copie o arquivo ``.env-example`` para um novo arquivo chamado ``.env`` e, se necessário, edite de acordo com as suas preferências
````sh
cp .env.example .env
````
4. Faça o up (o comando up também fará o build na primeira vez - este comando poderá demorar horas, dependendo da configuração da máquina)
````sh
sudo docker-compose up -d
# ATENÇÃO: para Windows não precisa de sudo
````

Os containers estão prontos para uso. 

# Usagem
Os comandos tem que ser executados **SEMPRE** na pasta do projeto docker-flex.

## Entrando no container do php7
Para usar o php-cli, o artisan ou o npm, você deverá entrar no container do php7 com o seguinte comando 
````sh
sudo docker-compose exec --user=flexdock php7 bash
```` 
Para entrar usando o usuário root
````sh
sudo docker-compose exec php7 bash
````
**Para Windows não é necessário o sudo**
````sh
docker-compose exec --user=flexdock php7 bash
````
**Dependendo do seu terminal,** você poderá precisar fazer uso do winpty (quando usar o Git Bash) no **Windows**
````sh
winpty docker-compose exec --user=flexdock php7 bash
````

## Criando projetos Laravel
Para criar projetos Laravel para o PHP7+ você deve entrar na pasta do nginx, entrar na pasta sites e criar um novo arquivo .conf para o seu projeto. Após isso editar as configurações. Siga os passos abaixo

````sh
cd nginx # entrou na pasta no nginx
cd sites # entrou na pasta sites
cp laravel.conf XXXX.conf # criou um arquivo .conf com base no arquivo laravel. XXXX é o nome do seu projeto
code XXXX.conf # vamos editar o arquivo .conf no vscode
````

No server_name altere para o nome do seu projeto. Usaremos de exemplo o nome ``projeto-teste``. O sufixo ``.test`` é uma boa prática.
````sh
server_name projeto-teste.test;
````
Você vai querer editar o root do seu projeto. O nome do seu projeto vai entre o ``/var/www/`` e o ``/public``.
````sh
root /var/www/projeto-teste/public;
````
Após isso você deve voltar para o terminal, entrar na pasta do projeto do docker-flex e reiniciar os containers
````sh
cd docker-flex # entrou no projeto do docker-flex
sudo docker-compose restart # reiniciou os containers - no Windos não precisa do sudo
````

Por fim você deve editar o arquivo hosts.
### No Linux
````sh
sudo nano /etc/hosts
````
Adicione as seguintes linhas no final do arquivo /etc/hosts
````sh
127.0.0.1   projeto-teste.test
````

### No Windows
Para editar o arquivo hosts no Windows você irá precisar abrir o arquivo usando algum editor de texto como administrador. Como o notepad++ ou o próprio vscode. 

O arquivo hosts no Windows fica no seguinte caminho: C:\Windows\System32\drivers\etc\hosts

Cuidado ao editar usando o Bloco de Notas (notepad), pois este arquivo não possui extensão e poderá ocasionar problemas ao salvar.

Adicione as seguintes linhas no final do arquivo /etc/hosts
````sh
127.0.0.1   projeto-teste.test
````

### Verificando configurações
Ao abrir no navegador o endereço http://projeto-teste.test você deverá ver o projeto Laravel instalado corretamente.
