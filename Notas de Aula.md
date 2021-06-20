# Notas de Aula - Docker

## Rodar um novo container 

* docker run <nome_da_imagem>
* docker run -it <nome da imagem> (o "-it" é um parâmetro para solicitar que o container forneça um terminal integrado - modo iterativo. Assim, temos executar comandos no container)
  
  * Por exemplo, o comando abaixo roda um container Ubuntu e fornece um terminal no qual podemos executar comandos Linux para o Ubuntu que está sendo executado neste container:
  ``` Dockerfile
    docker run -it ubuntu
  ``` 
* docker run -d <nome_da_imagem> (o "-d" é uma flag para rodar em background, "detached"). Por exemplo,

    ```docker
        docker run -d nginx
    ```

## Reiniciar um container

* O comando **_docker run_** cria um novo container. Para iniciar um container já existente, deve ser utilizado o comando **_docker start <id_do_container>_**.

## Parar um container

* docker stop <nome_do_container || id_do_container>

    Para obter o nome ou id do container para isso, usando o comando **docker ps** e verificando o nome do container na resposta.

    ``` docker
        docker stop brave_lewin
    ```

    ``` docker
        docker stop 2d1756f7e8e7
    ```

## Nomeando um container

* É ideal que um container receba um nome que o identifique, evitando que ele receba um nome genérico. Isso pode ser feito com o auxílio da flag **_--name_**:

    ```docker
        docker run -d -p 80:80 --name nginx_default nginx
    ```

## Verificar containers executados
* Podem ser utilizados:

    ```docker
        docker ps
        docker container ls
    ```

Se utilizarmos a flag **-a**, ele mostra todos os containers que já foram executados:
    ```docker
        docker ps -a
    ```

## Expor Portas

* Containers são isolados, e não possuem conexão com nada externo à eles;
* Para termos acesso externamente, devemos expor portas:
    ```docker
        docker run -d -p 80:80 nginx
    ```
    onde a sintaxe é:

    **docker run -p <porta_do_pc>:<porta_do_container> <nome_do_container>**

    No comando acima, o container nginx será executado, e sua porta 80 estará exposta na porta 80 do meu computador (porta padrão). Além disso, o container rodará em background, por conta da flag **-d** (detached).

## Expor logs de um container
* docker logs <id_do_container>
* AS últimas ações realizadas no container serão retornadas: 
    ```docker
        docker logs 584413ef25f7
    ```

## Remover Containers
* Para remover um container da máquina, é utilizado o comando **_docker rm [<id_do_container>]_**:

    ``` docker
        docker rm 8528847e4507
    ```

* Se o container estiver sendo executado, pode ser usada a flag **_-f_** (force):

    ``` docker
        docker rm 8528847e4507 -f
    ```

## Criar uma imagem

* Para definir uma imagem, criamos um Dockerfile. Exemplo: 

    ``` docker
        FROM node

        WORKDIR /app

        # Copiando o package e o package-lock (por isso o asterisco) para a workdir (indicada pelo ponto, que poderia ser substituído por '/app')
        COPY package*.json .

        RUN npm install

        # "Copia tudo (primeiro ponto) para o meu container (segundo ponto)"
        COPY . .

        # Porta do container a ser exposta
        EXPOSE 3000

        CMD ["node", "app.js"]
    ```    
* Para criar uma imagem, na pasta onde o Dockerfile estiver definido:
    ```powershell
        docker build .
    ```
    ou

    ```powershell
        docker build <-caminho-diretorio>
    ```

## Alterar uma imagem

* Sempre que uma alteração for feita, é preciso fazer o build novamente.

* Para o Docker, é como se fosse uma imagem completamente nova.

* Normalmente será bem rápido. Como as layers estão salvas em cache, uma modificação nos arquivos de um projeto representaria apenas uma mudança a partir da layer de _COPY_ do exemplo de Dockerfile acima.

## Camadas das Imagens

* AS imagens do Docker são divididas em camadas;

* Cada instrução no Dockerfile representa uma layer. As layers são salvas em cache para uma atualização mais rápida (em novos builds).

## Download de Imagens

* É possível fazer o download de uma imagem do Docker Hub e deixá-la disponível no ambiente de desenvolvimento:

    ```docker
        docker pull <imagem>
    ```

    Exemplo:

    ```docker
        docker run python
    ```

## Ajuda com os Comandos

* Para ter ajuda sobre qualquer comando Docker, basta inserir a flag _**--help**_ após inserir qualquer parte do comando. Exemplos:

    ``` docker
        docker --help
    ```
    
    ``` docker
        docker run --help
    ```

## Modificando a Tag de uma Imagem

* É possível criar tags personalizadas para uma imagem:

    ```docker 
        docker tag <id-imagem> <nome-imagem>:<tag>
    ```
## Nomeando e Criando tag no build

* Para simplificar o processo de nomear uma imagem e criar uma tag para ela, é possível fazer isso no build de uma imagem:

    ``` docker 
        docker build -t <nome_imagem>:<tag_imagem> .
    ```
## Reiniciando Container com Iteratividade

* Não é necessário rodar um" _docker run ..._" para trocar o container de detached para iteractive. Basta executar o comando docker start para um container já existente, passando a flag _-i_:

    ```docker
        docker start -i <nome-container>
    ```

## Remover Imagens
* Para remover uma imagem da máquina, é utilizado o comando **_docker rmi [<id_da_imagem>]_**:

    ``` docker
        docker rm 8528847e4507
    ```

* Se a imagem estiver sendo utilizada por um container, pode ser usada a flag **_-f_** (force):

    ``` docker
        docker rmi -f 8528847e4507
    ```

## Limpando o Sistema

* Para remover imagens, containers e networks não utilizadas:

```docker 
    docker system prune
```
## Remover um container após a sua utilização

* Para remover um container no momento que ele for parado, basta que ele seja criado utilizando a flag _**--rm**_ da seguinte forma:

``` docker 
    docker run --rm <container>
```
## Copiar arquivos de um container

* Um comando útil para copiar arquivos de um container para a máquina (e vice versa) é o _**cp**_:

    ``` docker
        docker cp <nome-container>:<caminho-relativo-container> <caminho-relativo-maquina> 
    ``` 
    Exemplo:

    ``` docker
        docker cp meucontainer:/app.app.js ./copiafolder
    ``` 