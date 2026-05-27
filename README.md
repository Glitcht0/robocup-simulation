# ⚽ RoboCup Simulation 2D

> Guia de configuração utilizando **GitHub Codespaces**

---

## 🚀 1. Importar o repositório

Acesse o GitHub e clique em **Create new...**

![Create new](readmeImgs/image.png)

Em seguida, selecione **Import repository**

![Import repository](readmeImgs/image-1.png)

> Ou acesse diretamente:  
> 👉 [https://github.com/new/import](https://github.com/new/import)

No campo **"The URL for your source repository"**, cole:

```bash
https://github.com/leandro-sobreira/robocup-simulation.git
```

![Begin import](readmeImgs/image-2.png)

Depois:

- Defina o nome do repositório
- Clique em **Begin Import**

![Begin import](readmeImgs/image-3.png)

⏳ Aguarde alguns minutos até a importação finalizar.

---

## 💻 2. Criar um Codespace

Dentro do repositório importado:

1. Clique em **<> Code**
2. Vá até a aba **Codespaces**
3. Clique em **Create codespace on main**

![Create codespace](readmeImgs/image-5.png)

⏳ O ambiente será criado automaticamente (pode levar alguns minutos).

> ⚠️ **Limites de uso:**
> - Plano gratuito: **60h/mês**
> - Plano educacional: **90h/mês**  
> 👉 [https://github.com/settings/education/benefits](https://github.com/settings/education/benefits)

---

## 🛠️ 3. Comandos e utilidades

### 🌐 Abrir interface gráfica (noVNC)

1. Vá até a aba **Ports**
2. Localize a porta **6080**
3. Clique em **Open in Browser**

![noVNC port](readmeImgs/image-6.png)

Uma nova aba será aberta com o uma interface gráfica (onde será aberto o **rcssmonitor** em breve)

---

### ⚙️ Rodar `rcssserver` e `rcssmonitor`

Execute em **terminais separados**:

```bash
rcssserver
```

```bash
rcssmonitor
```

---

### 🤖 Testar com Helios Base

Execute em **dois terminais diferentes**:

```bash
./helios-base/src/start.sh -t NomeDoTime
```

```bash
cd py2d/src
python3 start.py --team_name AdversarioUFGD --rpc-port 50052 --server-host 127.0.0.1 --server-port 6000
```

> ⚠️ Cada time deve possuir um nome diferente

---

# 🐍 Instalação do py2d

##

Ferramentas basicas

```bash
apt-get update
apt-get install -y git python3 python3-venv python3-pip fuse libfuse2 wget curl wget unzip

```

```bash
git clone https://github.com/CLSFramework/py2d.git
cd py2d
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

```

```bash
./generate.sh
cd scripts
sh download-proxy.sh
cd ..

```

### 1. Extrair os Binários

Dentro da pasta `scripts/proxy`, vamos "desmontar" os AppImages para que eles virem arquivos comuns:

Bash

```plaintext
# Extraia o jogador, o técnico e o trainer
./sample_player --appimage-extract
mv squashfs-root sample_player_dir

./sample_coach --appimage-extract
mv squashfs-root sample_coach_dir

./sample_trainer --appimage-extract
mv squashfs-root sample_trainer_dir


```

### 2. Criar Links Simbólicos

O script `start.py` vai continuar procurando pelos arquivos originais. Vamos enganar o sistema criando atalhos (links) que apontam para os executáveis reais que acabamos de extrair:

Bash

```plaintext
# Remove os arquivos que dão erro de FUSE
rm sample_player sample_coach sample_trainer

# Cria os links para os binários extraídos
ln -s sample_player_dir/AppRun sample_player
ln -s sample_coach_dir/AppRun sample_coach
ln -s sample_trainer_dir/AppRun sample_trainer

# Garanta que tudo é executável
chmod +x sample_player sample_coach sample_trainer


```

### Testar com py2d

```markdown
cd py2d
source venv/bin/activate
python3 start.py --team_name AdversarioUFGD --rpc-port 50052 --server-host 127.0.0.1 --server-port 6000

```
