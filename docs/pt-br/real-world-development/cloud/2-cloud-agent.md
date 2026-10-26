---
title: "Exercício 2 - Agente do GitHub Copilot na nuvem"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações — se é que alguma — não enfrentem dificuldades com dívida técnica. Ela pode incluir problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de recursos que ficaram paradas no backlog por falta de tempo para implementá-las. O agente do GitHub Copilot na nuvem foi criado para executar tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina o trabalho, ele gera um PR em rascunho para revisão de uma pessoa desenvolvedora. Isso permite delegar tarefas repetitivas, acelerar o processo de desenvolvimento e liberar as pessoas desenvolvedoras para se concentrarem em questões mais amplas.

Você explorará os seguintes aspectos do agente do Copilot na nuvem:

- personalizar o ambiente para a geração de código;
- garantir que as operações sejam realizadas com segurança;
- entender a importância de issues com escopo bem definido;
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas que gostaria de resolver. As pessoas contratadas inicialmente para criar a primeira versão do site deixaram a documentação em um estado insatisfatório — na verdade, você verá que ela está completamente ausente. Como primeira etapa, a equipe quer adicionar comentários TSDoc a todas as funções exportadas no aplicativo.

Além disso, a equipe de design está pronta para melhorar a descoberta de jogos. Ela gostaria que a página de detalhes de cada jogo exibisse jogos relacionados — outros títulos da mesma categoria — para que as pessoas continuem navegando. Por enquanto, não é necessário implementar uma solução refinada; a equipe só quer algo que possa usar nos testes de aceitação da experiência do usuário. Isso está bloqueando o trabalho no momento, mas há outras issues com prioridade mais alta.

Esses dois exemplos mostram tarefas que podem acabar perdendo prioridade com facilidade e que são ótimas opções para atribuir ao agente do Copilot na nuvem. Ele pode trabalhar nelas de forma assíncrona, permitindo que você se concentre em outras tarefas e depois volte para revisar o trabalho do Copilot e confirmar se está tudo como esperado.

## Apresentação do agente do GitHub Copilot na nuvem

O [agente do GitHub Copilot na nuvem](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano, de maneira muito semelhante a uma pessoa desenvolvedora. Assim como no trabalho com uma pessoa desenvolvedora, há várias maneiras de iniciar esse trabalho, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois de receber a atribuição, o Copilot cria um pull request em rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar na tarefa. Você pode explorar a sessão do Copilot enquanto o trabalho está em andamento ou depois que for concluído. Quando a solução proposta estiver pronta para revisão, o Copilot marcará você no pull request!

## A importância de instruções com escopo bem definido

Não há mágica no GitHub Copilot — não dá para pular a etapa de raciocínio. Até operações aparentemente simples ficam complexas quando você examina todos os detalhes; por isso, [defina com cuidado o escopo das tarefas para o agente do Copilot na nuvem][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe por etapas, aprenda, experimente e adapte a abordagem. Os fundamentos do desenvolvimento de software não mudam com a chegada da IA generativa.

## Instruções personalizadas neste repositório

Nos exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se já fez um exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; se não, este é um bom momento para dar uma olhada rápida. Antes de atribuir trabalho ao agente do Copilot na nuvem, leia os arquivos de instruções já incluídos neste repositório, sem editá-los, para depois identificar o efeito deles.

Abra os seguintes arquivos na interface Web do GitHub para seu repositório ou em um Codespace, se já houver um em execução:

- `.github/copilot-instructions.md` — Revise a seção **Code standards**, principalmente as expectativas para as convenções de TypeScript e os comentários de documentação TSDoc.
- `.github/instructions/unit-tests.instructions.md` — Observe o frontmatter `applyTo`, que associa essas instruções aos arquivos `**/*.test.ts`.

Quando você atribuir a issue `Code lacks documentation` ao agente na nuvem na próxima seção, observe no pull request resultante os comentários de documentação TSDoc, as convenções de TypeScript e os cabeçalhos de comentário. Essas orientações vêm dos arquivos de instruções que você acabou de revisar. Voltaremos a esse assunto ao revisar PRs em um exercício posterior.

## Configure o ambiente de desenvolvimento do agente do Copilot na nuvem

Em geral, criar código — seja quem for a pessoa ou ferramenta responsável — exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja em ordem. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de maneira semelhante a uma pessoa engenheira de software.

O agente na nuvem usa o [GitHub Actions][github-actions] para configurar o ambiente de trabalho. Você pode personalizá-lo criando um [workflow especial de configuração][setup-workflow], definido no arquivo `.github/workflows/copilot-setup-steps.yml`, que será executado antes de o agente começar. Isso disponibiliza as ferramentas e dependências de desenvolvimento necessárias. Esse workflow já foi configurado para facilitar o andamento do laboratório e proporcionar essa experiência de aprendizado. Ele garante que o Copilot tenha acesso ao Node.js, às dependências do projeto, aos binários de navegador para testes de ponta a ponta e ao banco de dados SQLite migrado e preenchido com dados para o aplicativo Astro único:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para suas etapas. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Para clonar o repositório como parte das etapas de configuração, por exemplo, para instalar dependências, será necessária a permissão `contents: read`. Se você não clonar o repositório nas etapas de configuração, o Copilot fará isso automaticamente após a conclusão das etapas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do front-end/aplicativo: Node.js (agora o aplicativo inteiro usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e preenche o banco de dados SQLite local para que os builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer outro workflow do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para testar se o workflow é executado corretamente, sem esperar que o Copilot o inicie.

## Adicione a documentação

Embora todas as pessoas entendam a importância da documentação, a maioria dos projetos tem informações desatualizadas ou não tem documentação alguma. Esse tipo de dívida técnica costuma ficar sem solução, reduzindo a produtividade e dificultando a manutenção da base de código ou a integração de novas pessoas à equipe. Felizmente, o Copilot é muito útil para criar documentação, e esta é uma issue ideal para atribuir ao agente do Copilot na nuvem. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o trabalho.

1. Acesse o repositório em github.com em uma nova guia do navegador.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de criação de issue.
4. Selecione **Blank issue** para criar a issue.
5. Defina o campo **Title** como `Code lacks documentation`.
6. Defina o campo **Description** como:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuindo uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

  ![Detalhes da atribuição ao Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-gerado. O título será parecido com `[WIP]: Code lacks documentation`. Se um novo PR não aparecer na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar vários minutos para aparecer no PR. O Copilot está configurando o ambiente (executando o workflow destacado anteriormente), analisando o projeto e definindo a melhor abordagem para resolver o problema.

13. Revise a lista e as tarefas que o Copilot pretende concluir.
14. Role a linha do tempo do pull request; você deverá ver uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

  ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão ao vivo e acompanhar como o Copilot resolve o problema. Isso inclui explorar o código e entender o estado atual, ver como o Copilot faz uma pausa para pensar e decidir o plano adequado e acompanhar a criação do código.

Esse processo provavelmente levará vários minutos. Um dos principais objetivos do agente do Copilot na nuvem é executar tarefas de forma assíncrona, liberando você para se concentrar em outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente do Copilot na nuvem e, em seguida, voltando nossa atenção para a criação de código que adicionará funcionalidades ao aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes do jogo

Como já destacamos, uma das grandes vantagens do agente do GitHub Copilot na nuvem é poder dividir o trabalho: você se concentra em um conjunto de tarefas enquanto ele trabalha em outro. Talvez adicionar uma seção de jogos relacionados para a equipe de design não leve muito tempo, mas esse tempo ainda poderia ser usado para outras tarefas. Vamos atribuir isso ao agente do Copilot na nuvem!

1. Volte ao repositório em github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de criação de issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. Defina o campo **Title** como `Show related games on the game details page`.
6. Defina o campo **Description** como:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuindo uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você verá uma série de 👀 no primeiro comentário da issue, indicando que o Copilot está trabalhando nela!

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

Agora, o Copilot está trabalhando diligentemente na sua segunda solicitação! O agente do Copilot na nuvem trabalha de maneira semelhante a uma pessoa engenheira de software. Você não precisa acompanhá-lo o tempo todo; basta revisar o trabalho quando estiver concluído. Vamos voltar nossa atenção para a criação e o uso de agentes personalizados.

## Resumo e próximos passos

Nesta lição, você explorou o [agente do GitHub Copilot na nuvem][copilot-agents]. Com ele, você pode atribuir issues ao Copilot para que sejam resolvidas de forma assíncrona. Você pode usar o Copilot para resolver dívidas técnicas, criar funcionalidades ou ajudar a migrar código de um framework para outro.

Você explorou estes conceitos:

- personalizar o ambiente para a geração de código;
- garantir que as operações sejam realizadas com segurança;
- entender a importância de issues com escopo bem definido;
- atribuir issues ao Copilot.

Enquanto o agente na nuvem trabalha em segundo plano, você pode voltar sua atenção à criação e ao uso de agentes personalizados. [O agente do Copilot na nuvem também pode usar servidores MCP][cloud-agent-mcp] e tem acesso às instruções personalizadas que exploramos em módulos anteriores.

## Recursos

- [Sobre o agente do Copilot na nuvem][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente do Copilot na nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
