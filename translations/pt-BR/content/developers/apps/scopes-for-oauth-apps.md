---
title: Escopos para aplicativos OAuth
intro: '{% data reusables.shortdesc.understanding_scopes_for_oauth_apps %}'
redirect_from:
  - /apps/building-integrations/setting-up-and-registering-oauth-apps/about-scopes-for-oauth-apps/
  - /apps/building-oauth-apps/scopes-for-oauth-apps/
  - /apps/building-oauth-apps/understanding-scopes-for-oauth-apps
versions:
  free-pro-team: '*'
  enterprise-server: '*'
---

Ao configurar um aplicativo OAuth no GitHub, os escopos solicitados são exibidos para o usuário no formulário de autorização.

{% note %}

**Observação:** Se você está criando um aplicativo no GitHub, você não precisa fornecer escopos na sua solicitação de autorização. Para obter mais informações sobre isso, consulte "[Identificar e autorizar usuários para aplicativos GitHub](/apps/building-github-apps/identifying-and-authorizing-users-for-github-apps/)".

{% endnote %}

{% if currentVersion == "free-pro-team@latest" or currentVersion ver_gt "enterprise-server@2.21" %}
Se seu {% data variables.product.prodname_oauth_app %} não tiver acesso a um navegador, como uma ferramenta de CLI, você não precisará especificar um escopo para que os usuários efetuem a autenticação no seu aplicativo. Para obter mais informações, consulte "[Autorizar aplicativos OAuth](/developers/apps/authorizing-oauth-apps#device-flow)".
{% endif %}

Verifique os cabeçalhos para ver quais escopos do OAuth você tem e o que a ação da API aceita:

```shell
$ curl -H "Authorization: token OAUTH-TOKEN" {% data variables.product.api_url_pre %}/users/codertocat -I
HTTP/1.1 200 OK
X-OAuth-Scopes: repo, user
X-Accepted-OAuth-Scopes: user
```

* `X-OAuth-Scopes` lista o escopo que seu token autorizou.
* `X-Accepted-OAuth-Scopes` lista os escopos verificados pela ação.

### Escopos disponíveis

| Nome                     | Descrição                                                                                                                                                                                                                                                                                                                                                                                          |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`(sem escopo)`**       | Concede acesso somente leitura a informações públicas (inclui informações do perfil do usuário público, informações do repositório público e gists){% if currentVersion != "free-pro-team@latest" %}
| **`site_admin`**         | Concede acesso de administrador aos pontos de extremidades da API de administração [{% data variables.product.prodname_ghe_server %}](/v3/enterprise-admin).{% endif %}
| **`repo`**               | Concede acesso total a repositórios privados e públicos. Isso inclui acesso de leitura/gravação ao código, status do commit, repositório e projetos da organização, convites, colaboradores, adição de associações de equipe, status de implantação e webhooks de repositórios para repositórios e organizações públicos e privados. Também concede capacidade para gerenciar projetos de usuário. |
| &emsp;`repo:status`      | Concede acesso de leitura/gravação aos status do commit do repositório público e privado. Esse escopo só é necessário para conceder a outros usuários ou serviços acesso a status de compromisso de repositórios privados *sem* conceder acesso ao código.                                                                                                                                         |
| &emsp;`repo_deployment`  | Concede acesso aos [status de implantação](/v3/repos/deployments) para repositórios públicos e privados. Esse escopo só é necessário para conceder a outros usuários ou serviços acesso ao status de implantação, *sem* conceder acesso ao código.                                                                                                                                                 |
| &emsp;`public_repo`      | Limita o acesso a repositórios públicos. Isso inclui acesso de leitura/gravação em código, status de commit, projetos de repositório, colaboradores e status de implantação de repositórios e organizações públicos. Também é necessário para repositórios públicos marcados com uma estrela.                                                                                                      |
| &emsp;`repo:invite`      | Concede habilidades de aceitar/recusar convites para colaborar em um repositório. Este escopo só é necessário para conceder a outros usuários ou serviços acesso a convites *sem* conceder acesso ao código.{% if currentVersion == "free-pro-team@latest" or currentVersion ver_gt "enterprise-server@2.21"%}
| &emsp;`security_events`  | Concede acesso de leitura e escrita a eventos de segurança na [API {% data variables.product.prodname_code_scanning %}](/v3/code-scanning).{% endif %}
| **`admin:repo_hook`**    | Concede acesso de leitura, gravação e ping aos hooks do repositório em repositórios públicos e privados. O escopos do `repo` e `public_repo` concede acesso total aos repositórios, incluindo hooks de repositório. Use o escopo `admin:repo_hook` para limitar o acesso apenas a hooks de repositório.                                                                                            |
| &emsp;`write:repo_hook`  | Concede acesso de leitura, escrita e ping para os hooks em repositórios públicos ou privados.                                                                                                                                                                                                                                                                                                      |
| &emsp;`read:repo_hook`   | Concede acesso de leitura e ping para hooks em repositórios públicos ou privados.                                                                                                                                                                                                                                                                                                                  |
| **`admin:org`**          | Gerencia totalmente a organização e suas equipes, projetos e associações.                                                                                                                                                                                                                                                                                                                          |
| &emsp;`write:org`        | Acesso de leitura e gravação à associação da organização, aos projetos da organização e à associação da equipe.                                                                                                                                                                                                                                                                                    |
| &emsp;`read:org`         | Acesso somente leitura à associação da organização, aos projetos da organização e à associação da equipe.                                                                                                                                                                                                                                                                                          |
| **`admin:public_key`**   | Gerenciar totalmente as chaves públicas.                                                                                                                                                                                                                                                                                                                                                           |
| &emsp;`write:public_key` | Criar, listar e visualizar informações das chaves públicas.                                                                                                                                                                                                                                                                                                                                        |
| &emsp;`read:public_key`  | Listar e visualizar informações para as chaves públicas.                                                                                                                                                                                                                                                                                                                                           |
| **`admin:org_hook`**     | Concede acesso de leitura, gravação, ping e e exclusão de hooks da organização. **Observação:** Os tokens do OAuth só serão capazes de realizar essas ações nos hooks da organização que foram criados pelo aplicativo OAuth. Os tokens de acesso pessoal só poderão realizar essas ações nos hooks da organização criados por um usuário.                                                         |
| **`gist`**               | Concede acesso de gravação aos gists.                                                                                                                                                                                                                                                                                                                                                              |
| **`notificações`**       | Condece: <br/>* acesso de gravação a notificações de um usuário <br/>* acesso para marcar como leitura nos threads <br/>* acesso para inspecionar e não inspecionar um repositório e <br/>* acesso de leitura, gravação e exclusão às assinaturas dos threads.                                                                                                             |
| **`usuário`**            | Concede acesso de leitura/gravação apenas às informações do perfil.  Observe que este escopo inclui `user:email` e `user:follow`.                                                                                                                                                                                                                                                                  |
| &emsp;`read:user`        | Concede acesso para ler as informações do perfil de um usuário.                                                                                                                                                                                                                                                                                                                                    |
| &emsp;`usuário:email`    | Concede acesso de leitura aos endereços de e-mail de um usuário.                                                                                                                                                                                                                                                                                                                                   |
| &emsp;`user:follow`      | Concede acesso para seguir ou deixar de seguir outros usuários.                                                                                                                                                                                                                                                                                                                                    |
| **`delete_repo`**        | Concede acesso para excluir repositórios administráveis.                                                                                                                                                                                                                                                                                                                                           |
| **`write:discussion`**   | Permite acesso de leitura e gravação para discussões da equipe.                                                                                                                                                                                                                                                                                                                                    |
| &emsp;`leia:discussion`  | Permite acesso de leitura para discussões da equipe.{% if currentVersion == "free-pro-team@latest" %}
| **`write:packages`**     | Concede acesso ao para fazer o upload ou publicação de um pacote no {% data variables.product.prodname_registry %}. Para obter mais informações, consulte "[Publicar um pacote](/github/managing-packages-with-github-packages/publishing-a-package)".                                                                                                                                        |
| **`read:packages`**      | Concede acesso ao download ou instalação de pacotes do {% data variables.product.prodname_registry %}. Para obter mais informações, consulte "[Instalando um pacote](/github/managing-packages-with-github-packages/installing-a-package)".                                                                                                                                                   |
| **`delete:packages`**    | Concede acesso para excluir pacotes de {% data variables.product.prodname_registry %}. Para obter mais informações, consulte "[Excluir pacotes](/github/managing-packages-with-github-packages/deleting-a-package)".{% endif %}
| **`admin:gpg_key`**      | Gerenciar totalmente as chaves GPG.                                                                                                                                                                                                                                                                                                                                                                |
| &emsp;`write:gpg_key`    | Criar, listar e visualizar informações das chaves GPG.                                                                                                                                                                                                                                                                                                                                             |
| &emsp;`read:gpg_key`     | Listar e visualizar informações das chaves GPG.{% if currentVersion == "free-pro-team@latest" %}
| **`fluxo de trabalho`**  | Concede a capacidade de adicionar e atualizar arquivos do fluxo de trabalho do {% data variables.product.prodname_actions %}. Os arquivos do fluxo de trabalho podem ser confirmados sem este escopo se o mesmo arquivo (com o mesmo caminho e conteúdo) existir em outro branch no mesmo repositório.{% endif %}

{% note %}

**Observação:** O seu aplicativo OAuth pode solicitar os escopos no redirecionamento inicial. Você pode especificar vários escopos separando-os com um espaço:

    https://github.com/login/oauth/authorize?
      client_id=...&
      scope=user%20public_repo

{% endnote %}

### Escopos solicitados e escopos concedidos

O atributo `escopo` lista os escopos adicionados ao token que foram concedido pelo usuário. Normalmente, estes escopos são idênticos aos que você solicitou. No entanto, os usuários podem editar seus escopos, concedendo, efetivamente, ao seu aplicativo um acesso menor do que você solicitou originalmente. Além disso, os usuários podem editar o escopo do token depois que o fluxo do OAuth for concluído. Você deve ter em mente esta possibilidade e ajustar o comportamento do seu aplicativo de acordo com isso.

É importante lidar com casos de erro em que um usuário escolhe conceder menos acesso do que solicitado originalmente. Por exemplo, os aplicativos podem alertar ou informar aos seus usuários que a funcionalidade será reduzida ou não serão capazes de realizar algumas ações.

Além disso, os aplicativos sempre podem enviar os usuários de volta através do fluxo para obter permissão adicional, mas não se esqueça de que os usuários sempre podem dizer não.

Confira o [Príncípios do guia de autenticação](/guides/basics-of-authentication/), que fornece dicas para lidar com escopos de token modificável.

### Escopos normalizados

Ao solicitar vários escopos, o token é salvo com uma lista normalizada de escopos, descartando aqueles que estão implicitamente incluídos pelo escopo solicitado. Por exemplo, a solicitação do usuário `user,gist,user:email` irá gerar apenas um token com escopos de `usuário` e `gist`, desde que o acesso concedido com o escopo `user:email` esteja incluído no escopo `usuário`.
