# Manual de configuração SAMBA

Este vai ser um guia rápido de como instalar o [SAMBA](https://www.samba.org/) para utilizar como um acesso direto a uma pasta ou drive em seu servidor Raspberry Pi. Caso tenha uma instância do Nextcloud rodando em sua máquina esse método é compatível para realizar acessos via sua rede local.

## Setup

Para esse guia, será utilizado um Raspberry Pi 5 4 GB como máquina principal. Caso queira, é possível utilizar qualquer máquina que seja x86 ou ARM64 (aarch64), tendo em mente que essa máquina deve contar com um suporte no mínimo ok.

### Instalação do OS (Sistema Operacional)

Como utilizarei um Raspberry Pi, irei utilizar um método mais simples e convencional para realizar a instalação do sistema operacional. Para isso, utilizarei o [RaspberryPi Imager](https://www.raspberrypi.com/software/). Como estou utilizando Linux para fazer esse processo, recomendo baixar a [AppImage](https://downloads.raspberrypi.com/imager/imager_latest_amd64.AppImage) para te salvar de dor de cabeça e perda de tempo com versões desatualizadas disponibilizadas por cada Distro.

Após baixar o arquivo AppImage, entre na pasta onde o arquivo foi salvo e execute-o com o comando: `sudo ./imager_2.0.4_amd64.AppImage`

**Passo a passo**:

1. Selecione o dispositivo que irá utilizar:
   ![exemplo](https://github.com/ChancellorMarko/manual_samba_raspberrypi/blob/main/img/1_screen.png)
2. Selecione seu sistema operacional de preferência, no meu caso utilizarei outros sistemas operacionais da própria Raspberry, como o Rasp OS Lite (64 bit):
   ![exemplo](https://github.com/ChancellorMarko/manual_samba_raspberrypi/blob/main/img/2_screen.png)
3. Insira as informações pertinentes de sua preferência conforme o que for solicitado.
4. Após configurar todas as informações pertinentes nas telas anteriores, você terá uma tela de acesso remoto que, dependendo da sua intenção, pode ser ou não interessante. Caso você queira acessar essa máquina remotamente para configuração via sua rede local (LAN), habilite a função SSH. Você tem duas opções para login via SSH:
   - Senha: com essa opção, você poderá escolher uma senha para realizar o login via sua rede, porém essa opção é insegura caso queira deixar essa máquina exposta na rede pública.
   - Chave SSH: uma das opções mais seguras caso queira deixar a máquina exposta na rede pública ou local.
   - Passo a passo: ![Configuração SSH](https://github.com/ChancellorMarko/manual_samba_raspberrypi/blob/main/Configura%C3%A7%C3%A3o%20SSH.md).
5. Caso queira, pode ativar a opção de Raspberry Pi Connect, no meu caso deixarei desativado.
6. Continue para a escrita das informações no cartão SD (**Atenção: todos os dados presentes nele serão apagados**).
   ![exemplo](https://github.com/ChancellorMarko/manual_samba_raspberrypi/blob/main/img/5_screen.png)

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

## Instalação do SAMBA

Com a máquina já operando pode ser realizada a instalação da ferramenta SAMBA através dos seguintes passos.

- **Ubuntu/Debian**

  ``` bash
    sudo apt install samba
  ```

- **Fedora/RHel**

  ``` bash
    sudo dnf install samba samba-common samba-client 
  ```

- **openSUSE**

  ```bash
      sudo zypper install samba samba-client
  ```

## Configurando o SAMBA

Para esta seção será realizada a configuração da aplicação, junto a passos anteriores para realizar estas configurações.

1. O primeiro passo é criar uma pasta para ser utilizada no armazenamento de arquivos:
  
```bash
    mkdir /mnt/backup/sambashare
```

A pasta pode ser criada em qualquer lugar que deseje como exemplo podemos criar na pasta `/etc/nome_de_sua_pasta` ou no meu caso como tenho discos externos montados para armazenamento `/mnt/local_de_montagem`.

2. Vamos editar as configurações do SAMBA e adicionar nossa pasta lá:

```bash
    sudo nano /etc/samba/smb.conf
```

No fim do arquivo vamos adicionar as nossas configurações:

```bash
[nome_de_sua_escolha]
   comment = Comentario sobre a sua pasta
   path = /local_escolhido/nome_da_pasta
   read only = no
   browsable = yes
   writeable=yes
   create mask=0777
   directory mask=0777
```

*Dica: CTR-O para salvar e CTR-X para sair da edição de arquivo.*

3. Reinicie o serviço do samba para ele reconhecer as novas configurações:

```bash
    sudo systemctl restart smbd
```

*Opcional*
4. Atualize as configurações de seu firewall para permitir o samba com:

```bash
    sudo ufw allow samba
```

5. Criar um novo usuário samba para fazer login e acessar sua pasta de forma segura:

```bash
    sudo smbpasswd -a nome-de-usuario
```

- **Nota: que o `nome-de-usuario` deve ser igual ao nome de usuário do sistema, caso contrário ele não será salvo**

## Conectar a pasta através do SMB

Utilize o seguinte endereço em seu explorador de arquivos ou ferramenta de conexão para acessar a sua pasta:

```bash
smb://ip-de-sua-maquina/nome_de_sua_escolha
```

Exemplo:

```bash
smb://192.168.0.122/NAS
```

Lembre se de usar o usuário e senha que você criou na etapa final do processo anterior.
