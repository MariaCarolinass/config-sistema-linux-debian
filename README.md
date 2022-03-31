# Prática 2

Praticando configurações de sistema na distribuição Linux Debian.

*******
Sumário
 1. [Criação da máquina virtual](#virtualbox)
 2. [Instalação da distribuição Linux Debian](#debian)
 3. [Definição de cotas de armazenamento para os usuários](#cotas)
 4. [Diretórios compartilhados](#diretorios)
*******

<div id='virtualbox'/>

## Criação da máquina virtual

As configurações no Linux - Debian serão realizadas na máquina virtual criada no VirtualBox. O sistema operacional escolhido para a máquina virtual é o Debian (versão 32 bits), que é uma distribuição derivado do kernel Linux. As especificações definidas para a máquina virtual foram: 

- Memória RAM de 512 MB; 
- Disco rígido de 16 GB;
- Tipo de disco VDI;
- Tipo de alocação Dinamicamente Alocado

[A prática 1 descreve como criar uma máquina virtual e instalar o Linux - Debian](https://github.com/MariaCarolinass/config-servidor-apache2)

<div id='debian'/>

## Instalação da distribuição Linux Debian

Durante a instalação do Debian foram definidas as configurações que o disco da máquina terá. O disco será divido em três partições descritas na tabela abaixo:

|                     | Partição primária | Partição Lógica |      Partição Lógica     |
|:-------------------:|:-----------------:|:---------------:|:------------------------:|
| Sistema de arquivos |        Ext4       |       SWAP      |           Ext4           |
|  Ponto de montagem  |         /         |       SWAP      | /home                    |
|       Tamanho       |        8GB        |       1GB       | Espaço restante em disco |

Ao terminar de configurar as partições do disco deverá escolher a opções "Finalizar particionamento e escrever mudanças no disco". A imagem abaixo mostra como deve ser o resultado das mudanças:

<img src="https://github.com/MariaCarolinass/config-sistema-linux-debian/blob/main/imagens/particaodisco.png" alt="Mudanças no disco" width="600" height="500"/>

Uma **importante** configuração a se fazer é escolher o nome de usuário como "donald" que terá haver com os tópicos de usuários, grupos e permissões.

Finalizando a instalação do Debian, algumas opções deverão ser marcadas na etapa de seleção de software, são elas: 

- Ambiente de área de trabalho no Debian; 
- Xfce;
- Utilitários de sistema padrão

A imagem a seguir mostra as opções de software marcadas:

<img src="https://github.com/MariaCarolinass/config-sistema-linux-debian/blob/main/imagens/softwares.png" alt="Opções marcadas" width="600" height="500"/>

<div id='cotas'/>

## Definição de cotas de armazenamento para os usuários

Feito toda a instação da máquina e do ambiente Linux - Debian, agora serão definidas algumas configurações antes de realizar a criação dos novos usuários e grupos do sistema. Para isso é necessário definir as cotas padrões que todos os usuário terão, primeiro é preciso instalar o **quota** no terminal: 

`# apt-get install quota`

Agora, é necessário definir onde será o controle de armazenamento. Vamos editar o arquivo `/etc/fstab` e na linha que mostra o ponto de montagem `/home` será adicionado após o default a palavra `,usrquota` sem espaço. O comando a seguir abre o arquivo em questão:

`# nano /etc/fstab`

A imagem abaixo mostra o arquivo editado com as configurações citadas:

<img src="https://github.com/MariaCarolinass/config-sistema-linux-debian/blob/main/imagens/quota.png" alt="Configuração ursquota" width="600" height="400"/>

Após isso, é preciso atualizar as configurações que foram adicionadas, digitando o comando abaixo:

`# mount -o remount /home`

Para finalizar as configurações no ponto de montagem `/home` é preciso digitar os comandos abaixo:

`# quotacheck -cum /home`

`# quotaon /home`

Cada usuário terá 1GB de restrição de cota “leve” e 1.1GB de restrição de cota “rígida” para utilizar. E sempre qualquer novo usuário criado terá essas mesmas restrições de cota. O comando a seguir define as restrições de cota através do usuário principal `donald` criado durante a instalação do Linux - Debian:

`# edquota -u donald`

Deverá ser editadas as linhas soft e hard com as restrições de cota citadas. A imagem abaixo mostra as restrições adicionadas:

<img src="https://github.com/MariaCarolinass/config-sistema-linux-debian/blob/main/imagens/restricoes.png" alt="Restrições adicionas" width="600" height="400"/>

Caso queira verificar se ocorreu tudo certo:

`# quota -s donald`

A última configuração é no arquivo `/etc/adduser.conf`, onde será adicionado o nosso usuário modelo `donald`. A partir do comando abaixo:

`# nano /etc/adduser.conf`

Será editado a linha `QUOTAUSER`, nela vamos adicionar o nome donald entre as aspas e sem espaço, como mostra a imagem abaixo:

<img src="https://github.com/MariaCarolinass/config-sistema-linux-debian/blob/main/imagens/config-usuario.png" alt="Donald adicionado" width="500" height="400"/>

### Usuários e grupos

Serão criados 5 usuários: margarida, patinhas, huguinho, zezinho e luizinho. Todos esses usuários vão se aplicar as restrições de cota de armazenamento do usuário principal donald. Abaixo os comandos para criar cada usuário:

`# adduser margarida`

`# adduser patinhas`
 
`# adduser huguinho`

`# adduser zezinho`

`# adduser luizinho`

Para verficar as cotas de todos os usuários:

`# repquota -as`

A imagem abaixo mostra o resultado do comando `repquota -as`:

<img src="https://github.com/MariaCarolinass/config-sistema-linux-debian/blob/main/imagens/usuarios-criados.png" alt="Resultado do comando repquota" width="700" height="300"/>

O arquivo `/etc/passwd` mostra os usuário criados:

`# nano /etc/passwd`

Abaixo a imagem exibir o conteúdo do arquivo `/etc/passwd`:

<img src="https://github.com/MariaCarolinass/config-sistema-linux-debian/blob/main/imagens/usuarios.png" alt="Arquivo /etc/passwd" width="500" height="400"/>

Agora, os usários serão divididos nos grupos “adultos” e “criancas”. Primeiro é preciso criar os dois grupos com os comandos abaixo:

`# addgroup adultos`

`# addgroup criancas`

Os usuários foram divididos da seguinte forma:

- Adultos: donald; margarida; patinhas.
- Crianças: huguinho; zezinho; luizinho. 
 
Para adiciona-lós aos seus respectivos grupos é digitado os comando abaixo:

`# usermod -aG adultos donald`

`# usermod -aG adultos margarida`

`# usermod -aG adultos patinhas`

`# usermod -aG criancas huguinho`

`# usermod -aG criancas zezinho`

`# usermod -aG criancas luizinho`


O arquivo `/etc/groups` mostrar os usuários e grupos que foram criados, digitando o comando a seguir:

`# nano /etc/groups`

A imagem adiante mostra o resultado do comando digitado: 

<img src="https://github.com/MariaCarolinass/config-sistema-linux-debian/blob/main/imagens/grupos-criados.png" alt="Grupos criados" width="500" height="400"/>

Também com o comando `groups <nome-do-usuario>` é possível verificar o grupo de cada um dos usuários. Resultado do comando groups na imagem a seguir:

<img src="https://github.com/MariaCarolinass/config-sistema-linux-debian/blob/main/imagens/groups.png" alt="Resultado do comando groups" width="700" height="300"/>

<div id='diretorios'/>

## Diretórios compartilhados

Por fim, vamos práticar sobre permissões de arquivos. Serão criados dois diretórios de uso compartilhado, que ficarão salvos no diretório `/compartilhado` criado no ponto de montagem “/”. O comando abaixo cria o diretório:

`# mkdir compartilhado`

E para entrar no diretório `compartilhado`: 

`# cd compartilhado`

Dentro dele serão criados os diretórios adultos e criancas:

`# mkdir adultos`

`# mkdir criancas`

Em seguida, os comandos abaixo serão utilizados para adicionar os grupos que antes foram criados nos diretórios compartilhados que acabamos de criar:

`# chgrp adultos adultos`

`# chgrp criancas criancas`

Esses diretórios terão duas permissões diferentes para cada um, que estão descritas na tabela abaixo:

|        Diretório        |                 Permissão 1                 |                        Permissão 2                       |
|:-----------------------:|:-------------------------------------------:|:--------------------------------------------------------:|
|  /compartilhado/adultos | Usuários do grupo adultos podem ler e escrever dentro desse diretório | Usuários que não pertencem ao grupo “adultos” não podem ler, escrever ou executar esse diretório    |
| /compartilhado/criancas | Usuários do grupo crianças podem ler e escrever dentro desse diretório | Usuários que não pertencem ao grupo “crianças” podem ler e executar o diretório, mas não podem escrever |

Para adicionar as permissões definidas na tabela anterior, será digitado os seguintes comandos:

`# chmod a-rwx adultos`

`# chmod a-rwx criancas`

`# chmod g+rwx adultos`

`# chmod g+rwx criancas`

`# chmod o+rx criancas`

Verificando se as permissões foram realizadas:

`# ls -l`

A imagem a seguir mostra os resultados das permissões configuradas:

<img src="https://github.com/MariaCarolinass/config-sistema-linux-debian/blob/main/imagens/permissoes.png" alt="Resultado das perimissões" width="700" height="100"/>
