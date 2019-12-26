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
conda env create -n cling-torch
```

### Passo 4 - Instalando Xeus Cling e Pytorch

Criado o ambiente de desenvolvimento, basta ativá-lo e instalar os pacotes necessários

```bash
source activate cling-torch
conda install xeus-cling notebook pytorch-cpu -c conda-forge
```


### Passo 5 - Abrindo um Jupyter Notebook

Abrar um novo terminal, ative o ambiente (`source activate cling-torch`) e digite `jupyter notebook`

Note as opções disponíveis.

![](https://krshrimali.github.io/assets/kernels-available.png) 

### Passo 6 - Configurando Libtorch no Xeus Cling

Abra um jupyter notebook e na primeira célula do Jupyter coloque:

```c++
#pragma cling add_include_path("$HOME/miniconda3/envs/cling-torch/lib/python3.7/site-packages/torch/include")
#pragma cling add_include_path("$HOME/miniconda3/envs/cling-torch/lib/python3.7/site-packages/torch/include/torch/csrc/api/include")
#pragma cling add_library_path("$HOME/miniconda3/envs/cling-torch/lib/python3.7/site-packages/torch/lib")
#pragma cling load("libtorch")
```

Suponho que a instalação do Miniconda está em $HOME, então basta usar como a seguir. Do contrário, deve ajustar de acordo.


Agora o Cling está pronto pra usar a API C++ do Pytorch.

# Comentários

A versão do Pytorch instalada é a `1.1.0`. Atualmente, a versão estável é a `1.3.1`. Como o `Xeus Cling` é distribuido pelo conda-forge, acredito que as libs do pytorch são mais compatíveis com a versão do conda-forge (1.1.0), pois devem ser compiladas seguindo um mesmo padrão. 

Usando o libtorch da página oficial, tentei reproduzir os exemplos da documentação na versão dita estável, mas somente tive sucesso com as versões `1.1.0 e 1.2.0`. Como o objetivo do tutorial é usar a API C++ do PyTorch no Jupyter, não julguei necessário usar a versão mais atual, que não deve ter mudado muito. 

Caso queira usar uma versão mais recente, sugiro tentar instalar usando o código-fonte (o que pode demorar algumas horas). Veja exemplos de como fazer:


https://medium.com/repro-repo/build-pytorch-from-source-on-ubuntu-18-04-1c5556ca8fbf
https://kezunlin.me/post/54e7a3d8/
