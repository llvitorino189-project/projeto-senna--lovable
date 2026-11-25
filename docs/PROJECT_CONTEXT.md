# SENA IMPORTS — Contexto do projeto

## Etapa 03 — Painel administrativo

Status: contexto documentado; painel não implementado.

Este documento registra os requisitos solicitados e as evidências disponíveis. Não comprova a conclusão das etapas anteriores nem a existência de funcionalidades além das descritas abaixo.

## Evidências da revisão

- Repositório: `llvitorino189-project/projeto-senna--lovable`.
- Branch: `main`.
- HEAD informado na leitura: `a555de323a574fe3f7df1d2ea8b74f746850c119`.
- A listagem fornecida contém somente `README.md`, lido integralmente, com o título do projeto.
- `docs/PROJECT_CONTEXT.md` não constava dessa listagem; este documento foi criado nesta entrega, não recuperado de um planejamento anterior.
- Não constam arquivos da aplicação, dependências, rotas, design system, autenticação ou cliente Supabase nessa listagem. Portanto, não foi possível identificar suas implementações nem integrar uma rota administrativa preservando os controles existentes.
- O banco conectado e a existência de `public.profiles` foram confirmados no contexto do pedido. A ausência dos arquivos da aplicação não significa ausência de banco.
- Não houve execução de consultas, migrations, build ou testes nesta revisão.

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

Entregue: documentação do contexto, inventário das evidências e critérios de disponibilidade, identidade visual, segurança e validação.

Pendente: identificar os arquivos da aplicação e o mecanismo administrativo no projeto; implementar e integrar o painel; usar o ativo oficial de marca; validar desktop e mobile. A implementação não deve substituir a stack nem criar uma aplicação paralela para compensar os arquivos ausentes da listagem.
