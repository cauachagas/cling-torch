[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/cauachagas/cling-torch/master)

# Xeus Cling + libtorch (C++)

Este documento tem como referência este [link](https://krshrimali.github.io/Setting-Up-Xeus-Cling-Libtorch-OpenCV/) 

É bem simples instalar o Xeus Cling usando o Miniconda/Anaconda. Neste tutorial, mostrarei como instalar e criar um ambiente de desenvolvimento no Miniconda

Caso já tenha uma instalação do Miniconda/Anaconda, ignore o passo 1. Se o comando `conda` estiver variável `PATH` (se estiver, digita "cond" + "tab" que ele completará "conda"), ignore o passo 2.


### Passo 1 - Instalando Miniconda

```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O miniconda3.sh
bash miniconda3.sh -b -p ~/miniconda3
```
Este estes comandos acima instalarão o Miniconda (versão com Python3) em $HOME, 
sem carragar o ambiente de desenvolvimento automaticamente. 

### Passo 2 - Colocando os comandos no PATH

Se o usuário tiver permissão do root

```bash
sudo ln -s ~/miniconda3/bin/conda /usr/bin/
sudo ln -s ~/miniconda3/bin/activate /usr/bin/
```

Do contrário, é útil criar uma pasta que contenha só links simbólicos do que for 
de interesse pessoal e adicionar o conteúdo da pasta no `PATH` (Costumo usar desta forma). 

```
mkdir ~/bin
ln -s ~/miniconda3/bin/conda ~/bin
ln -s ~/miniconda3/bin/activate ~/bin
echo "export PATH=$"PATH":~/bin" >> ~/.bashrc
source ~/.bashrc
```

### Passo 3 - Criando um ambiente de desenvolvimento

É possível criar vários ambientes, com depêndencias ao seu critério. Um ambiente com o nome `cling-torch` é criado assim:

```bash
conda create -p ~/cling-torch xeus-cling notebook -c conda-forge
```
O comando acima irá criar o ambiente de desenvolvimento em `${HOME}/cling-torch`


### Passo 4 - Abrindo um Jupyter Notebook e escolhendo o Kernel

Abrar um novo terminal, ative o ambiente com `source activate ~/cling-torch` e digite `jupyter-notebook`

Note as opções disponíveis.

![](https://krshrimali.github.io/assets/kernels-available.png) 


### Passo 5 - Baixando o Libtorch


Usando um notebook com um Kernell C++ iremos primeiro baixar os [binários do Libtorch](https://pytorch.org/get-started/locally/) na primeira célula. 

```c++
system("/usr/bin/wget https://download.pytorch.org/libtorch/cpu/libtorch-cxx11-abi-shared-with-deps-1.6.0%2Bcpu.zip -O libtorch.zip")
```

Para facilitar, o arquivo `.zip` será extraído na mesma pasta do notebook.

```c++
system("/usr/bin/unzip libtorch.zip")
```

### Passo 6 - Carregando o Libtorch com Xeus Cling

Em outra célula, iremos apontar para o cling onde ficam as bibliotecas e cabeçalhos. Em seguida, iremos carregar o libtorch

```c++
#pragma cling add_include_path("./libtorch/include")
#pragma cling add_include_path("./libtorch/include/torch/csrc/api/include")
#pragma cling add_library_path("./libtorch/lib")
#pragma cling load("libtorch")
```

Agora o Cling está pronto para usar a API C++ do Pytorch. 

Veja o exemplo um exemplo mais completo, com o uso do `Autograd`, nesse [notebook](https://nbviewer.jupyter.org/github/cauachagas/cling-torch/blob/master/cling-torch.ipynb). Você pode testar sem instalar em sua máquina usando o [Binder](https://mybinder.org/v2/gh/cauachagas/cling-torch/master).

# Comentários

- A instalação foi feita em num sistema Linux, na versão CPU. Acredito que para sistemas OSX seja semelhante. 
- O `cling/xeus-cling` tem problema com [from_blob](https://github.com/jupyter-xeus/xeus-cling/issues/357).