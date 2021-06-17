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