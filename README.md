# Sistema de Notificação e Análise de Dados

Este repositório contém o material e código-fonte do trabalho desenvolvido durante a disciplina de GCC129 - Sistemas Distribuídos na Universidade Federal de Lavras (UFLA).

## Conteúdo

- **Código-Fonte:** Implementação do projeto em [linguagem de programação utilizada].
- **Instruções de Execução:** Passos para configurar e executar o projeto.

## Objetivos do Projeto

Este projeto envolve o desenvolvimento de um sistema de notificação e análise de dados, incluindo scraping de dados, limpeza de dados, visualização de gráficos e envio de notificações.
   
## Funcionalidades
- **Scrape usando Bash**: Scripts para download de dados.
- **Data cleaning usando Python**: Limpeza de dados com scripts Python.
- **Envio de dataset para servidor Node**: Criação e envio de dataset para o servidor.
- **Exibição de gráficos no servidor Node**: Visualização de gráficos gerados a partir dos dados.
- **Envio de notificações para eventos cadastrados**: Sistema de notificações via email.
   
## Como Usar Este Repositório

1. **Clone o repositório:**
    ```bash
    git clone https://github.com/OsvaldoUfla/Trabalho---Sistemas-Distribuidos-UFLA.git
    ```
2. **Navegue pelos diretórios** para encontrar o material do projeto.
3. **Siga as instruções** no arquivo `README.md` para configurar e executar o projeto.
  
# Construindo as imagens Docker para cada serviço.
---
Abra o terminal na pasta raiz do projeto e execute os comandos abaixo. 

node-server:

    docker build -t node-server-image -f ./node/Dockerfile ./node

python-server:

    docker build -t python-server-image -f ./python/Dockerfile ./python

## Criando rede para comunicação entre os server

    docker network create minha-rede

## Executando os Contêineres

Após construir as imagens, você pode executar os contêineres. Como o python-server é uma dependência do node-server, você deve iniciar o contêiner do python-server primeiro.

python-server:

    docker run -d --name python-server-container -p 5000:5000 -v $(pwd)/python:/app --network minha-rede python-server-image

-d: Inicia o contêiner em modo "desanexado" (em segundo plano, se desejar ver as saidas basta retirar -d).   
--name: Dá um nome ao contêiner para facilitar a referência.    
-p 5000:5000: Mapeia a porta 5000 do contêiner para a porta 5000 do host.     
-v $(pwd)/python:/app: Monta o diretório local ./python no diretório /app do contêiner.   
-- network minha_rede: conecta o server a minha_rede  

node-server:

    docker run -d --name node-server-container -p 3000:3000 -v $(pwd)/node:/app -v /app/node_modules --network minha-rede node-server-image

-d: Inicia o contêiner em modo "desanexado" (em segundo plano, se desejar ver as saidas basta retirar -d).   
--name: Dá um nome ao contêiner para facilitar a referência.   
-v /app/node_modules: Monta o diretório /app/node_modules dentro do contêiner para persistir as dependências.  
-- network minha_rede: conecta o server a minha_rede

## Acessando o serviço
Acesse o endereço http://localhost:3000/ em seu navegador.
Na página contém gráficos com o ranking de medalhas na olimpiadas de 2024.   
Há também um botão de redirecionamento para página de cadastro de email para receber notificações das novas medalhas.