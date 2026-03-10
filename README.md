# Manual de configuração SAMBA

Este vai ser um guia rápido de como instalar o [SAMBA](https://www.samba.org/) para utilizar como um acesso direto a uma pasta ou drive em seu servidor Raspberry Pi. Caso tenha uma instância do Nextcloud rodando em sua maquina esse método é compativel para realizar acessos via sua rede local.

## Setup

Para esse guia, será utilizado um Raspberry Pi 5 4 GB como máquina principal. Caso queira, é possível utilizar qualquer máquina que seja x86 ou ARM64 (aarch64), tendo em mente que essa máquina deve contar com um suporte no mínimo OK.

### Instalação do OS (Sistema Operacional)

Como utilizarei um Raspberry Pi, irei utilizar um método mais simples e convencional para realizar a instalação do sistema operacional. Para isso, utilizarei o [RaspberryPi Imager](https://www.raspberrypi.com/software/). Como estou utilizando Linux para fazer esse processo, recomendo baixar a [AppImage](https://downloads.raspberrypi.com/imager/imager_latest_amd64.AppImage) para te salvar de dor de cabeça e perda de tempo com versões desatualizadas disponibilizadas por cada Distro.

Após baixar o arquivo AppImage, entre na pasta onde o arquivo foi salvo e execute-o com o comando: `sudo ./imager_2.0.4_amd64.AppImage`

**Passo a passo**:

1. Selecione o dispositivo que irá utilizar:
   ![exemplo](hhttps://github.com/ChancellorMarko/manual_samba_raspberrypi/blob/main/img/1_screen.png)
2. Selecione seu sistema operacional de preferência, no meu caso utilizarei outros sistemas operacionais da própria Raspberry, como o Rasp OS Lite (64 bit):
   ![exemplo](hhttps://github.com/ChancellorMarko/manual_samba_raspberrypi/blob/main/img/2_screen.png)
3. Insira as informações pertinentes de sua preferência conforme o que for solicitado.
4. Após configurar todas as informações pertinentes nas telas anteriores, você terá uma tela de acesso remoto que, dependendo da sua intenção, pode ser ou não interessante. Caso você queira acessar essa máquina remotamente para configuração via sua rede local (LAN), habilite a função SSH. Você tem duas opções para login via SSH:
   - Senha: com essa opção, você poderá escolher uma senha para realizar o login via sua rede, porém essa opção é insegura caso queira deixar essa máquina exposta na rede pública.
   - Chave SSH: uma das opções mais seguras caso queira deixar a máquina exposta na rede pública ou local.
   - Passo a passo: ![Configuração SSH](hhttps://github.com/ChancellorMarko/manual_samba_raspberrypi/blob/main/Configura%C3%A7%C3%A3o%20SSH.md).
5. Caso queira, pode ativar a opção de Raspberry Pi Connect, no meu caso deixarei desativado.
6. Continue para a escrita das informações no cartão SD (**Atenção: todos os dados presentes nele serão apagados**).
   ![exemplo](hhttps://github.com/ChancellorMarko/manual_samba_raspberrypi/blob/main/img/5_screen.png)

### Login via SSH

Com sua máquina rodando e conectada à internet, você deverá descobrir o IP dessa máquina na sua rede local para realizar a conexão via SSH. Após descobrir o IP da sua máquina, geralmente algo parecido com 192.168.0.XXX, você poderá fazer login:

Para isso, utilize o comando abaixo para adicionar sua chave customizada:

```bash
ssh-add server-key-file-name
```

Após isso, tente realizar o login via comando no terminal:

```bash
ssh seu-usuario@ip-da-sua-maquina
```

Caso tenha problemas, tente usar a chave diretamente no comando:

```bash
ssh -i server-key-file-name seu-usuario@ip-da-sua-maquina
```

Com tudo funcionando corretamente, mova as chaves para a pasta onde deveriam estar:

```bash
mv server-key-file-name server-key-file-name.pub ~/.ssh/
```

---
