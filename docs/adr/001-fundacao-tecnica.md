# ADR-001: Fundação técnica — stack, white-label, i18n, CI/CD

- **Status:** Aceito
- **Ticket:** [ST-155](https://spirandeli.atlassian.net/browse/ST-155)

## Contexto

Rabboni é uma plataforma de ensino white-label (EN-first + PT) construída pela
fábrica de agentes Spirandeli. Antes de qualquer scaffolding de código,
precisamos fixar as decisões técnicas de fundação — stack, estratégia de
white-label, internacionalização e pipeline de CI/CD — para que os próximos
tickets de implementação partam de um consenso único e não rediscutam essas
escolhas a cada execução. O teto de infraestrutura para o MVP é de US$ 15/mês,
o que restringe as opções de hospedagem e serviços gerenciados.

## Decisão

**Stack**
- Next.js 15 (App Router), output standalone — portabilidade garantida.
- Tailwind CSS v4 + shadcn/ui.
- Postgres como banco de dados principal.
- Deploy via Vercel Hobby enquanto custo = US$ 0 (diretriz do investidor — Decision Log v16).
- Storage de arquivos: Cloudflare R2.
- Autenticação: solução open-source (Auth.js/better-auth).
- Vídeos: embeds do Vimeo da escola — a plataforma nunca hospeda mídia própria.

**Gatilho de saída — Vercel → VPS**
- Condição de saída (qualquer uma basta): Vercel solicitar upgrade para plano pago OU custo
  projetado ultrapassar US$ 10/mês. Nesse evento, Mateus (CFO) escala ao CEO para autorização
  de migração para VPS (Hetzner CX21 ou similar, ~US$ 5-8/mês).

**Lei de portabilidade**
- `output: 'standalone'` obrigatório no `next.config`. Nenhuma API exclusiva da Vercel
  (Edge Functions, KV, Blob, ISR via Vercel CDN) pode ser usada. Qualquer feature que exija
  API Vercel-exclusiva é considerada desvio desta ADR e deve ser bloqueada em code review.

**White-label**
- Toda cor de interface é consumida exclusivamente via tokens de design.
- Os tokens seguem a spec "Design & Identidade Visual v1.0" (Confluence,
  space ST), que é a fonte de verdade para identidade visual; nenhuma cor é
  hardcoded fora dos tokens.

**i18n**
- Suporte a EN/PT em toda string de interface, com inglês como idioma
  primário de desenvolvimento (EN-first).

**CI/CD**
- GitHub Actions: lint, typecheck e testes (qualidade de código). O deploy
  não é responsabilidade do Actions — é automático via integração Vercel + GitHub
  (commit em `main` → Vercel deploya; PR aberto → Vercel gera preview de PR).
- GitHub Actions NÃO faz deploy; qualquer step de `vercel deploy` no Actions é
  desvio desta ADR.

## Consequências

- Tickets subsequentes de scaffolding (setup de projeto, configuração de
  tokens, integração de i18n, workflow de CI/CD) implementam estas decisões
  sem reabri-las.
- Qualquer string de interface introduzida sem chave de i18n, ou cor aplicada
  fora do sistema de tokens, é considerada desvio desta ADR.
- Escolhas de serviços gerenciados (storage, auth, deploy) ficam restritas ao
  teto de US$ 15/mês de infraestrutura do MVP.
- O build DEVE ser sempre portável: `pnpm next build` produz pasta `.next/standalone`
  funcional em qualquer ambiente Node.js 20 sem dependência de runtime Vercel.
  Falha nesse critério é bloqueante de merge.
- Ao atingir o gatilho de saída (plano pago ou custo > US$ 10/mês projetado),
  a migração para VPS não exige nova ADR — apenas execução do plano de infra
  (épico ST-14x). Esta ADR permanece válida com a seção de deploy atualizada.
- Mudanças em qualquer um dos demais pontos exigem uma nova ADR que supersede esta.
