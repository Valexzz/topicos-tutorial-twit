# Sessão 1-2 - Conceitos colaborativos do Git

## O que será aprendido?

- Comandos básicos do git:
    - git clone
    - git branch
    - git checkout
    - git fetch
    - git merge
    - git pull
- Conceitos de branches
- Fork
- Pull request
- Melhores práticas de commits, branching e merge

## Pré-requisitos
- Git instalado
- Conta no GitHub

## Introdução

Beleza, fizemos sozinho nosso software da Nasa. Mas nosso colega quer fazer um software para SpaceX e quer o nosso código da Nasa como base. 

Um outro não é amigo, é um estranho, mas ele é um estranho legal, ele quer ajudar no nosso código! Mas mesmo assim, você não daria acesso total a um estranho né? Teria que avaliar o que ele quer trazer de novo pro seu código...

## Simulação de um colega

Talvez seja pedir muito, mas é importante pra aprendermos melhor. Você pode muito bem simular um fluxo de outro colega trabalhando no seu repositório remoto a partir de outra conta no GitHub. Então pegue aquele seu e-mail institucional, ou aquele que você fez para ter 30 dias grátis no Spotify, ou até mesmo aquele feito para você fazer um Elo Job no LoL.

Marque o repositório remoto que você fez como público, mas por enquanto não insira colaboradores.

## Clonando um repositório

Antes de prosseguir, entre com sua conta secundária. Agora você é o estranho amigável/seu amigo

Lembre-se que os repositórios do github são marcados como "origem". Sendo assim, o repositório que nós criamos, se for público, poder ser clonado por qualquer um. Isso não quer dizer que ele pode ser alterado por todo mundo. 

Ao clonar um repositório, você está fazendo um repositório local que é linkado ao o repositório remoto. A partir de agora, toda alteração que você fizer será considerada a partir da última alteração enviada ao repositório remoto quando você deu o clone.

O comando é bem simples:

```bash
git clone <link-do-repositorio>
```

Isso cria uma pasta com o mesmo nome do repositório remoto e insere os arquivos do repositório, assim como o .git. Se quiser alterar o nome do diretório a ser criado ou quer inserir num diretório já existente, acompanhe a sintaxe:

```bash
git clone <link-do-repositorio-remoto> <./nova-pasta-ou-pasta-ja-existente>
```

## Tentando algo...

Faça alguma alteração nesse repositório local, e tente dar um git push. Você verá o seguinte:

``` bash
remote: Permission to nasa/site-da-nasa.git denied to <seu-usuario>.
fatal: unable to access 'https://github.com/nasa/site-da-nasa.git/': The requested URL returned error: 403
```

Você não tem permissão, infelizmente você fez um software não é contribuidor!! E então...

- Se formos o amigo, como podemos criar nosso próprio site da SpaceX? Nossa equipe não consegue trabalhar sem ter um repositório remoto com acesso.
- Se formos o estranho amigo, como podemos trabalhar em mudanças de uma boa forma e enviar para o repositório remoto? 

Para responder as duas perguntas acima, iremos rebobinar um pouco e seguir outro fluxo.

## O Fork

Um garfo tem dois, três, quatro, até cinco dentes. No geral, quando um garfo é criado, se nada for alterado, seus dentes são todos iguais. Entretanto, podemos pintar um, entortar o outro e deixar um como está.

A partir dessa analogia, podemos compreender que o processo de fork é a cópia de um repositório público remoto para um repositório remoto próprio para uso qualquer. O repositório é de autoria nossa, portanto podemos modificar e adicionar contribuidores da forma que quisermos, isso não irá alterar o repositório no qual copiamos. 

Para fazer um fork, na sua conta secundária, vá no link do repositório e clique em "fork". Escolha um nome e selecione a checkbox de copiar somente o branch `master` ou `main`. Posteriormente, você pode definir se o repositório é privado ou público.

Agora, repita o processo de clone anteriomente, dessa vez considerando o repositório forkado da conta secundária.

## Teoria: Usos da Branches

Caso não goste muito da parte teórica e já conheça os usos da branches, pule essa sessão para a parte prática.

Até agora, chamamos a branch main/master de "entrada principal". Entretanto, vamos mudar para o termo real, que faz tanto sentido quanto: branch; um galho, ramificação! Pense agora realmente como uma árvore, onde a branch main é o tronco.

Para ser uma ferramenta poderosa de versionamento, o Git possui a funcionalidade de branches, iremos focar em duas facilitações que as branches trazem:

- Trabalho em um único projeto por múltiplas pessoas

- Incentiva e disciplina a melhor organização e modularização de alterações de um código. 

Podemos também ver a utilidade das branches ao analisar cenários hipotéticos em que elas simplesmente não existem.

### Cenário sem branch: Trabalho por múltiplas pessoas

Imagine o seguinte: somos o amigo da SpaceX, e 3 pessoas da nossa equipe estão mexendo no projeto, a seguir estão as tarefas:

- Colega 1: Integração de autenticação com o Google
- Colega 2: novo estilo para a área de login
- Colega 3: Só trocar a logo

Para vermos o problema de não ter branch, basta ver o histórico de commits da equipe depois das três funcionalidades estarem prontas:
``` bash
Colega 2: Cria nova cor para de tema para o site
Colega 2: Modifica padding, margin e borda dos botões
Colega 1: Cria novo service para cuidar da autenticação
Colega 2: Ajusta tamanho e fonte do texto principal do menu de login  
Colega 3: Cria nova imagem de logo e muda nome da imagem para Spacex.png
```
    O colega 2 fica confuso o porquê a imagem parou de carregar, será que ele fez algo errado?
``` bash
Colega 1: Modifica rotas para login de autenticação Google
```
    Agora, o colega 2 está novamente confuso por não conseguir mais logar...

Como podemos ver, ter três pessoas enviando modificações a uma única fonte de código cria diversos problemas de concorrência!

### Cenário sem branch: Organização de alterações do código.

Imagine agora que somos o estranho amigo que quer adicionar funcionalidades no nosso código. Começamos então a criar uma página que permite hackear o pentágono também. Veja só o histórico de commits:

``` bash
estranho-legal: cria estrutura da pagina de hackear pentagono
estranho-legal: cria página de autenticação diferente para hackear o pentágono
estranho-legal: modifica layout para página
estranho-legal: adiciona rotas para página de hackear o pentagono
estranho-legal: adiciona novas alterações no banco de dados
estranho-legal: cria função de hackear pentagono
estranho-legal: testes de hack
```

Nós então propomos essa alteração no repositório principal. Entretanto, temos alguns problemas:

- O dono do repositório não entende muito bem nossa linha de raciocínio, ele acha que fomos bagunçados, mexendo com muitas coisas diferentes ao mesmo tempo.
- O dono identificou um problema nas modificações do frontend, mas está tudo certo no back, ele queria muito muito que essas alterações viessem mais separadas!

### A solução do trabalho em equipe

No primeiro cenário, se cada colega fizesse uma branch diferente, a partir da branch principal, seus commits não entrariam em conflito, é como se cada commit estivesse em uma "pasta":

``` bash
Colega 1: 
    commit 1, commit 2, commit 3...
Colega 2:
    commit 1, commit 2, commit 3...
Colega 3:
    commit 1, commit 2, commit 3

```

Se o colega 3 e colega 1 terminasse e mandasse para o branch principal primeiro, o colega 2 já veria as mudanças e se adaptaria melhor a elas. Em um cenário mais ideal, o colega 2 analisaria as mudanças do colega 1 e colega 3, e bateria na cabeça deles:

    Não muda o nome das rotas por favor! Não muda o nome da imagem!!

O histórico de commit seria bem mais organizado:

``` bash
# BRANCH MAIN
...mudanças anteriores da branch main

Commits do colega 3 (mudança na logo)
...
Commits do colega 2 (mudança no estilo da página de login)
...
Commits do colega 1 (adição de serviço de autenticação do Google)
...

```

### A solução da organização

No cenário do amigo estranho, coaches diriam para ele ter uma maior disciplina para organizar o código corretamente tomando banho gelado às 5 horas da manhã e correndo 4 km diariamente. Entretanto, somos muito indisciplinados e podemos usar branches para nos ajudar. 

Toda vez que criarmos uma nova mudança, devemos ter a boa prática de criar uma branch nova a partir da principal que o código está sendo produzido e assim, ir inserindo os commits, isso não só organiza nossa mente, como organiza nosso histórico:

``` bash

#Depois de criar a branch "hackear-pentagono-frontend" a partir de main
estranho-legal: commits do frontend da página de hackear pentagono...

#Depois de criar a branch "hackear-pentagono-backend" a partir de main
estranho-legal: commits das rotas e serviços da página de hackear pentagono...

#Depois de criar a branch "hackear-pentagono-bd" a partir de main
estranho-legal: commits do banco de dados da página de hackear pentagono...

```

## Criação de branches

Primeiramente, devemos selecionar uma branch para estar antes de criar uma. Devemos fazer isso com o comando switch ou checkout:

``` bash
git switch branch-existente
```

``` bash
git checkout branch-existente
```

O comando checkout é um comando mais antigo, mas pode fazer as mesmas coisas do switch, o switch foi criado para separar as responsabiliaddes quando foi percebido que o checkout faz... muitas coisas!

Para criar novas branches a partir da qual estamos, podemos usar a opção -c no switch, ou o -b no checkout. Ambas criam a branch ou entram na branch ja existente se existir:

``` bash
git switch -c nova-branch-ou-branch-existente
```

``` bash
git checkout -b nova-branch-ou-branch-existente
```

Lembre-se, você cria uma nova branch com base nas mudanças da branch da qual você está. Pense realmente como ramos de árvore:

Imagem

Siga a boa prática: pensou em uma funcionalidade para a aplicação? Crie uma branch!**A branch main deve ser a sua aplicação com mínimos erros, completa, funcinal!**

Com a conta secundária, pense em uma pequena funcionalidade pro seu projeto, depois crie uma nova branch com um nome descritivo pra essa funcionalidade a partir da branch main no seu repositório.

## Commits em branches: Boas práticas

Na branch criada a partir da main, desenvolva o fluxo que foi ensinado na seção anterior: add, commit, push. E siga as boas práticas:

- Crie commits atômicos, pode realmente ser desde algo mínimo a algo mais complexo, o importante é: se der pra ser dividido, faça a partir do add. Se seu commit não pode ser todo explicado com menos de 50 palavras, talvez possa ser dividido. 
- Tente ao máximo manter os commits dentro do contexto de mudanças/adições que sua branch propõe. Se ela propõe a mexer com o estilo da página de login no frontend, é muito estranho você mudar arquivos relacionados a regras de negócio no backend, ou até mesmo mudanças em outras páginas.
- Se a sua branch propor a fazer algo mais complexo ou que pode ser bem dividido, tente destrinchar em mais branches dentro dessa própria branch!

Para simular um cenário com mudanças maiores, faça uma ou mais branches dentro da branch que você criou, faça mesmo que sejam coisas bem simples, é só para simular.

## Integrando as funcionalidades

Depois de fazer as mudanças na sua "sub branch" e terminar o que deve ser feito, você deverá integrar ela à branch que você criou.

O git pensa dessa forma: **Na branch que você está**, você pode selecionar outra para ser integrada. Isso resulta num comando com a seguinte sintaxe:

``` bash
git merge sub-branch
```

Execute isso na branch principal que você criou para adiconar uma funcionalidade. Dê um `git log --oneline --graph --all` E veja o que aparece!

As alterações de uma outra branch foram mescladas a esta. Assim, é como se seguissemos uma linha reta de mudanças. Mas de início, era uma ramificação.

Se houver mais "sub-branches" dentro dessa branch principal de funcionalidade, dê os gits merges delas.

## F5 no repositório remoto

Você criou a branch e fez os merges. Se você for uma pessoa normal, 6 horas de letras, códigos, números te cansam demais, você entã;o descansa e volta no outro dia.

No outro dia, você vê que nada foi feito no repositório remoto a partir do seu git local! Isso por que ele ainda não foi **atualizado** das últimas mudanças remotas.

Para darmos um refresh em como o git do nosso computador vê as mudanças remotas, devemos utilizar o **fetch**. O fetch **NÃO** muda seu trabalho local, ele apenas dá um refresh para seu computador sair do mundo e ver as novas atualizações lá fora. A seguir, está a sintaxe, lembre-se que origin vai indicar o repositório remoto:

``` bash
git fetch origin
```

## F5 no seu repositório

Imagine o seguinte: o seu colega está no mesmo branch que o seu. Ele faz algumas mudanças, você faz outras... Se ele fizer o commit e mandar pro repositório remoto antes de você, não só você terá que atualizar sua máquina para ver novas mudanças do remoto, como atualizar os seus arquivos locais! 

É isso que o pull faz, ele dá um git fetch e puxa as mudanças mais recentes feitas na branch que você está trabalhando. A seguir a sintaxe:

``` bash
git pull
```
## Pull Requests

Vamos considerar os dois cenários dos nossos personagems secundários após terminar a branch de funcionalidade:

- Se formos da equipe da SpaceX, devemos enviar para análise nossa funcionalidade para outros membros da equipe, antes de ser enviada para a branch principal.

- Se formos o estranho legal, devemos enviar para análise nossa funcionalidade para o dono do repositório original.

Em ambas as situações, utilizamos as pulls requests. 

Uma pull request é uma requisição feita aos membros do repositório para dar merge de uma branch a outra. Geralmente é feito para adicionar funcionalidades na branch main.

No GitHub, vá no repositório que quer adicionar a funcionalidade, selecione a branch que quer adicionar essas funcionalidades e selecione uma branch do seu repositório para ser integrada. 

Logo em seguida, você pode descrever o que foi feito e enviar, colegas ou o dono do repositório irão revisar, você poderá editar sua branch com base nas sugestões, e outros até mesmo poderão criar uma nova branch e pedir uma outra pull request para sua branch. Veja:

IMAGEM

Esse é o mesmo procedimento tanto para o time do amigo da SpaceX quanto para o estranho legal. A diferença é que um é no mesmo projeto, o outro é de um fork.

## Relembrando boas práticas

Vamos reforçar as boas práticas que foram mostradas ao longo dessa sessão:

### Crie branches para adicionar novas funcionalidades

Errado:

git switch main

git add . => git commit "adiciona e estiliza caixa da página de login" => git push

git add . => git commit "adiciona campos de e-mail e senha estilizados" => git push

git add . => git commit "adiciona validações no formulário" => git push

Correto:

git switch main

git switch -c pagina-login-frontend

commits da página de login...

### Se uma branch fizer muita coisa, divida em mais branches

Errado:

git switch main

git switch -c sistema-login

commits que mudam o frontend...

commits que mudam o banco de dados...

commits que mudam o backend...

Certo:

git switch main 

git switch -c sistema-login

git switch -c sistema-login/frontend

commits de frontend...

git switch -c sistema-login/banco

commits de banco...

git switch -c sistema-login/backend

commits do backend...

### Faça mensagens descritivas

errado:

"conserta bug no login"

"Cria tabelas no banco de dados relacionada ao login"

Certo:

"Modifica enviarLogin para usar um objeto 'usuario' constante

Existia um bug que fazia com que o usuário sempre entrasse, mesmo com o login errado"

"Cria tabela de usuário no banco de dados para o sistema de login"

### Faça commits atômicos

Commits não devem conter muita coisa de uma só vez, se for o caso, tente dividir com o add e commite as divisões.

Isso anda junto com as mensagens descritivas, e até facilita elas a falarem o que fizeram de maneira mais fácil.

Errado:

git add .

git commit "cria campos de login no frontend, cria layout na página de login, insere validação no frontend dos campos de login, insere rotas de login no backend, cria nova tabela usuario no banco"

(+ 800 linhas modificadas, -200 linhas removidas, 20 arquivos alterados)

Certo:

git add <tudo que foi modificado relacionado aos campos de login>

git commit "cria campos de login"

(+ 30 linhas adicionadas, 1 arquivo modificado)
git add <tudo que foi modificado relacionado à validação dos campos de login>

git commit "cria validação dos campos de login"
(+ 20 linhas adicionadas, -5 linhas removidas, 1 arquivo modificado)

## Usando seu editor de texto/IDE

O Vscode e IDEs como as do Jetbrains possuem uma interface gráfica muito intuitiva que mascaram o terminal ameaçador do git a partir de telas, apresentações, textos e listas bonitas. 

**APRENDA** a usar essas ferramentas disponibilizadas, vai ser muito mais tranquilo o fluxo do git, e você vai ver que o importante é entender os conceitos da ferramenta, não decorar comandos. 

Como foi ensinado a teoria, vai ser questão de poucas horas ou até minutos para entender a interface gráfica.

## Considerações Finais

Se você seguiu tudo nessa sessão, agora você sabe:

- Como clonar um repositório Git
- O que é fork e como dar fork de um repositório do GitHub
- O quão útil são as branches e o porquê usar elas bastante!
- Criar novas branches e mudar entre as branches 
- Integrar funcionalidades de sub-branches numa branch principal
- Melhores práticas na adição de funcionalidades 
- Melhores práticas de commits
- Como atualizar o repositório remoto ou local com o fetch e o pull
- Como dar Pull Requests

Parabéns! Na próxima sessão, iremos trabalhar com o repositório do quarkus sobre a rede social, iremos criar outro a partir dele.