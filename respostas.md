# Respostas · Avaliação Prática de Docker · Cooperativa AgroVale (Turma A)

Nome: Gustavo Maschette
Matrícula: 26128314
Usuário do GitHub:maschettez
Usuário do Docker Hub: maschtz22

Responda com as suas palavras e com o que aconteceu na SUA máquina. Resposta curta e certa vale mais
do que texto longo copiado. Resposta que contradiz o seu próprio Dockerfile ou compose vale zero.

## Parte 1 · Dockerfile do portal

1. Qual imagem base você usou e qual o tamanho final da imagem do portal (saída de `docker images`)?
A imagem é nginx:1.27 -alpine com tag fixa na versão alpine.

2. Em qual pasta do container o Nginx procura os arquivos do site? Mostre o comando que você usou para
   conferir que o `index.html` está lá dentro.
   2. O Nginx procura os arquivos do site em `/usr/share/nginx/html`. Para conferir que o
   `index.html` está lá dentro:
docker exec portal ls /usr/share/nginx/html
A saída mostrou `index.html` e `estilo.css`.


## Parte 2 · Docker Hub

3. Nome completo da imagem publicada e link público do repositório no Docker Hub.
`maschtz22/agrovale-portal:1.0-26128314`
   Link público: https://hub.docker.com/r/maschtz22/agrovale-portal

4. Por que o `docker login` foi feito com um token de acesso e não com a senha da conta?
 Usei um token de acesso pessoal porque ele é mais seguro que a senha da conta. O token
   tem só as permissões que eu escolho (usei leitura e escrita), pode ser apagado ou
   desativado a qualquer momento no Docker Hub e, se vazar, eu revogo só ele, sem precisar
   trocar a senha nem perder acesso à conta. A senha da conta daria acesso a tudo
   (configurações, e-mail, cobrança), e o `docker login` pode deixar a credencial salva
   no computador ou no histórico do terminal.

## Parte 3 · Página de manutenção

5. Preencha uma linha por defeito encontrado. Defeito inexistente listado aqui desconta pontos.

| # | Instrução | O que estava errado | O que você viu acontecer | Como corrigiu |

| 1 |Fazer login no Docker Hub para publicar a imagem | O login inicial não estava funcionando corretamente com a senha|O Docker retornou erro de autenticação (unauthorized: incorrect username or password) |Foi utilizado um Personal Access Token (PAT) no lugar da senha e o login foi realizado novamente |
| 2 |Publicar a imagem no Docker Hub com o nome correto |A imagem precisava ser identificada com o namespace e repositório corretos |A imagem precisava ser publicada como maschtz22/agrovale-portal |Foi utilizada a tag maschtz22/agrovale-portal:1.0-26128314 e realizado o push para o Docker Hub |
| 3 |Deixar o repositório acessível para validação |O repositório precisava estar público para o avaliador conseguir acessar a imagem |A validação dependia do acesso ao repositório no Docker Hub |O repositório maschtz22/agrovale-portal foi configurado como público |

6. Qual a diferença entre `-p 7042:80` e `-p 80:7042` no `docker run`? Qual dos dois números é a porta do container?
A opção -p segue o formato porta_do_host:porta_do_container. Portanto, em -p 7042:80, a porta do container é 80. Já em -p 80:7042, a porta do container é 7042. O primeiro número sempre representa a porta do host e o segundo representa a porta do container.

## Parte 4 · docker-compose.yml

7. No serviço `blog`, por que `WORDPRESS_DB_HOST` recebe `db` e não `localhost`?
db é o nome do container/serviço do banco na rede Docker. localhost apontaria para o próprio container do WordPress, não para o banco.

8. Por que o serviço `db` não publica a porta 3306? Se precisar consultar o banco, como faz sem publicar
   a porta? Mostre o comando.
   O serviço db não precisa publicar a porta 3306 porque o WordPress acessa o banco pela rede interna do Docker. Para consultar o banco, pode executar um comando dentro do container:
docker compose exec db mysql -u root -p

## Parte 5 · Persistência

9. Quais comandos você usou para derrubar e subir a stack? Qual comando teria apagado o post que você criou,
   e por quê?
   Para derrubar a stack:
docker compose down

Para subir novamente:
docker compose up -d

O comando docker compose down -v apagaria também os volumes, podendo apagar o post criado, porque os dados do banco ficam armazenados no volume.

10. Código de conclusão impresso pelo verificador:

```
 AGROVALE-26128314-78B759D9
```
