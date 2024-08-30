# Imagem Docker por sistema
   - Intercâbmbio Itajubá
     ```bash
     softwarespti/ubuntu22-apache-php81
     ```
   - Protocolo Itajubá
     ```bash
     softwarespti/ubuntu-apache-php5.6
     ```
   - Protocolo Sul Mineira (PA)
     ```bash
     softwarespti/ubuntu22-apache-php81
     ```
   - Protocolo Clube Itajubense
     ```bash
     softwarespti/ubuntu-apache-php8.0
     ```
   - Protocolo Amparo
     ```bash
     softwarespti/ubuntu-apache-php5.6
     ```
   - Protocolo Farmácia Amparo
     ```bash
     softwarespti/ubuntu-apache-php5.6
     ```
   - Site Coperciso
     ```bash
     softwarespti/ubuntu-apache-php8.0
     ```

# Tutorial: Instalação do WSL Ubuntu com Docker via Linha de Comando

Este tutorial orienta a instalação do WSL (Windows Subsystem for Linux) com Ubuntu e configuração do Docker, sem o uso do Docker Desktop.

## Passo 1: Habilitar o WSL e o WSL 2

1. **Abrir o PowerShell como Administrador**:
   - Clique com o botão direito no menu Iniciar e selecione **Windows PowerShell (Admin)**.

2. **Ativar o WSL e a Virtualização**:
   - Execute os seguintes comandos no PowerShell:
     ```powershell
     dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
      ```
     ```powershell
     dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
     ```

3. **Definir o WSL para usar a versão 2**:
   - Após a reinicialização do computador, execute o comando abaixo no PowerShell para definir o WSL 2 como a versão padrão:
     ```powershell
     wsl --set-default-version 2
     ```

## Passo 2: Instalar o Ubuntu no WSL

1. **Instalar o Ubuntu**:
   - Execute o seguinte comando no PowerShell para instalar o Ubuntu diretamente da Microsoft Store:
     ```powershell
     wsl --install -d Ubuntu
     ```
   - Se a instalação não iniciar automaticamente, digite `ubuntu` no terminal para configurá-la e crie um novo usuário e senha.

## Passo 3: Instalar o Docker no Ubuntu WSL

1. **Atualizar os Repositórios do Sistema**:
   - No terminal do Ubuntu (dentro do WSL), execute:
     ```bash
     sudo apt update
     ```
     ```bash
     sudo apt upgrade -y
     ```

2. **Instalar Dependências para o Docker**:
   - Instale as dependências necessárias:
     ```bash
     sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
     ```

3. **Adicionar o Repositório Oficial do Docker**:
   - Adicione a chave GPG e repositório do Docker:
     ```bash
     sudo apt-get update
     ```
     ```bash
     sudo apt-get install ca-certificates curl
     ```
     ```bash
     sudo install -m 0755 -d /etc/apt/keyrings
     ```
     ```bash
     sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
     ```
     ```bash
     sudo chmod a+r /etc/apt/keyrings/docker.asc
     ```
     ```bash
     echo \
       "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
       $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
       sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
     ```

4. **Instalar o Docker**:
   - Atualize o índice de pacotes e instale o Docker:
     ```bash
     sudo apt update
     ```
     ```bash
     sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
     ```

5. **Adicionar o Usuário ao Grupo Docker**:
   - Adicione seu usuário ao grupo Docker para executar comandos sem `sudo`:
     ```bash
     sudo usermod -aG docker $USER
     ```
   - Saia e entre novamente no terminal para que a mudança tenha efeito.

## Passo 4: Configurar o Docker para Rodar no WSL

1. **Configurar o Docker para Rodar no WSL 2**:
   - O Docker precisa ser configurado para rodar com o backend do WSL 2. Crie ou edite o arquivo `/etc/docker/daemon.json`:
     ```bash
     sudo nano /etc/docker/daemon.json
     ```
   - Adicione o seguinte conteúdo:
     ```json
     {
       "hosts": ["unix:///var/run/docker.sock"],
       "iptables": false,
       "storage-driver": "overlay2"
     }
     ```
   - Salve o arquivo e feche o editor (`Ctrl + O`, `Enter`, `Ctrl + X`).

2. **Iniciar o Serviço Docker**:
   - Instale e configure o serviço `systemd-genie` para gerenciar serviços systemd:
     ```bash
     sudo apt install systemd-genie -y
     sudo genie -s
     ```
   - Inicie o Docker manualmente:
     ```bash
     sudo service docker start
     ```

3. **Testar o Docker**:
   - Verifique se o Docker está funcionando corretamente:
     ```bash
     docker --version
     docker run hello-world
     ```

## Conclusão

Você agora configurou o WSL 2 com o Ubuntu e instalou o Docker sem o uso do Docker Desktop. Com isso, você pode executar containers diretamente no ambiente Linux integrado ao Windows.
