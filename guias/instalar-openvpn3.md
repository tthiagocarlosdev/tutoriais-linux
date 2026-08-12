# Instalação e Uso do OpenVPN 3 no Linux

### OpenVPN 3 Linux Client - Guia Universal de Instalação e Uso

## 1. Verificar a versão e a base do sistema

Antes de iniciar, identifique a distribuição e a base do seu sistema operacional executando o comando abaixo. Isso ajudará a definir o repositório correto caso esteja em um sistema baseado em Debian/Ubuntu:

```bash
cat /etc/os-release
```

---

## 2. Tabela de Equivalência de Codinomes Oficiais (OpenVPN 3)

Se você utiliza sistemas baseados em **Debian ou Ubuntu** (como o Linux Mint), substitua a palavra informada nos comandos do repositório pelo codinome correto listado abaixo:

| Sistema Operacional      | Versão do Sistema | Base do Sistema      | Codename para o Repositório OpenVPN      |
| :----------------------- | :---------------- | :------------------- | :--------------------------------------- |
| **Linux Mint 22 / 22.x** | 22 / 22.1 / 22.3  | Ubuntu 24.04 (Noble) | **`noble`**                              |
| **Linux Mint 21 / 21.x** | 21 / 21.1 / 21.3  | Ubuntu 22.04 (Jammy) | **`jammy`**                              |
| **Ubuntu Linux**         | 24.04 LTS         | Nativo               | **`noble`**                              |
| **Ubuntu Linux**         | 22.04 LTS         | Nativo               | **`jammy`**                              |
| **Debian Linux**         | 12 (Stable)       | Nativo               | **`bookworm`**                           |
| **Debian Linux**         | 11 (Oldstable)    | Nativo               | **`bullseye`**                           |
| **Pop!_OS**              | 22.04 LTS         | Ubuntu 22.04         | **`jammy`**                              |
| **Kali Linux**           | Atual (Rolling)   | Debian Testing       | **`bookworm`** *(Geralmente compatível)* |

---

## 3. Instalação do Cliente

Escolha a seção correspondente à família do seu sistema operacional:

### Opção A: Sistemas baseados em Debian / Ubuntu / Linux Mint

Execute a sequência de comandos para adicionar a chave de segurança pública, mapear o repositório e efetuar a instalação via gerenciador de pacotes `apt`:

```bash
# 1. Baixar a chave GPG oficial do repositório
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://swupdate.openvpn.net/repos/openvpn-repo-pkg-key.pub | sudo tee /etc/apt/keyrings/openvpn3-keyring.asc

# 2. Adicionar o repositório (substitua "noble" pelo codenome da tabela se necessário)
echo "deb [signed-by=/etc/apt/keyrings/openvpn3-keyring.asc] https://openvpn.net noble main" | sudo tee /etc/apt/sources.list.d/openvpn3.list

# 3. Atualizar e instalar
sudo apt update
sudo apt install openvpn3
```

### Opção B: Sistemas baseados em Fedora / Red Hat / CentOS

Execute os comandos abaixo utilizando o gerenciador de pacotes `dnf` para instalar o plugin do repositório Copr mantido oficialmente para a plataforma OpenVPN 3:

```bash
# 1. Instalar o gerenciador de repositórios Copr
sudo dnf install dnf-plugins-core

# 2. Ativar o repositório oficial do OpenVPN 3
sudo dnf copr enable dszlin/openvpn3

# 3. Atualizar os índices e instalar o pacote do cliente
sudo dnf install openvpn3-client
```

---

## Como testar se funcionou?

Independentemente da distribuição utilizada, valide se a instalação terminou corretamente verificando a versão do executável básico no terminal:

```bash
openvpn3 version
```

O terminal exibirá detalhes sobre o release atual do OpenVPN 3 instalado na sua máquina.

---

## Como conectar à sua VPN?

O OpenVPN 3 gerencia as conexões de rede através de perfis importados e sessões rodando em segundo plano. Siga os passos práticos para iniciar:

### 1. Obter o arquivo .ovpn

Garanta que você tenha o arquivo de configuração (ex: `meu_perfil.ovpn`) salvo localmente.

### 2. Caminho do arquivo .ovpn

Caso não se lembre onde o arquivo foi baixado, você pode localizá-lo realizando uma busca na raiz do sistema:

```bash
sudo find / -name "*.ovpn" 2>/dev/null
```

### 3. Importar o arquivo de configuração (.ovpn)

Insira o comando abaixo fornecendo o caminho absoluto do arquivo. O parâmetro `--name` define um apelido customizado (as aspas evitam erros de leitura se houver espaços ou parênteses no nome do arquivo):

```bash
openvpn3 config-import --config "/caminho/do/seu/arquivo.ovpn" --name MinhaVPN --persistent
```

*\*`MinhaVPN` é o identificador (apelido) escolhido para a sua conexão.*

---

# Uso no Dia a Dia

Depois que o perfil de configuração já foi importado no sistema uma primeira vez, basta abrir o terminal para ligar e desligar a rede virtual sempre que precisar:

## 1. Como se conectar

Abra o seu terminal (**Ctrl + Alt + T**) e inicie a sessão utilizando o apelido que você definiu:

```bash
openvpn3 session-start --config MinhaVPN
```

*Se o servidor de destino exigir credenciais, o terminal solicitará seu **usuário** e depois sua **senha**.*

## 2. Como verificar o status

Para verificar as estatísticas de tráfego e garantir que a rede virtual está conectada corretamente, digite:

```bash
openvpn3 sessions-list
```

## 3. Como se desconectar

Ao terminar as suas tarefas e querer retornar para a navegação comum da sua internet padrão, finalize a sessão digitando:

```bash
openvpn3 session-manage --config MinhaVPN --disconnect
```

---

## Dica de Ouro: Criar Atalhos Rápidos (Opcional)

Se preferir não digitar toda a sintaxe do OpenVPN a cada uso, configure apelidos (*aliases*) automatizados no terminal do seu usuário:

1. Abra o arquivo de perfil do interpretador Bash:

   ```bash
   nano ~/.bashrc
   ```

2. Role até a última linha do arquivo e cole o bloco contendo as suas palavras-chave preferidas:

   ```bash
   alias ligar-vpn='openvpn3 session-start --config MinhaVPN'
   alias desligar-vpn='openvpn3 session-manage --config MinhaVPN --disconnect'
   ```

3. Grave o arquivo de texto pressionando **Ctrl + O**, confirme com **Enter** e saia utilizando **Ctrl + X**.

4. Atualize o seu terminal atual para aplicar as modificações:

   ```bash
   source ~/.bashrc
   ```

A partir deste momento, basta digitar apenas **`ligar-vpn`** para iniciar o túnel criptografado ou **`desligar-vpn`** para desativar a rede.

---

## Alterando o apelido cadastrado no OpenVPN 3

### 1. Remover o registro atual do gerenciador

Caso queira alterar o nome identificador, limpe o apelido antigo registrado na memória do cliente:

```bash
openvpn3 config-remove --config MinhaVPN
```

### 2. Importar definindo o novo nome preferido

Faça o mapeamento do arquivo novamente apontando o novo nome de identificação desejado (ex: `VPN_Trabalho`):

```bash
openvpn3 config-import --config "/caminho/do/seu/arquivo.ovpn" --name VPN_Trabalho --persistent
```

### 3. Atualizar os comandos no arquivo `.bashrc`

Abra o seu utilitário de atalhos e reajuste os aliases criados anteriormente:

```bash
nano ~/.bashrc
```

Altere as linhas finais do arquivo para que utilizem o novo apelido da configuração:

```bash
alias conectar='openvpn3 session-start --config VPN_Trabalho'
alias desconectar='openvpn3 session-manage --config VPN_Trabalho --disconnect'
```

### 4. Salvar e Aplicar

Grave com **Ctrl + O** (**Enter**), saia com **Ctrl + X** e recarregue as variáveis de ambiente:

```bash
source ~/.bashrc
```
