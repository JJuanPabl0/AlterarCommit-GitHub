# 📌 Comandos Git: Verificar Atualização da Branch e Alterar Commits

Este guia explica como verificar se sua branch local está atualizada com a branch remota e como modificar commits no Git, incluindo o último commit e commits anteriores.

---

## 📍 Verificar se a Branch Local está Atualizada com a Remota  

Para verificar se sua branch local está sincronizada com a branch remota, utilize os seguintes comandos:

```bash
git fetch origin
git status
```

### 🔹 Explicação:
- `git fetch origin` → Atualiza as referências da branch remota sem modificar a branch local.
- `git status` → Exibe mensagens informando se sua branch está:
  - Atualizada ✅  
  - Atrasada (precisa de um `git pull`) ⬇️  
  - À frente (precisa de um `git push`) ⬆️  

---

### 🔹 Alternativa: Comparação Direta com a Branch Remota
Se quiser comparar a branch local com a remota:


```bash
git log --oneline origin/SEU_BRANCH..SEU_BRANCH
```

### 🔹 Verificar se a branch remota tem commits não baixados
Para verificar se sua branch remota tem commits que você ainda não baixou, use:

```bash
git log --oneline SEU_BRANCH..origin/SEU_BRANCH
```

### 🔄 Atualizar sua Branch  
Se sua branch estiver desatualizada, execute:  

```bash
git pull origin SEU_BRANCH
```

## 📍Como Modificar os Commits que já foram Lançados


### 🔹 Alterar apenas a mensagem do último commit  
Se você cometeu um erro na mensagem do último commit, use:  

```bash
git commit --amend -m "Nova mensagem do commit"
```
Isso substituirá a mensagem do commit anterior sem criar um novo commit.

### 🔹 Adicionar arquivos ao último commit
Caso tenha esquecido de adicionar arquivos ao commit anterior:
```bash
git add arquivo_esquecido.txt
git commit --amend --no-edit
```
Isso mantém a mensagem original e adiciona os novos arquivos.

### 🔹 Alterar um commit antigo (não enviado para o repositório remoto)

Se deseja modificar um commit antigo, use rebase interativo:

```bash
git rebase -i HEAD~3
```

Isso abrirá um editor com os últimos 3 commits. No editor, altere pick para edit no commit que deseja modificar, salve e feche.

- Depois, faça as alterações necessárias e finalize com:

```bash
git commit --amend
git rebase --continue
```

### 🔹 Alterar um commit já enviado para o repositório remoto ⚠️
**1.** Visualizar o Histórico de Commits: Primeiro, use o comando git log para exibir o histórico de commits. Isso permitirá que você veja o hash do commit que deseja alterar.
```bash
git log
```

Isso exibirá algo como:

```sql
commit 1234567890abcdef (HEAD -> main)
Author: Seu Nome <seuemail@example.com>
Date:   Thu Feb 25 18:23:45 2025 -0300

    Mensagem do commit que quero alterar

commit abcdef1234567890
Author: Seu Nome <seuemail@example.com>
Date:   Thu Feb 24 16:12:34 2025 -0300

    Outro commit
````

Anote o hash do commit que você deseja modificar. No exemplo acima, seria 1234567890abcdef.


**2.** Rebase Interativo com o Commit Específico: Use o comando git rebase -i e forneça o hash do commit anterior ao que você deseja modificar. Por exemplo, se você deseja modificar o commit 1234567890abcdef, use o hash abcdef1234567890 (o commit anterior):

```bash
git rebase -i abcdef1234567890
```
Isso abrirá o editor de rebase interativo com os commits a partir do commit selecionado.

**3.** Escolher o Commit para Editar: No editor de rebase, você verá uma lista de commits a partir do commit que você selecionou. Altere pick para edit no commit que você deseja alterar:

```pgsql
pick abcdef1234567890 Outro commit
edit 1234567890abcdef Mensagem do commit que quero alterar
````

**4.** Alterar o Commit:

- Após salvar e fechar o editor, o Git irá parar no commit que você selecionou para editar. Agora, você pode fazer as alterações necessárias (editar arquivos ou modificar a mensagem do commit).

- Para alterar a mensagem do commit, use:
```bash
git commit --amend
```
Isso abrirá um editor onde você pode modificar a mensagem do commit. Se precisar modificar o conteúdo (arquivos), basta editar os arquivos antes de usar o git commit --amend.

**5.** Continuar o Rebase: Após fazer as alterações necessárias, continue o rebase:
```bash
git rebase --continue
````

**6.** Forçar o Push para o Repositório Remoto: Depois de concluir o rebase, será necessário forçar o push para o repositório remoto para atualizar o histórico com o commit alterado:
```
git push --force
```

Assim como no processo de rebase interativo, o uso de git push --force sobrescreverá o histórico remoto. Certifique-se de que ninguém mais esteja trabalhando nos commits alterados, caso contrário, pode haver conflitos.

Essa abordagem é uma forma de editar um commit específico sem precisar rebasear toda a sequência de commits de uma vez, dando mais controle sobre os commits que você deseja alterar.

## 📌 Conclusão  
Agora você sabe como:  
✔️ Modificar a mensagem e os arquivos do último commit.  
✔️ Alterar commits antigos usando `git rebase`.  
✔️ Enviar alterações para o repositório remoto (com cautela).  

Use esses comandos com atenção para manter um histórico limpo e organizado! 🚀