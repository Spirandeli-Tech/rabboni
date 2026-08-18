# Rabboni — regras do repositório

Plataforma de ensino white-label (EN-first + PT) construída pela fábrica de
agentes Spirandeli. Specs no Confluence (space ST); tickets no Jira (ST).

## Execução autônoma (LEI)
O trabalho aqui é executado por agentes via runner, sem humano na conversa:
- **NUNCA termine uma execução perguntando algo.** Na ambiguidade, adote a
  leitura mais fiel ao ticket, execute, e REGISTRE a escolha feita (no corpo do
  commit/PR). Pergunta sem resposta = trabalho perdido.
- Escopo é o do ticket, exatamente: nem menos, nem o vizinho tentador.
- Ticket de ADR produz APENAS o documento em `docs/adr/NNN-titulo.md`
  (formato: Contexto / Decisão / Consequências) — sem scaffolding de código,
  salvo se o ticket pedir explicitamente.

## Decisões que valem aqui (não rediscutir em execução)
- Stack: Next.js 15 (App Router) self-hosted · Tailwind CSS v4 + shadcn/ui ·
  Postgres. Deploy: Vercel Hobby enquanto custo = $0 (gatilho de saída: plano pago ou custo projetado > US$10/mês → VPS). CI (lint/typecheck/testes) no GitHub Actions. Teto de infra MVP: US$ 15/mês. LEI DE PORTABILIDADE: build standalone, nenhuma API exclusiva da Vercel.
- Storage de arquivos: Cloudflare R2. Auth: open-source (Auth.js/better-auth).
  Vídeos: embeds do Vimeo da escola (nunca hospedar mídia).
- Design: tokens da spec "Design & Identidade Visual v1.0" (Confluence) são lei.
  Cor SÓ via tokens (white-label). i18n EN/PT em toda string de interface.

## Scaffolding e dependências (LEI)
- Projeto/estruturas novas SEMPRE via CLI oficial: `pnpm create next-app`,
  `pnpm dlx shadcn@latest init/add`, `pnpm add <pkg>`. NUNCA escrever
  package.json, lockfiles ou boilerplate de framework à mão — lockfile só
  existe como resultado de install real.
- Depois do scaffold, customize o gerado; não o contrário.
- Antes de abrir PR: `pnpm build` LOCAL tem que passar — PR que não builda
  não nasce (check da Vercel é pré-condição de merge).

## Convenções
- Branch por ticket: `st-NNN-descricao-curta`. Commits em inglês, imperativos.
- PR into `main`, título `[ST-NNN] <entrega>`, corpo com o que foi feito e as
  escolhas registradas.
