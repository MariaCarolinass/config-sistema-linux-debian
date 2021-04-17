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

[A prática 1 descreve como criar uma máquina virtual e instalar o Linux - Debian](https://github.com/MariaCarolinass/config-servidor-apache2/edit/main/README.md)

<div id='debian'/>

## Instalação da distribuição Linux Debian

Durante a instalação do Debian foram definidas as configurações que o disco da máquina terá. O disco será divido em três partições descritas na tabela abaixo:

|                     | Partição primária | Partição Lógica |      Partição Lógica     |
|:-------------------:|:-----------------:|:---------------:|:------------------------:|
| Sistema de arquivos |        Ext4       |       SWAP      |           Ext4           |
|  Ponto de montagem  |         /         |       SWAP      | /home                    |
|       Tamanho       |        8GB        |       1GB       | Espaço restante em disco |

Ao terminar de configurar as partições do disco deverá escolher a opções "Finalizar particionamento e escrever mudanças no disco".

Uma **importante** configuração a se fazer é escolher o nome de usuário como "donald" que terá haver com os tópicos de usuários, grupos e permissões.

Finalizando a instalação do Debian, algumas opções deverão ser marcadas na etapa de seleção de software, são elas: 

- Ambiente de área de trabalho no Debian; 
- Xfce;
- Utilitários de sistema padrão

<div id='cotas'/>

## Definição de cotas de armazenamento para os usuários

<div id='diretorios'/>

## Diretórios compartilhados
