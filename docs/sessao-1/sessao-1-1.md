# Sessão 1-1 - Primeiros Passos em Git e fluxo básico

## O que será aprendido?

- Comandos básicos do git:
    - git config 
    - git init
    - git status
    - git add
    - git rm
    - git commit
    - git push
    - git branch
- Funcionamento dos repositórios locais e remotos

## Pré-requisitos
- Git instalado
- Conta no GitHub

## Introdução

Enquanto estudava no curso técnico do IFTO, já mexia com alguns projetos. Com um nível de amadorismo muito maior do que atualmente, nós salvavamos os arquivos em um Google Drive compartilhado. Cada "versão" era uma pastazinha diferente.

Como pode imaginar, isso é algo ruim, nós tinhamos que logar nas nossas contas do drive nos comptuadores da escola, zipar a pasta do código, salvar nessa pasta, em casa ou em outro lugar. O processo de iniciar o projeto em outra máquina era tão demorado quanto.

Dessa forma, quero que você aprenda fundamentos básicos do Git! Uma ferramenta poderosissíma em compartilhar e versionar o código. 

Todos nós já ouvimos falar, porém, talvez você nunca tenha buscado muito estudar, pois pode se sentir um pouco ameaçado pela linha de comando e pela quantidade de conceitos desconhecidos, digo isso porque já me senti assim por bastante tempo. Espero que consiga tirar esse medo e ensinar a teoria de maneira sólida e a prática de forma divertida.

## Iniciando um repositório local

Antes de iniciarmos com alguma coisa relacionada ao projeto Quarkus, vamos brincar um pouco com um repositório. É bom você seguir os passos e criar seu próprio, com suas próprias customizações

A seguir é um exemplo de início de repositório:

```bash
git init <nome-do-seu-repositorio-muito-massa>
```

O comando é bem simples! Ele simplesmente cria uma nova pasta com o nome que você escolheu no comando e com uma pasta `.git`.


!!! info "⁉️Para não haver dúvidas"
    Arquivos e diretórios ocultos são iniciados por ".". Nos gerenciadores de arquivos do Linux e Windows, não são visíveis por padrão. </br> 
    Para ver os arquivos oculto, no Windows, no explorador de arquivos, vá em "exibir" e marque a checkbox de "Itens ocultos". </br> 
    No Linux o processo no explorador de arquivos é bem similar. Além disso são exibidos pelo comando 'ls -a' e acessados por um editor como o nano, vim, gedit ou o próprio vscode (com o comando code).

O git, sua IDE e aplicações como Github buscarão por `.git` para identificar que aquela pasta é um repositório local. **Nunca modifique esse diretório por você mesmo!**

Com o repositório local iniciado, crie arquivos dentro dele, crie também pastas e arquivos dentro dessas pastas. Pode ser uns arquivos de texto escrito "olá mundo", uma página web simples ou o próximo Instagram.

Eu fiz uma pequena aplicação web:
![alt text](image.png)

![alt text](image-1.png)
## Adicionando ao índice

Pense no repositório como algo separado dos seus arquivos salvos na pasta. Só porque os seus arquivos estão dentro de uma pasta que contem o `.git`, não quer dizer que eles estão dentro do repositório local.

Entretanto, é necessário realizar algumas ações antes de mudanças serem adicionadas ao repositório local, elas devem antes serem adicionadas a um lugar chamado **Git staging área**, **git index**, **área de preparação** ou **índice**. Iremos chamar de índice.

Imagine que o índice é uma lista de coisas a serem selecionadas para adicionar ao repositório. Por exemplo, fizemos os arquivos de js, css e html site da Nasa, mas talvez queremos só adicionar o html no repositório por enquanto. Dessa forma, a lista do índice pareceria algo assim:

- [x] index.html
- [ ] estilo.css
- [ ] js/hackNasa.js
- [ ] js/hackNasaDois.js

para marcar algo a ser preparado a entrar no nosso repositório local, é utilizado o seguinte comando: 
```bash
git add <arquivo ou diretório inteiro!>
```

Alguns exemplos:

```bash
# Adicionar apenas o index.html
git add index.html
```

```bash
# Adicionar o index.html e o estilo.css
git add index.html estilo.css
```

```bash
#Adicionar todos da pasta ./js
git add ./js
```

```bash
# Adicionar todos do diretório atual que estou
git add .
```

!!! info "⁉️Para não haver dúvidas"
    o "." tanto no Windows quanto no linux é um diretório oculto em toda pasta que aponta pro próprio diretório que está. Quando utilizamos "." isolado nos comandos, geralmente estamos nos referindo ao próprio diretório que estamos, por isso, o `git add .` traduz para "adiciona ao índice todas as coisas que temos a partir de onde eu estou!"

Geralmente, utilizamos com frequência o `git add .`.

**Por enquanto, dê git add em algum diretório inteiro ou em um ou mais arquivos.**

## O que tem no índice?

Adicionei algumas coisas novas ao repositório de exemplo (vamos chegar lá) e depois modifiquei uns arquivos, coloquei umas modificações no índice e outras não. Para visualizar o que tem ou não no índice, utilizamos o comando a seguir:

``` bash 
git status
```

Um exemplo do que ele pode retornar:

``` bash
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   index.html
        new file:   pagina3.html

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   estilo.css

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        js/
        pagina2.html
```

`Changes to be commited` são mudanças no índice que adicionei com o `git add` Você pode ver que no meu caso eu só modifiquei o arquivo `index.html` inseri de novo a `pagina3.html`.

`Changes not staged for commit` são mudanças que ocorreram mas não foram adicionadas ao índice com o `git add`.

`Untracked files` são arquivos que o índice nem sabe que existem. São arquivos novos.

O git fala que para removermos algo do índice, basta executar:

``` bash
git rm --cached <arquivo(s) ou diretórios separados por espaço>
```

## Adicionando ao repositório (finalmente!)

Após adicionarmos ao índice, podemos realizar o famoso commit para adicionarmos mudanças ao repositório.

Na maioria das vezes, o comando "commit" é acompanhado de uma opção "-m", e após ela deveremos inserir a mensagem. Observe abaixo:

```
git commit -m "adiciona nova página index.html"
```

!!! info "⁉️Para não haver dúvidas"
    Todo comando em sistema operacional pode vir acompanhado de opções. Elas são traçadas por `-` quando são apenas uma letra (como acima), ou `--` quando são uma palavra inteira, ex.: `--message`. Opções podem ser seguidas de parâmetros como `-m <mensagem>`. Um comando pode ter nenhuma a várias opções.

Lembre-se do status do índice mostrado anteriormente, principalmente das novas mudanças adiiconadas ao índice: modificações em `index.html` e adição de `pagina3.html`, a seguir, temos um exemplo de um commit sendo realizado com base na situação daquele status:

```bash
 # O meu comando
 > git commit -m "adiciona nova estrutura de pagina3.html e corrige cabeçalho de pagina1.html"

 #Saída
[master f8a3834] adiciona nova estrutura de pagina3.html e corrige cabeçalho de pagina1.html
 2 files changed, 1 insertion(+), 1 deletion(-)
 create mode 100644 pagina3.html

 #Logo depois, executo o git status:
 git status

 #Saída
 Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   estilo.css

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        js/
        pagina2.html 

```

Veja que as duas coisas que estavam no índice agora estão no repositório local, e por isso nem aparecem no status como modificadas ou ainda não adicionadas ao índice. Lembre-se que o Git rastreia mudanças com base no que existe no repositório!

Tente brincar com o `git add`, `git rm --cached` e `git commit`. É bem simples e legal!

## Iniciando o repositório remoto

Você deve ter notado que sempre foi dito "repositório local". Esse "local" não é de enfeite, ninguém sem acesso ao seu computador consegue acessar seu software super elaborado ainda. É para isso que existem repositórios remotos, o mais famoso é o GitHub.

Dessa forma, podemos pensar nesse cenário como 4 "caixas", a sua pasta que você está trabalhando, o índice, o seu repositório local e o repositório remoto que os outros podem acessar.

Imagem

Antes de adicionarmos ao repositório remoto, você tem que adicionar informações importantes ao git do seu computador. São uma espécie de váriaveis globais para o seu sistema operacional

``` bash
git config --global user.name "Seu Nome"
git config --global user.email "seu@email.com"
```

Dê o mesmo comando, mas com a opção list: 
```bash
git config --list
```

Com essa opção, você verá algumas variáveis do git, navegando com as setinhas, verá que estão salvos o seu nome e seu e-mail da conta GitHub.

Agora, crie um novo repositório remoto no GitHub! Lembre-se de criar sem nada, nem mesmo o README.md. Logo em seguida, você terá que gerá um **token de autenticação** para realizar alterações ao seu repositório remoto e diversas outras coisas.

Faça o seguinte fluxo: 

Sua foto de perfil -> Settings -> Personal Access Tokens -> Generate New Token -> Selecionar o repositório remoto que você criou -> Selecione pra ter acesso a tudo -> Gere o token -> copie o token e guarde-o.

Isso é importante por que o Git requer autenticação antes de você enviar mudanças do seu repositório local ao remoto. Quando isso ocorre, ele pergunta sua senha do GitHub, mas ele é um mentiroso e depois de inserir a senha ele fala que na verdade quer um token de autenticação e é esse token que você irá inserir.

Com essas considerações, temos que falar pro nosso repositório local que existe um repositório remoto irmão dele! Para isso, utilizamos o comando a seguir:

``` bash
git remote add origin <link do seu repositório GitHub, ex.: https://github.com/Valexzz/repositorio-massa>

```

Vamos entender cada parte do comando: 

`git remote` é um comando que vai mexer com algo de repositórios remotos.  

`add origin` o origin é importante, o Git precisa entender que há um repositório "origem" de um código, uma fonte primária. A fonte primária do nosso código deve ser no repositório remoto, não no nosso local, concorda? Nossos colegas e recrutadores irão pegar o código do remoto! 

Então existem dois "tipos" de repositórios nesse cenário, vários sem o origin: os locais dos nossos colegas ou recrutadores; e somente um com o origin: um que todos mudam, pegam e atualizam o código.

![alt text](<Sem título.png>)

O link claramente representa o seu repositório.

## Adicionando o repositório remoto

Para adicionar mudanças do nosso repositório local ao remoto, usamos o `push`:

``` bash
git push
```

Tente executar esse comando, o git vai te chamar de estranho e provavelmente vai retornar uma mensagem assim:

``` bash

fatal: The current branch master has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin main

```

o que ocorre é que o repositório remoto tem que ter uma entrada principal linkando ao nosso repositório local. Não é bem uma entrada, é uma "branch", vamos chamar de entrada enquanto não vemos sobre branches. 

Então temos que falar qual é essa entrada principal e executar o comando que ele pede. No seu Github, pode ver o nome dela, geralmente é main ou master, seu comando vai mudar com base no nome dessa entrada principal

```bash
git push --set-upstream origin <geralmente main ou master>
```

No comando de falha do push, você viu que é possível fazer esse comando "automaticamente", só ajustar a opção que o git fala com o `git config --global`. 

![alt text](image-2.png)

Nosso repositório também tem essa "entrada principal", é uma ideia configurar ele pra ser o mesmo nome da nossa entrada. Você pode ver o nome dela agora com o comando a seguir:

``` bash
git branch
```

E pode mudar o nome dela com o comando a seguir:

``` bash
git branch -m <nome-que-voce-quer-modificar> <nome-novo>
```

!!! tip "O que significa?" 
    O m da opção `-m` vem de "modify" = "modificar"

Agora, tente dar o push novamente, ele irá pedir senha, não insira sua senha, coloque o token que geramos atrás!

Sucesso! Agora temos um repositório remoto como origem do nosso local e podemos "lançar" as mudanças para ele para que nossos colegas possam acessá-las.

## Considerações Finais

Se você seguiu tudo nessa sessão, agora você sabe:

- Como iniciar um repositório local 
- O que é o índice de Git e como mexer com as mudanças feitas para ele
- Como enviar as mudanças do índice para o seu repositório local
- Como criar um repositório remoto
- Como configurar variáveis do git
- Como gerar um token de autorização para modificar repositório
- Como enviar as mudanças do seu repositório local para o repositório remoto
- A diferença entre sua pasta, a área de índice, o seu repositório local e o repositório remoto no GitHub 

Parabéns! Na próxima sessão, iremos trabalhar com o repositório do quarkus sobre a rede social, iremos criar outro a partir dele.

