# Atualização do Moodle - IFRN EaD
**(OBS: Executar via CLI)**

### Instalações necessárias Debian 9 para o Moodle:

* Instale o SUDO
```shell
su apt-get install sudo
```

* Crie usuários e adicione as permissões de SUDO
```shell
useradd -m -U -d /home/usuario -s /bin/bash usuario

usermod -a -G sudo usuario
```

* Modifique a senha do usuário
```shell
passwd usuario
```

* Instale os pacotes essenciais
```shell
sudo apt-get -y update && sudo apt-get -y upgrade

sudo apt-get -y install sudo vim vim-scripts unzip zip p7zip-full htop iotop wget lynx curl locate ssh nano git build-essential software-properties-common
```

* Instale o Apache e configure o caminho padrão do WWW
```shell
sudo apt-get install apache2

sudo vim /etc/apache2/sites-available/000-default.conf

sudo service apache2 reload
```

* Instale o MySQL Server
```shell
sudo apt-get install mysql-server
```

* Instale o PHP 7.0 e extensões necessárias
```shell
sudo apt-get install php7.0 php7.0-mysql php7.0-json php7.0-curl php7.0-gd php7.0-intl php7.0-pspell php7.0-xml php7.0-xmlrpc php7.0-zip php7.0-cli php7.0-ldap aspell graphviz
```

================================================================================

### Dump e restauração do banco de dados

* Faça o dump do banco original
```shell
mysqldump -h 00.000.0.000 -u usuario_mysql -p nome_banco > /home/usuario/arquivo.sql
```

* Tranfira o **sql do banco**, a pasta **moodledata** e a **pasta do Moodle** via SCP
```shell
scp -r caminho_arquivo_origem usuario_destino@00.000.00.0:/home/usuario_destino/pasta_destino
```

* Restaure o banco
```shell
mysql -u root -p nome_banco < arquivo.sql
```

================================================================================

### Atualização via GitHub:

* Mude o nome da pasta antiga do Moodle
```shell
sudo mv moodle moodle_old
````

* Clone o repositório oficial do Moodle no GitHub
```shell
sudo git clone https://github.com/moodle/moodle.git nome_pasta
```

* Entre na pasta e escolha a versão do Moodle que você deseja fazer o upgrade
```shell
cd moodle

sudo git branch --track NOME_BRANCH origin/MOODLE_34_STABLE

sudo git checkout NOME_BRANCH
```

* Copie os plugins e o tema da pasta do Moodle antigo (e.g. moodle_old)

>blocks/course_overview_campus/

>blocks/enrollment/

>blocks/fbcomments/

>blocks/ranking/

>blocks/twitter_feed/

Dentro da pasta **blocks**:
```shell
sudo cp -R ../../moodle_old/blocks/course_overview_campus/ ../../moodle_old/blocks/enrollment/ ../../moodle_old/blocks/fbcomments/ ../../moodle_old/blocks/ranking/ ../../moodle_old/blocks/twitter_feed/ .
```

>filter/geshi/

Dentro da pasta **filter**:
```shell
sudo cp -R ../../moodle_old/filter/geshi/ .
```

>lib/editor/atto/plugins/computing/

>lib/editor/atto/plugins/htmlplus/

Dentro da pasta **lib/editor/atto/plugins**:
```shell
sudo cp -R ../../../../../moodle_old/lib/editor/atto/plugins/computing/ ../../../../../moodle_old/lib/editor/atto/plugins/htmlplus/ .
```

>local/cohortrole/

>local/mailtest/

>local/recyclebin/

Dentro da pasta **local**:
```shell
sudo cp -R ../../moodle_old/local/cohortrole/ ../../moodle_old/local/mailtest/ ../../moodle_old/local/recyclebin/ .
```

>mod/bigbluebuttonbn/

>mod/game/

>mod/hsuforum/

>mod/journal/

>mod/vpl/

Dentro da pasta **mod**:
```shell
sudo cp -R ../../moodle_old/mod/bigbluebuttonbn/ ../../moodle_old/mod/game/ ../../moodle_old/mod/hsuforum/ ../../moodle_old/mod/journal/ ../../moodle_old/mod/vpl .
```

>report/saas_export/

Dentro da pasta **report**:
```shell
sudo cp -R ../../moodle_old/report/saas_export/ .
```

>theme/essential/

Dentro da pasta **theme**:
```shell
sudo cp -R ../../moodle_old/theme/essential/ .
```

* Configure o arquivo **config.php** com as informações corretas


* Atualize via CLI (o comando upgrade pode demorar alguns minutos para finalizar)
```shell
sudo php admin/cli/maintenance.php --enable

sudo php /usr/bin/php admin/cli/upgrade.php

sudo php admin/cli/maintenance.php --disable
````

* Acesse o endereço que foi configurado no **config.php** e sua atualização estará finalizada
