# Padrões de repositório

Este documento define as diretrizes padrão para atualização de código, gerenciamento de branches, commits, Pull Requests (PRs) e revisões de código nos repositórios da organização. O objetivo é manter o histórico limpo, facilitar a manutenção e garantir a qualidade das entregas.

Considere que não haverá somente uma pessoa fazendo manutenção no futuro e é de extrema importância conseguir localizar pontos de desenvolvimento que permitam retorno sem quebrar features ou colocar em risco a segurança e organização do repositório.

---
## 1 Nomenclaturas

### 1.1 Branches Principais

`main`
	Código estável em produção. **Nunca** faça push direto nesta branch.
`develop (dev)`
	Branch de integração para homologação/testes antes de ir para produção.

### 1.2 Branches de trabalho

Toda alteração deve ser feita em uma branch separada, criada a partir da branch principal atualizada (`main` ou `develop`).

**Formato padrão:**
`<tipo>/<id-da-tarefa>-<descricao-curta-em-kebab-case>`
*(Caso não haja ID de tarefa/issue, omita o número).*

| Prefixo     | Uso                                    | Exemplo                             |
| :---------- | :------------------------------------- | :---------------------------------- |
| `feat/`     | Nova funcionalidade                    | `feat/142-login-google`             |
| `fix/`      | Correção de bug                        | `fix/205-calculo-desconto`          |
| `hotfix/`   | Correção crítica direto em produção    | `hotfix/erro-checkout-pix`          |
| `refactor/` | Refatoração sem alterar comportamento  | `refactor/otimizar-queries-usuario` |
| `docs/`     | Alterações apenas em documentação      | `docs/atualizar-readme-api`         |
| `chore/`    | Configurações, dependências, build, CI | `chore/atualizar-node-20`           |
| `test/`     | Adição ou correção de testes           | `test/cobrir-servico-auth`          |

---
## 2 Padrão de Commits

Utilizamos a especificação **Conventional Commits** para manter o histórico legível e possibilitar a geração automática de changelogs.

### 2.1 Estrutura

```text
<tipo>(<escopo>): <descrição curta no imperativo>

[corpo explicando o "porquê" da mudança]

[rodapé para breaking changes ou fechamento de issues]

[arquivos atualizados e linhas adicionadas/removidas]
```

### 2.2 Tipos Permitidos

* **`feat`**: Nova funcionalidade para o usuário.
* **`fix`**: Correção de um bug.
* **`docs`**: Alterações exclusivas na documentação.
* **`style`**: Formatação, espaços, ponto e vírgula (sem mudança de lógica).
* **`refactor`**: Reestruturação de código que não corrige bug nem adiciona feature.
* **`perf`**: Melhoria de desempenho.
* **`test`**: Adição ou ajuste de testes automatizados.
* **`build`** / **`ci`**: Alterações no sistema de build, dependências ou pipelines de CI/CD.
* **`chore`**: Tarefas de manutenção que não modificam código de produção nem testes.

### 2.3 Boas Práticas de Commit
 * Escreva a descrição no **imperativo** e em letras minúsculas, sem ponto final:
	 `feat(auth): adiciona validação de token jwt`
* Faça **commits atômicos**: cada commit deve representar uma mudança **lógica única** e completa.
* Para mudanças que quebram compatibilidade (**Breaking Changes**), adicione `!` após o tipo/escopo:
	`feat(api)!: altera formato de resposta do endpoint de pedidos`

---

## 3 Padrão de Pull Requests (PRs)

Todo código só entra na branch principal via Pull Request.
### 3.1 Título do PR
Deve seguir o mesmo padrão do Conventional Commits:
* **Bom:** `feat(pagamentos): implementa integração com boleto bancário`
* **Evitar:** `ajustes finais`, `correção`, `PR do Fulano`

### 3.2 Boas Práticas

 **Mantenha o PR pequeno e focado:** Prefira dividir entregas grandes em múltiplos PRs menores. PRs com mais de ~400 linhas alteradas são mais difíceis de revisar e têm maior chance de ocultar bugs.

**Use Draft PRs:** Se a tarefa ainda está em andamento mas você deseja feedback antecipado ou validar a pipeline de CI, abra o PR como **Draft (Rascunho)**.

**Vincule a tarefa/issue:** Use palavras-chave como `Closes #123` ou `Fixes #123` na descrição para fechar issues automaticamente após o merge.

**Inclua evidências visuais:** Para alterações de interface (front-end), anexe prints ou GIFs demonstrando o "antes e depois".

### 3.3 Estrutura Esperada na Descrição

Certifique-se de preencher todos os campos:
1. **Descrição:** O que foi feito e qual problema resolve.
2. **Tipo de mudança:** Bugfix, feature, refactor, etc.
3. **Como testar:** Passo a passo claro para o revisor validar localmente.
4. **Checklist:** Confirmação de testes, lint e autorrevisão.

---

## 4 Versionamento e Releases

Começar a utilizar **Versionamento Semântico (SemVem)** no formato `vMAJOR.MINOR.PATCH` (`v2.4.1`). Tratar o sistema atual como a versão `v1.0` e evoluir a partir dela.

* **`MAJOR`** (`2.0.0`): Mudanças incompatíveis com versões anteriores (*Breaking Changes*).
* **`MINOR`** (`2.4.0`): Novas funcionalidades compatíveis com versões anteriores.
* **`PATCH`** (`2.4.1`): Correções de bugs compatíveis com versões anteriores.
### 4.1 Publicação de Nova Versão
1. Garanta que a `main` está testada e estável.
2. Crie uma tag anotada ou gere uma **Release** diretamente pela aba *Releases* do GitHub.
3. Documente na Release os principais itens alterados agrupados em: **Novidades**, **Correções** e **Melhorias/Breaking Changes**.

---

### 5 Fluxos de Emergência (Hotfix)

Quando um erro crítico ocorre em produção:
1. Crie uma branch `hotfix/<descricao>` diretamente a partir da `main`.
2. Aplique a correção mínima necessária para estancar o problema.
3. Abra um PR apontando para a `main` com a label `hotfix` / `urgente`.
4. Solicite revisão prioritária e realize o merge assim que validado.
5. Caso o projeto use `develop`, sincronize a correção da `main` de volta para a `develop` imediatamente após o deploy.

---

## 6 Checklist Rápido antes de abrir um PR

- [ ] Minha branch está atualizada com a `main` e sem conflitos.
- [ ] O código segue os padrões de formatação e lint do projeto.
- [ ] Não há arquivos temporários, `console.log` esquecidos ou credenciais expostas.
- [ ] Testei o fluxo principal e cenários de erro localmente.
- [ ] O título e a descrição do PR seguem o padrão deste documento.
