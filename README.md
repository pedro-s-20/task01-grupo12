
## Documentação

Funcionamento, sintaxe e aplicação do comando git rebase;

Funcionamento: 

O rebase é utilizado para alterar o histórico de uma branch, em um caso onde: tendo sido criado uma branch para alteração e a branch antecessora ter tido commit após a branch nova já ter sido criada, poder alinhar todos os commits na antecessora alterando o histórico, sem que se perca os commits da nova, deixando mais linear. Isto porque o comando merge cria um novo commit na antecessora, fazendo com que não fiquem registrados os commits feitos na branch nova. (GUEDES, 2019)

Visualmente, o Merge funciona da seguinte forma:

Figura 1: ramificação de commits utilizando o merge.

<img src="/imagens-README/figura-01.png"/>

Fonte: BITBUCKET, 2023.

Já o rebase teria a seguinte ramificação:

Figura 2: ramificação de commits utilizando o rebase.

<img src="/imagens-README/figura-02.png"/>

Fonte: BITBUCKET, 2023.

Sintaxe: 

git rebase <nome da branch de destino (base)>

Executar o git rebase com a bandeira -i dá início a uma sessão de rebasing interativa. Em vez de mover todos os commits para a nova base sem conhecimento, o rebasing interativo oferece a oportunidade de alterar os commits individuais no processo. Assim você pode limpar o histórico removendo, dividindo e alterando uma série existente de commits. É como uma versão aprimorada do Git commit --amend. (BITBUCKET, 2023)

Comandos adicionais:

git rebase -- d significa que durante a reprodução o commit vai ser descartado do bloco de commit combinado final.
git rebase -- p deixa o commit como está. Ele não vai modificar a mensagem ou conteúdo do commit e ainda vai ser um commit individual no histórico de ramificações.
git rebase -- x durante a reprodução, executa um script do shell da linha de comandos em cada commit marcado. Um exemplo útil seria executar o conjunto de teste da base de código em commits específicos, o que poderia ajudar a identificar as regressões durante um rebase. (BITBUCKET, 2023)

Aplicação:

Supondo que há uma branch master e uma feature, onde ambas possuem commits feitos após a criação da feature, para alinhar o histórico de commits sem que haja perdas de histórico das duas, pode se aplicar o rebase:

Figura 3: branch main (master) e branch feature divergindo nos commits.	

<img src="/imagens-README/figura-03.png"/>

Fonte: GUEDES, 2019.

git checkout feature # indo para o branch da feature
git rebase master    # fazendo o rebase entre o feature e o master

Figura 4: aplicação do rebase.

<img src="/imagens-README/figura-04.png"/>

Fonte: GUEDES, 2019.

Dessa forma, o histórico ficou linear com o rebase aplicado. 

Contudo, um importante alerta ao usar o rebase é que, não se pode aplicá-lo em branchs públicas (sem avisar a equipe antes). Isso se deve ao fato de que pela alteração do histórico que causa divergências entre branchs locais e remotas. Em muitos casos pode gerar conflitos catastróficos. (GUEDES, 2019)

Funcionamento, sintaxe e aplicação do comando git cherry pick;

Funcionamento:

É utilizado para criar cópia de commit específico (selecionado “à dedo” = cherry pick) na branch selecionada (HEAD). Apesar de ser o mesmo conteúdo, é criado um novo commit pois a data é atual (e não a mesma data do commit original selecionado).

Sintaxe:

git cherry-pick [--edit] [-n] [-m <parent-number>] [-s] [-x] [--ff] [-S[<keyid>]] <commit>…​
git cherry-pick (--continue | --skip | --abort | --quit)

Aplicação:

Quando se deseja realizar um procedimento semelhante ao rebase, mas apenas com commites específicos.

Ex: Antes de aplicar o procedimento:

Figura 5: antes de aplicar o cherry-pick.

<img src="/imagens-README/figura-05.png"/>

Fonte: CHACON; STRAUB, 2014.

Após usar o comando git cherry-pick e43a6, para introduzir o commit e43a6 na branch master.

Figura 6: após aplicar o cherry-pick.

<img src="/imagens-README/figura-06.png"/>

Fonte: CHACON; STRAUB, 2014.

Funcionamento, sintaxe e aplicação do comando git revert;

Funcionamento:

O comando git revert funciona criando um novo commit que "desfaz" as alterações feitas no commit especificado. Ele não remove o commit original do histórico do repositório, mas cria uma nova entrada no histórico que anula as alterações feitas no commit original.

Quando você executa o comando git revert, o git cria um novo commit com as alterações revertidas e o adiciona à branch atual. Esse novo commit terá um novo hash e será mostrado no histórico do repositório como um commit separado do commit original.


Sintaxe:

-> git  revert  <commit>
Onde <commit> está o hash do commit ou referência do commit que você deseja reverter.

Por exemplo para reverter o último commit, você pode usar o comando:
-> git revert  HEAD
você também pode reverter um commit específico usando seu hash:
-> git revert abc123

Aplicação:

O comando git revert é usado para desfazer as alterações de um commit específico em um repositório git. Ele cria um novo commit com as alterações revertidas em vez de remover o commit original. Isso é útil quando você quer manter o histórico do repositório, mas desfazer as alterações de um commit específico. O uso básico do comando é:

-> git revert <commit hash>

Ao usar esse comando, você deve fazer um merge/pull request e obter aprovação de seus colegas de equipe antes de mesclar as alterações.



Funcionamento, sintaxe e aplicação do comando git squash;
Funcionamento:
O comando "git squash" é usado para combinar vários commits em um único commit. Isso é útil quando você precisa limpar o histórico de commits antes de fazer um push para um repositório remoto. Ele é geralmente usado com o comando "git rebase" para selecionar os commits que você deseja combinar.

Exemplo:

-> git rebase -i HEAD~3 

Aqui, o comando acima irá abrir o editor de texto padrão do sistema e mostrar os três últimos commits. Troque o primeiro 'pick' para 'squash' e salve o arquivo. Depois de fazer isso, o Git vai pedir para você escrever uma nova mensagem de commit para o commit combinado.

É importante lembrar que essa operação modifica o histórico do repositório, então é recomendado só fazer isso em branches locais e não compartilhados.


Sintaxe:

A sintaxe básica do comando "git squash" é:


-> git rebase -i HEAD~n

Onde "n" é o número de commits que você deseja combinar.

Exemplo:

->git rebase -i HEAD~3

Isto irá abrir o editor de texto padrão do sistema e mostrar os três últimos commits. Troque o primeiro 'pick' para 'squash' e salve o arquivo. Depois de fazer isso, o Git vai pedir para você escrever uma nova mensagem de commit para o commit combinado.
Além disso, o comando "git squash" pode ser usado em conjunto com o comando "git rebase" para especificar o intervalo de commits a serem combinados.

-> git rebase -i <hash-do-commit-inicial> <hash-do-commit-final>

Isso permite selecionar commits específicos para serem combinados, independentemente de quantos commits foram feitos desde então.

Aplicação:

A aplicação do comando "git squash" em um projeto é geralmente feita quando você deseja limpar o histórico de commits antes de fazer o merge para o branch principal. Aqui está um exemplo de como aplicar o comando "git squash" em um projeto:

Navegue até a branch onde você deseja aplicar o "git squash" usando o comando git -> checkout [nome-da-branch].

Use o comando git log para verificar o número de commits que deseja combinar.
Use o comando git rebase -i HEAD~[número-de-commits] para abrir o editor de texto com a lista dos últimos commits.

 Exemplo: 

-> git rebase -i HEAD~3
Altere a palavra "pick" para "squash" na frente de cada commit que deseja combinar. Salve e feche o arquivo.

O git abrirá novamente o editor de texto para que você possa editar a mensagem de commit resultante. Salve e feche o arquivo.

Use o comando git push -f [nome-da-branch] para enviar suas mudanças para o repositório remoto.
Faça o merge para o branch principal.

É importante notar que essa operação pode causar problemas se outras pessoas já baixaram sua branch e fizeram modificações. Certifique-se de avisar as pessoas envolvidas e discutir com elas antes de realizar essa operação.


Criar outro README-info.md: nomes dos integrantes e trilha do VemSer;

Criar o arquivo .gitignore (definições livres);

—---

Todos os integrantes tem que usar os comandos básicos visto em aula;

Cada integrante tem que ter uma branch feature;

Cada integrante tem que abrir PR com reviewers;

Após aprovação fazer o merge com a main;

Não excluir a branch;
