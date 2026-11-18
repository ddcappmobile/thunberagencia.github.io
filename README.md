# Thunber Academy — MVP funcional de interface

## Conteúdo

- `index.html`: landing page e catálogo (Neon via Worker, com fallback visual de demonstração).
- `academy.html`: laboratório demonstrativo de JavaScript, 3 missões, dicas, testes e XP locais.
- `admin.html`: catálogo, configuração da API e criação de rascunhos **locais**.
- `assets/`: tema claro/escuro, componentes e sandbox didático.
- `worker/index.js`: Cloudflare Worker de **somente leitura** para catálogo publicado.
- `worker/wrangler.toml.example`: referência de implantação.

## Como executar localmente

Hospede os arquivos com um servidor estático (GitHub Pages ou Live Server). Evite abrir com `file://`, pois o Web Worker pode ser bloqueado pelo navegador.

## Publicação GitHub Pages

Copie `index.html`, `academy.html`, `admin.html` e `assets/` para a raiz do repositório ou pasta publicada. Em Settings > Pages, selecione a branch e a pasta correspondente.

## Integração Neon

O banco existente possui `courses` com colunas `id`, `slug`, `title`, `subtitle`, `description`, `level`, `estimated_hours`, `min_plan_tier`, `status`, `cover_image_url`. A rota `GET /courses` usa exclusivamente essa estrutura e filtra `status=published`. **Nenhum SQL de recriação é necessário.**

1. Crie um Cloudflare Worker e cole o conteúdo de `worker/index.js` no editor do painel, ou faça deploy com Wrangler (opcional).
2. Configure `NEON_DATA_API_URL` com o endpoint `/rest/v1` e `ALLOWED_ORIGINS` com os domínios exatos que servirão a Academy.
3. Configure credencial **de leitura restrita** no segredo `NEON_READONLY_TOKEN`, caso a configuração da Data API exija bearer. Não use token de administrador/bypass RLS. A disponibilidade de acesso público depende das políticas de acesso do Neon; não afrouxe RLS para contornar erro.
4. No `admin.html`, informe a URL HTTPS do Worker e clique em Testar conexão.
5. A API oferece `GET /health` e `GET /courses`; não expõe rotas de escrita.

## O que já funciona

- Navegação entre páginas e responsividade;
- Alternância de tema claro/escuro;
- Catálogo demonstrativo e leitura de cursos publicados pela API;
- Três desafios JavaScript, testes automáticos didáticos, dicas e XP local;
- Rascunhos administrativos locais;
- Worker com filtro de origem e sem operações de escrita.

## Limitações deliberadas (não estão implementadas)

- Cadastro, login, recuperação de senha, sessões e autorização administrativa reais;
- Matrículas, aulas e progresso persistidos no Neon;
- Avaliação de IA, checkout, certificação e comunidade;
- CRUD administrativo remoto.

O laboratório utiliza `Function` em Web Worker para testar exercícios locais. **Não é sandbox segura contra código malicioso**. Antes de aceitar código de terceiros, implante isolamento robusto (ambiente remoto com limites de recursos ou sandbox dedicado). Não coloque segredos no código de frontend.

## Próximo sprint

1. Integrar Neon Auth e mapear identidade autenticada a `users` / `profiles`;
2. Definir e testar permissões de matrícula e progresso;
3. Criar endpoints autenticados para catálogo completo, matrículas e progresso;
4. Integrar aulas reais e exercícios à interface;
5. Implementar CRUD administrativo com autorização no servidor.

## Segurança e implantação

Não envie credenciais do banco para o navegador. Não use token administrativo na variável `NEON_READONLY_TOKEN`. O Worker é intencionalmente somente leitura até existir um contrato de autenticação validado. CORS não substitui autenticação. Para produção, configure a origem exata e faça testes de acesso não autorizado.


## Camada visual (novembro 2026)
- Bootstrap 5.3.9 + Bootstrap Icons 1.13.2 por CDN; CSS local carregado depois para preservar os tokens da Thunber.
- `assets/design-enhancements.css`: botões, cartões, acessibilidade, navegação responsiva, motion e modo claro/escuro.
- `assets/design-enhancements.js`: menu móvel, foco/estado ativo, animação por interseção e helper de feedback.
- Não altera endpoints, tabelas ou lógica de negócio.
- O tom verde atual vem do MVP anterior; substituir pelos tokens oficiais assim que o CSS institucional estiver disponível.
- Páginas `catalogo.html`, `curso.html`, `login.html` são legadas e precisam de revisão funcional antes de publicação.
