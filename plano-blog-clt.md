# Plano: Blog Jurídico CLT — Dr. Wladmir Bonadio Filho
**Data:** 2026-06-11
**Objetivo:** Criar uma seção `/blog` dentro do site wladmirbonadio.com.br 100% focada em conteúdo sobre CLT/Direito do Trabalho, com artigos reais publicados, otimizada para SEO e GEO (busca por IA), gerando autoridade para fechar clientes e posicionamento orgânico no Google.
**Critério de sucesso:** Site navegável localmente com `/blog/index.html` + no mínimo 6 artigos completos (cada um 1200-1800 palavras), todos com meta tags, schema.org (Article + FAQPage), sitemap.xml e robots.txt atualizados, sem quebrar o redirect existente de `/` → `/plano`.

---

## Fase 1 — Estrutura técnica e base SEO/GEO
**Objetivo da fase:** Criar o esqueleto do blog dentro de `ATUALIZADO/`, sem conflitar com o redirect atual, com layout reaproveitável e fundação técnica de SEO.
**Prazo estimado:** 2-3h

### Tarefas
- [x] 1.1 Criar pasta `/blog` com `index.html` (listagem de artigos) usando o design system "Pulse Blue"
- [x] 1.2 Criar template HTML reaproveitável para posts (`/blog/_template.html`) com slots para título, meta description, conteúdo, FAQ e CTA para `/plano`
- [x] 1.3 Adicionar bloco de meta tags padrão (title, description, canonical, Open Graph, Twitter Card) ao template
- [x] 1.4 Adicionar JSON-LD schema.org `Article` + `FAQPage` + `BreadcrumbList` ao template
- [x] 1.5 Criar/atualizar `sitemap.xml` e `robots.txt` na raiz incluindo as novas URLs `/blog` e `/blog/*`
- [x] 1.6 Confirmar no `vercel.json` que `/blog` e subpáginas NÃO caem no redirect `/` → `/plano` (testar regra `missing utm_source`)

### Entregável
Esqueleto funcional do blog (`/blog/index.html` + template) navegável localmente, com SEO técnico de base pronto e sem conflito de redirect.

---

## Fase 2 — Pauta editorial CLT (GEO + SEO research)
**Objetivo da fase:** Definir a lista de temas/títulos com maior potencial de busca (Google) e de citação por IA (GEO), priorizando dúvidas reais de trabalhadores.
**Prazo estimado:** 1-2h

### Tarefas
- [x] 2.1 Levantar 10-12 temas de CLT com alta intenção de busca (rescisão, horas extras, FGTS, justa causa, equiparação salarial, adicional de insalubridade/periculosidade, estabilidade gestante, assédio moral, banco de horas, demissão por acordo)
- [x] 2.2 Para cada tema, definir título otimizado (formato pergunta, "como", "o que fazer") + intenção de busca + 3-5 perguntas para o FAQ
- [x] 2.3 Priorizar os 6-8 títulos que entram na primeira leva de publicação
- [x] 2.4 Definir estrutura de internal linking (cada post linka para 2-3 outros posts + CTA para `/plano`)

### Entregável
Lista final de 6-8 títulos priorizados, cada um com ângulo, palavras-chave e perguntas de FAQ definidas.

---

## Fase 3 — Produção dos artigos (skill /blog-post + humanizar-texto)
**Objetivo da fase:** Escrever e publicar os 6-8 artigos completos como páginas HTML reais.
**Prazo estimado:** 4-6h

### Tarefas
- [x] 3.1 Gerar artigo 1 (tema mais buscado, ex: "Fui demitido sem justa causa, o que recebo?") via `/blog-post`, revisar com `/humanizar-texto`, montar HTML a partir do template
- [x] 3.2 Gerar artigos 2 e 3 (horas extras + FGTS) seguindo o mesmo fluxo
- [x] 3.3 Gerar artigos 4 e 5 (justa causa + equiparação salarial)
- [x] 3.4 Gerar artigos 6 a 8 (demais temas priorizados na Fase 2)
- [x] 3.5 Adicionar cada artigo ao `/blog/index.html` (card com título, resumo, data) e ao `sitemap.xml`
- [x] 3.6 Revisar internal linking entre todos os posts publicados

### Entregável
6-8 páginas de blog publicadas em `/blog/*.html`, listadas no índice e no sitemap, todas seguindo o padrão de humanização de texto.

---

## Fase 4 — Validação, SEO/GEO final e checagem
**Objetivo da fase:** Garantir que tudo funciona, está indexável e segue os padrões de segurança/SEO do projeto.
**Prazo estimado:** 1-2h

### Tarefas
- [x] 4.1 Abrir `/blog/index.html` e 2-3 posts no navegador local, validar layout, links e CTAs
- [x] 4.2 Validar JSON-LD (schema Article/FAQPage) sem erros de sintaxe
- [x] 4.3 Checar headers de segurança do `vercel.json` continuam aplicados às novas rotas
- [x] 4.4 Conferir `sitemap.xml`/`robots.txt` apontam corretamente para `wladmirbonadio.com.br/blog/...`
- [x] 4.5 Registrar no CLAUDE.md do projeto a nova seção `/blog` e o fluxo de publicação de novos posts

### Entregável
Blog 100% funcional, validado localmente, documentado para futuras publicações.

---

## Milestones
| Marco | Fase | Entregável |
|-------|------|-----------|
| M1 | Fim Fase 1 | Estrutura `/blog` + template SEO/GEO pronto |
| M2 | Fim Fase 2 | Pauta de 6-8 títulos definida com FAQ e keywords |
| M3 | Fim Fase 3 | 6-8 artigos publicados e linkados |
| M4 | Fim Fase 4 | Blog validado, documentado e pronto para deploy |

---

## Riscos e Bloqueios
- **Risco:** Conflito com redirect `/` → `/plano` afetando indexação do blog → **Mitigação:** manter regra de redirect restrita à rota exata `/`, blog vive em `/blog/*` que não é afetado.
- **Risco:** Conteúdo genérico de IA sem voz humana, prejudicando GEO e confiança → **Mitigação:** todo texto passa por `/humanizar-texto` antes de virar HTML.
- **Risco:** Falta de dados estruturados corretos reduz chance de aparecer em AI Overviews → **Mitigação:** template já inclui JSON-LD Article + FAQPage validado.

---

## Próximo passo imediato
Executar Fase 1.1: criar a pasta `/blog` com `index.html` inicial usando o design system "Pulse Blue" do projeto.