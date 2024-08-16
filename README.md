Para este exemplo suponho que voce ja tenha uma familiaridade com o docker ou que saiba os comandos básicos para subir uma container 

Hoje vamos criar uma container e deixá-lo pronto para rodar um projeto Laravel.

Este é um projeto de estudo mas que com os devidos ajustes voce pode usalo como exemplo e ao invés de instalar o laravel instalar outro framework de sua preferencia. 

Sem mais delongas vamos lá.

Iniciamos pegando uma imagem docker com o php funcionando, peguei uma imagem aleatória dentro do docker hub. 

A idea é fazer uma instalação normal executando o container com o php e em seguida fazer a instalação do laravel a sacada e voce ir anotando os passos que fez para depois otimizarmos isso usando o dockerfile. O Dockerfile pode-se dizer que é uma receita que vai executar sempre os mesmo passos desta forma sempre teremos uma imagen padrão desta forma matando a famosa frase “na minha maquina funciona”.  :P 

Let’s go!

Vamos iniciar baixando a nossa imagem do php, peguei uma imagem aleatória.

*docker run -it --name php php:7.4-cli bash*

docker run -> para startar uma imagem 
-it -> para interagir com esse container
–name -> para dar um nome a esse container
php:7.4-cli -> o nome da imagem que vamos usar 
bash -> complemento do -it para podermos digitar comandos no container.

Após esta ação voce terra uma container rodando php.

É uma boa pratica manter seu container sempre atualizado por isso vamos atualizar o nosso.

*apt-get update && apt-get upgrade*

Agora que já temos o container pronto e rodando php vamos precisar instalar novo framework neste caso o Laravel.


Ao acessar a pagina do laravel e ver na documentação como instalar. https://getcomposer.org/download/

Ao tentar instalar teremos um erro pois nao temos o “orquestrador” instalado por tanto vamos instalar o “Composer”

php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === 'dac665fdc30fdd8ec78b38b9800061b4150413ff2e3b6f88543c636f7cd84f6db9189d43a81e5503cda447da73c7e5b6') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"

Apos rodar os comandos de instalação do composer ja podemos dar inicio a instalação do Laravel.

Então vamos lá na documentação dele para ver como instalar ele no nosso container.https://laravel.com/docs/11.x/installation

Como podemos observar e bem tranquilo instalar basta digitar o seguinte comando.

*composer create-project laravel/laravel example-app*

Ops.. tivemos um erro ne…

O laravel tem algumas dependências que precisam ser carregadas para que ele consiga ser extraído e carregado.

*php composer-setup.php*

Agora vai vamos tentar rodar novamente  .. sera.. opss não foi. 

Temos que falar para o laravel que o zip ja esta pronto para isso vamos usar um instalador de pacotes específico para o php que vai adicionar o plugin do zip que ele ta reclamando na instalação.

*docker-php-ext-install zip*

Tentando novamente instalar o Laravel e agora tudo ocorreu como esperado, laravel instalado.

Vamos testar:

*php artisan serve*

Bhá e não é que eta rodou. …

Por hoje foi isso, atá a próxima onde vamos automatizar ele para subir o laravel junto do start do contêiner e expor a rota para que conseguimos consumir a rota na maquina host.
