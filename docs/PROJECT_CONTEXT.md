# SENA IMPORTS — Contexto do projeto

## Objetivo e situação das etapas

Este documento reúne o contexto do projeto, as evidências de arquitetura e o planejamento disponível. Distingue requisitos de funcionalidades implementadas e não comprova a conclusão de uma etapa apenas pela existência de documentação.

| Etapa | Contexto disponível | Situação |
| --- | --- | --- |
| 01 | Criação de `public.profiles`, conforme SQL informado como aplicado | Tabela e políticas confirmadas pelo contexto; conclusão integral da etapa não comprovada |
| 02 | O histórico solicitou avaliar pré-requisitos e pediu definição do escopo | Escopo e conclusão não documentados no histórico disponível |
| 03 | Painel administrativo da SENA IMPORTS | Escopo planejado descrito neste documento; implementação não confirmada |
| 04 | Citada no pedido de documentação, sem especificação funcional no histórico disponível | Escopo não definido; não atribuir funcionalidades ou entregas por suposição |

O banco conectado e o cadastro de contas existente devem ser preservados. A existência desses recursos foi informada no contexto; seus arquivos de implementação não constam da listagem atual.

## Evidências da revisão

- Repositório: `llvitorino189-project/projeto-senna--lovable`.
- Branch: `main`.
- HEAD de referência anterior a esta atualização: `39dfb28cc6842b7f4057c70dc21f97b3af544ea9`.
- A listagem atual contém `README.md` e `docs/PROJECT_CONTEXT.md`, ambos lidos integralmente. O README contém apenas o título do projeto.
- Este documento já existia e foi atualizado nesta entrega; não representa código de aplicação.
- Não constam arquivos da aplicação, dependências, rotas, design system, autenticação ou cliente Supabase nessa listagem. Não há evidência de código para identificar framework, estrutura de componentes, roteamento ou mecanismo de autorização administrativa.
- O banco conectado, o cliente Supabase já configurado e o cadastro de contas existente foram informados no contexto. A ausência dos arquivos da aplicação na listagem não significa ausência desses recursos no projeto.
- A existência de `public.profiles` e a aplicação do SQL foram confirmadas pelo contexto do pedido, não por execução de migrations ou consultas nesta revisão.
- Não houve execução de consultas, migrations, build ou testes nesta revisão.

## Arquitetura: confirmação e limites

### Identificado nos arquivos

A estrutura versionada apresentada nesta revisão é documental: um README e este arquivo de contexto. Não há código de aplicação nesses arquivos. Portanto, nenhuma stack de frontend, biblioteca de componentes, rota, serviço de backend ou fluxo de telas pode ser descrito como identificado no código.

### Informado no contexto do projeto

- Autenticação e cadastro de contas existentes por Supabase Auth, com cliente Supabase já configurado.
- Banco conectado com a tabela de perfil `public.profiles`.
- Vínculo do perfil com a conta por `profiles.id = auth.users.id`.
- Isolamento dos perfis por RLS, conforme políticas descritas abaixo.

Esses elementos orientam a integração futura, mas não comprovam como o código atual organiza sessão, providers, formulários ou persistência do perfil. A criação automática do perfil após o cadastro não foi verificada.

### Preservação do cadastro e integração

- Reutilizar o cliente Supabase e o cadastro de contas existentes; não criar autenticação paralela nem substituir o fluxo sem necessidade aprovada.
- Usar os métodos de autenticação existentes da plataforma: `signUp`, `signInWithPassword`, `signOut`, `getSession` e `onAuthStateChange`.
- Não criar tabela de senhas, coluna de senha em `profiles` ou verificação manual de credenciais. As senhas são gerenciadas pelo Supabase Auth, não pela tabela de perfil.
- Consultar os dados adicionais do perfil após autenticação, usando o identificador da sessão como `profiles.id`.
- Quando houver implementação do vínculo de cadastro com perfil, gravar apenas as colunas existentes e respeitar a sessão e a RLS. Se o cadastro exigir confirmação de e-mail e ainda não houver sessão, a gravação deve aguardar autenticação, sem desabilitar políticas nem usar credenciais privilegiadas no navegador.
- Preservar registros existentes e não sobrescrever um nome já salvo ao apenas garantir a existência do perfil.
- Não incluir senhas, tokens, chaves de API ou outras credenciais nesta documentação.

## Etapa 03 — Painel administrativo

Status: requisitos documentados; nenhuma implementação do painel foi confirmada nos arquivos apresentados.

Objetivo: criar uma central administrativa profissional, inspirada na organização da Nuvemshop, integrada à aplicação e aos controles de acesso existentes. O mapa abaixo é planejamento, não uma lista de funcionalidades já entregues.

## Banco confirmado

Tabela `public.profiles`:

| Coluna | Tipo | Restrição |
| --- | --- | --- |
| `id` | uuid | Chave primária; referência a `auth.users(id)` com exclusão em cascata |
| `nome` | text | Opcional |
| `criado_em` | timestamptz | Obrigatória; padrão `now()` |

Segundo o SQL informado como aplicado, RLS está habilitada. As políticas permitem SELECT, INSERT e UPDATE apenas quando `auth.uid() = id`; INSERT e UPDATE também validam essa condição nos dados gravados. Não foi informada política de DELETE.

Consequências para o painel:

- Um usuário autenticado pode acessar o próprio perfil, respeitando as políticas existentes.
- Ter perfil não equivale a ser administrador.
- Não há campo, vínculo ou mecanismo administrativo confirmado para identificar equipe, papéis ou permissões.
- Não se deve listar todos os perfis, contar perfis como clientes ou usar a data de criação do perfil como histórico comercial.
- Não criar tabelas, políticas ou registros para preencher o painel. Preservar `public.profiles` e seus registros.
- Qualquer integração futura deve usar o cliente Supabase existente e a sessão autenticada. Não criar autenticação paralela nem expor credenciais privilegiadas.

## Direção visual solicitada

- Organização inspirada na Nuvemshop, sem reproduzir funcionalidades sem suporte.
- Sidebar recolhível com fundo `#080808`, textos claros e ícones dourados discretos.
- Item ativo em carvão, com detalhe dourado; referência dourada `#D4AF37`.
- Área de trabalho off-white, cards brancos, botões pretos com detalhes dourados e tipografia Inter.
- Layout limpo, responsivo, com submenus expansíveis e breadcrumbs.
- Utilizar Tailwind e os componentes do design system da aplicação, quando seus arquivos estiverem disponíveis na revisão.

### Referência anexada

`/uploads/download_1_.png` é uma captura de referência de um editor visual, com preto, dourado, painéis laterais e marca SENA IMPORTS dentro da composição. Não deve ser utilizada como imagem do painel inteiro nem tratada como arquivo isolado do logotipo oficial. O caminho de um ativo oficial de logotipo separado não foi identificado.

## Mapa solicitado e disponibilidade verificada

Os itens abaixo representam o escopo solicitado, não rotas existentes. Nenhum desses módulos teve implementação confirmada pelos arquivos disponíveis.

| Grupo | Itens solicitados | Situação |
| --- | --- | --- |
| Visão geral | Dashboard; atividades recentes | Sem fontes comerciais ou eventos confirmados |
| Vendas | Pedidos; vendas manuais; carrinhos abandonados; trocas e devoluções | Sem suporte confirmado |
| Produtos | Todos os produtos; novo produto; categorias; marcas; coleções; variações; decants; kits | Sem suporte confirmado |
| Estoque | Inventário; movimentações; alertas | Sem suporte confirmado |
| Clientes | Cadastro; segmentos; histórico | Sem suporte confirmado; profiles não comprova cadastro comercial |
| Marketing | Cupons; promoções; campanhas; gerador de links; automações | Sem suporte confirmado |
| Loja virtual | Editor visual; banners; vitrines; páginas; menus; identidade visual; biblioteca de mídia; SEO | Sem suporte confirmado |
| Financeiro | Receitas; transações; custos e margens | Sem suporte confirmado |
| Logística | Entregas; frete; retirada; rastreamento | Sem suporte confirmado |
| Relatórios | Vendas; produtos; estoque; clientes; campanhas | Sem suporte confirmado |
| Configurações | Dados da loja; equipe; permissões; atendimento; integrações; segurança | Sem suporte confirmado; próprio perfil é o único dado de conta documentado |

Na implementação, itens sem suporte devem ser identificados como indisponíveis, com motivo legível, sem links quebrados, formulários de fachada ou botões que simulem operações.

## Indicadores do dashboard

Faturamento, pedidos, ticket médio, clientes, produtos cadastrados, estoque crítico, campanhas e atividades recentes não têm fontes confirmadas na estrutura fornecida.

Regras de apresentação:

- Exibir indisponibilidade e sua justificativa enquanto não houver fonte real e autorização verificadas.
- Não apresentar zeros artificiais, gráficos demonstrativos, tendências inventadas ou atividades geradas para preencher espaço.
- Diferenciar ausência de suporte, falta de permissão, carregamento, falha de consulta e resultado realmente vazio.
- Exibir zero somente quando uma consulta real e autorizada retornar esse resultado.
- Não deduzir indicadores comerciais a partir de `profiles`.

## Recursos transversais

- Busca administrativa: habilitar apenas para fontes e módulos reais autorizados; não apresentar resultados fictícios.
- Notificações: nenhuma fonte confirmada; não inventar contadores ou eventos.
- Perfil: a estrutura permite acessar o próprio `nome` após autenticação, mas a integração depende dos arquivos do cliente e da sessão existentes.
- Ver minha loja: habilitar somente após identificar a rota ou URL real da loja. Não presumir que a raiz do projeto seja a vitrine.
- Breadcrumbs e submenus: refletir apenas a navegação implementada; itens indisponíveis não devem levar a telas simuladas.

## Segurança obrigatória

Antes de disponibilizar a rota administrativa, identificar o mecanismo real de autorização do projeto. A autenticação, por si só, não concede acesso administrativo.

A proteção deve abranger acesso direto às rotas, ações e consultas, inclusive recarregamentos e sessões expiradas. Ocultar o menu não é controle de acesso. Não contornar RLS, conceder acesso com base em valores controlados pelo usuário ou introduzir uma credencial privilegiada no navegador.

Enquanto não houver autorização administrativa identificada, não liberar o painel como se todos os usuários autenticados fossem administradores.

## Etapa 04 — Escopo ainda não definido

O histórico disponível não especifica objetivo, módulos, telas, operações de banco, permissões ou critérios de aceite para a etapa 04. Nenhuma funcionalidade deve ser atribuída a essa etapa por inferência, incluindo os módulos planejados para a etapa 03.

Antes de implementar a etapa 04, seu escopo precisa ser definido e registrado: entregas, limites, dependências das etapas anteriores, permissões necessárias e critérios de validação. Isso não impede documentar o contexto já conhecido, mas impede declarar a etapa planejada em detalhe, implementada ou pronta para execução.

As restrições de preservação de `public.profiles`, do cadastro de contas existente e de proteção das credenciais também se aplicam à etapa 04. Não há autorização implícita para alterar o banco ou substituir a autenticação.

## Validação pendente

Nenhum teste desktop ou mobile foi executado. Não há aplicação verificável na listagem desta revisão.

Critérios para a futura validação:

- Compilação, verificação de tipos e testes disponíveis no projeto.
- Desktop: recolhimento da sidebar, expansão dos grupos, indicação de item ativo e organização dos cards.
- Mobile: navegação sem transbordamento horizontal, fechamento do menu, foco controlado e alvos de toque adequados.
- Acessibilidade: navegação por teclado, rótulos, contraste e estado expandido anunciado.
- Segurança: usuário sem sessão, usuário autenticado sem autorização administrativa, administrador autorizado conforme o mecanismo real e sessão expirada.
- Dados: consultas autorizadas, falhas de rede, fontes indisponíveis e resultados vazios sem substituição por dados fictícios.
- Navegação: nenhum link quebrado e nenhum recurso indisponível apresentado como funcional.

## Entrega desta revisão

Entregue: atualização deste documento com contexto do projeto, limites da identificação da arquitetura, distinção entre recursos informados e funcionalidades planejadas, escopo disponível da etapa 03 e ausência de definição da etapa 04. Foram registradas a preservação de `public.profiles (id, nome, criado_em)` e do cadastro de contas existente, sem criar tabela de senhas nem incluir credenciais.

Esta entrega altera somente documentação. Não modifica o banco, o cadastro, permissões ou arquivos da aplicação e não implementa os módulos descritos.

Pendências do produto: identificar a implementação da aplicação e seu mecanismo administrativo; implementar e integrar o painel da etapa 03; identificar e usar o ativo oficial de marca; validar desktop e mobile; definir o escopo da etapa 04. Não declarar essas pendências concluídas a partir deste documento. A implementação não deve substituir a stack nem criar uma aplicação paralela para compensar os arquivos ausentes da listagem.
