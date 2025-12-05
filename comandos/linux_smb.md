#### Instalar o Samba

```bash
sudo apt update
sudo apt install samba
```
#### Configurar o Samba
1. Edite o arquivo de configuração do Samba:

```bash
sudo nano /etc/samba/smb.conf
```
2. Adicione a seguinte seção ao final do arquivo para compartilhar uma pasta:

```ini
[Compartilhamento$]
   comment = Pasta Compartilhada
   path = /caminho/para/sua/pasta
   browsable = yes # Permite que a pasta seja visível na rede
   guest ok = no # Não permite acesso anônimo
   read only = no # Permite escrita na pasta compartilhada
   create mask = 0755 # Permissões para novos arquivos
   directory mask = 0755 # Permissões para novos diretórios
   force user = seu_usuario # Substitua por seu nome de usuário do sistema
```

3. Salve e feche o arquivo e reinicie o serviço do Samba:

```bash
sudo systemctl restart smbd
sudo systemctl restart nmbd
```
#### Montar compartilhamento ( TEMPORARIO )
1. Crie um ponto de montagem:
```bash
sudo mkdir /mnt/compartilhamento
```
2. Monte o compartilhamento Samba:
```bash
sudo mount -t cifs //[IP_DO_WINDOWS ou PASTA_DO_LINUX]/[NOME_DO_COMPARTILHAMENTO] /mnt/compartilhamento -o username=[SEU_USUARIO_WINDOWS ou LINUX],password=[SUA_SENHA_WINDOWS ou LINUX],uid=$(id -u),gid=$(id -g)
```
#### Montar compartilhamento ( DEFINITIVO )
1. Edite o arquivo fstab:
```bash
sudo nano /etc/fstab
```
2. Adicione a seguinte linha ao final do arquivo:
```
//[IP_DO_WINDOWS ou PASTA_DO_LINUX]/[NOME_DO_COMPARTILHAMENTO] /mnt/compartilhamento cifs _netdev,username=root,password=SENHA,uid=33,gid=33,file_mode=0664,dir_mode=0775,nofail 0 0
```
* Se a senha tiver caracteres especiais escape usando \ antes do caractere especial.
* O correto a se fazer é criar um arquivo de credenciais para não deixar a senha exposta no fstab.
3. Salve e feche o arquivo.
4. Monte todos os sistemas de arquivos listados no fstab:
```bash
sudo mount -a
systemctl daemon-reload
```
#### Acessar compartilhamento via linha de comando
```bash
smbclient //[IP_DO_WINDOWS ou PASTA_DO_LINUX]/[NOME_DO_COMPARTILHAMENTO] -U [SEU_USUARIO_WINDOWS ou LINUX]
```
#### Criar credenciais para montagem
1. Crie um arquivo para armazenar as credenciais:
```bash
sudo nano /etc/samba/credenciais_smb
```
2. Adicione as seguintes linhas ao arquivo:
```
username=SEU_USUARIO_WINDOWS ou LINUX
password=SUA_SENHA_WINDOWS ou LINUX
```
3. Salve e feche o arquivo.
4. Altere as permissões do arquivo para que apenas o root possa lê-lo:
```bash
sudo chmod 600 /etc/samba/credenciais_smb
```
5. Atualize a entrada no fstab para usar o arquivo de credenciais:
```
//[IP_DO_WINDOWS ou PASTA_DO_LINUX]/[NOME_DO_COMPARTILHAMENTO] /mnt/compartilhamento cifs _netdev,credentials=/etc/samba/credenciais_smb,uid=33,gid=33,file_mode=0664,dir_mode=0775,nofail 0 0
```
6. Salve e feche o arquivo.
7. Monte todos os sistemas de arquivos listados no fstab:
```bash
sudo mount -a
systemctl daemon-reload
```
#### Listar pontos de montagem
```bash
mount | grep cifs
```
#### Desmontar compartilhamento
```bash
sudo umount /mnt/compartilhamento
```

