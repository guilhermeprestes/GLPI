# GLPI

## Implementação do sistema de HelpDesk GLPI no UBUNTU Server

![imagem-exemplo](https://glpi-project.org/wp-content/uploads/2020/06/upload_9504af02ea5974a0532b17c2b38144aa.png)

## Requisitos 
1 Servidor para Banco de dados <br>
1 Servidor para GLPI

> Neste caso estamos utilizando um servidor com UBUNTU SERVER

## Banco de dados

Dentro da console do MySQL, vamos criar o banco de dados com o nome glpi, um
usuário e a senha, com permissão para acessar seu próprio banco:

```bash
mysql> create database glpi character set utf8 collate utf8_bin;
mysql> create user glpi@ip identified by 'glpipw';
mysql> grant all privileges on glpi.* to glpi@ip;
mysql> quit;
```

## Apache

 Agora, precisamos instalar o servidor da web Apache e todo o software necessário.
No console do Linux, use os seguintes comandos para instalar os pacotes necessários.

```bash
# apt-get install apache2 php libapache2-mod-php
# apt-get install php-json php-gd php-curl php-mysql php-mbstring
# apt-get install php-xml php-cli php-imap php-ldap php-xmlrpc php-apcu
```

Agora, você deve encontrar a localização do arquivo php.ini em seu sistema.
Depois de encontrar, você precisa editar o arquivo php.ini.

```bash
# updatedb
# locate php.ini
# vim /etc/php/7.4/apache2/php.ini
```
**Aqui está o arquivo original, antes da nossa configuração.**

> file_uploads = On <br>
max_execution_time = 30 <br>
memory_limit = 128M <br>
post_max_size = 8M <br>
max_input_time = 60 <br>
; max_input_vars = 1000 <br>

**Aqui está o novo arquivo com nossa configuração.**

> file_uploads = On <br>
max_execution_time = 300 <br>
memory_limit = 256M <br>
post_max_size = 32M <br>
max_input_time = 60 <br>
max_input_vars = 4440 <br>

Você também deve reiniciar o apache manualmente e verificar o status do serviço.

```bash
# service apache2 stop
# service apache2 start
# service apache2 status
```

## INSTALAÇÃO DO GLPI
se os seguintes comandos para baixar o pacote GLPI.

```bash
# wget https://github.com/glpi-project/glpi/releases/download/9.5.5/glpi-9.5.5.tgz
# tar -zxvf glpi-9.5.5.tgz
# ls glpi glpi-9.5.5.tgz
```
Mova todos os arquivos GLPI para o diretório raiz da sua instalação do Apache.
Defina a permissão de arquivo correta em todos os arquivos movidos.

```bash
# mkdir /var/www/html/glpi
# mv glpi/* /var/www/html/glpi
# chown www-data.www-data /var/www/html/glpi/* -R
```


Crie um arquivo de configuração do Apache chamado `glpi.conf`

```bash
# vim /etc/apache2/conf-available/glpi.conf
```
Copie e cole as seguintes irformações

><Directory /var/www/html/glpi> <br>
AllowOverride All <br>
</Directory> <br>
<Directory /var/www/html/glpi/config> <br>
Options -Indexes <br>
</Directory> <br>
<Directory /var/www/html/glpi/files> <br>
Options -Indexes <br>
</Directory> <br>


Ative a nova configuração no Apache.

```bash
# a2enconf glpi
```

instale alguns pacotes necessarios

```bash
# apt-get install php7.4-intl
# apt-get install php-cas
# apt-get install php-zip
# apt-get install php-bz2
# apt-get install php-ldap
```

Reinicie o serviço Apache.

```bash
# service apache2 stop
# service apache2 start
```

Abra o seu navegador e digite o endereço IP do seu servidor /glpi

Em nosso exemplo, o seguinte URL foi inserido no navegador:

> http://192.168.0.10/glpi

> obs - No console do Linux, exclua o arquivo de instalação do GLPI.
{.is-warning}

```bash
# rm /var/www/html/glpi/install/install.php
```

> (usuario padrao - user: glpi pass: glpi|)
