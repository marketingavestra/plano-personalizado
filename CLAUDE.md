# CLAUDE.md — Gerador de Propostas Comerciais para Advogados

## O que é este projeto

Landing page + gerador de proposta comercial personalizada para advogados, desenvolvido para a **Avestra** (agência) em benefício do cliente **Dr. Wladmir Bonadio** (`wladmirbonadio.com.br`).

O usuário preenche um formulário modal com 7 campos, clica em "Receber Minha Proposta Agora" e a página oculta a landing, exibe a seção `#playbook-result` e preenche dinamicamente um plano comercial personalizado em 6 seções — tudo com dados mockados em JavaScript puro, sem nenhuma chamada de API externa.

**Arquivo único:** `index.html` (~1.620 linhas). Não há bundle, transpile ou build step.

---

## Design System — Pulse Blue

Inspirado no template Pulse (fundo escuro, estrelas animadas, botão shiny com border-spin) mas com paleta **azul** em vez de vermelha.

### Variáveis CSS

```css
--b1: #204080   /* azul base / fundo de elementos primários */
--b2: #306090   /* hover de botões */
--b3: #5070B0   /* border-spin, borda de cards */
--b4: #6090C0   /* labels, tags, meta-texto */
--b5: #90C0E0   /* destaques, gradientes de texto, estrelas */
--n1: #E0E0D0   /* texto principal */
--n2: #F0F0F0   /* texto branco suave */
--n3: #B0C0D0   /* texto secundário */
--n4: #7090C0   /* texto terciário / disclaimers */
--bg: #04080F   /* fundo da página */
```

### Componentes visuais principais

| Componente | Classe | Notas |
|---|---|---|
| Fundo de estrelas | `.stars-layer .s1 / .s2` | box-shadow trick, `animStar` 60s/90s |
| Botão CTA principal | `.shiny-btn` | CSS `@property --ga` + `conic-gradient` border-spin 2.5s |
| Cards de alvo | `.target-card` | borda azul `rgba(80,112,176,0.18)` |
| Scroll reveal | `.reveal` → `.reveal.on` | IntersectionObserver, threshold 0.08 |
| Animações hero | `.up1`–`.up6` | `fadeInUp` com delay 0.1s–0.6s |

### Fontes

- **Manrope** (títulos, labels, botões) — via Google Fonts
- **Inter** (corpo de texto) — via Google Fonts

---

## Estrutura da Página

```
<head>
  GTM dataLayer init + UTM capture (inline script)
  GTM snippet (Stape CDN)
  Tailwind CDN
  Google Fonts (Manrope + Inter)
  <style> (todo o CSS inline)

<body>
  GTM noscript fallback
  Stars layers (.s1, .s2)
  Ambient glows
  .top-banner
  <nav> (logo Avestra + dot "Disponível Agora")
  <main id="landing-page">
    Hero (headline, badges, CTA shiny-btn, checklist)
    Para quem é (3x .target-card)
    .not-for-box (NÃO é para)
    Final CTA section
  </main>
  Modal form (#modal-overlay > .modal-card)
  Playbook result (#playbook-result)
    .pb-header (título + subtítulo personalizados)
    .pb-body
      Seção 01 — Diagnóstico (#diag-*)
      Seção 02 — Gargalos (#gargalos-grid)
      Seção 03 — Processo Comercial (#flow-steps)
      Seção 04 — Roteiro de Reunião / SPIN (#script-grid)
      Seção 05 — Plano 30/60/90 dias (#phase-grid)
      Seção 06 — Projeção de Resultado (#compare-grid)
      CTA Final (#pb-cta-box — só exibido para leads qualificados)
    <footer>
  <script> (toda a lógica JS inline)
```

---

## Modal Form

### Comportamento

- `openModal()` — adiciona `.open` ao `#modal-overlay`, trava scroll do body
- `closeModal()` — remove `.open`, restaura scroll
- `handleOverlayClick(e)` — fecha ao clicar fora do `.modal-card`
- Botão X circular (`.modal-close`) no canto superior direito

### Campos do formulário

| ID | Tipo | Descrição |
|---|---|---|
| `f-nome` | text | Nome e sobrenome (required) |
| `f-phone` | tel | Telefone — prefixo `🇧🇷 •` fixo, `padding-left:58px` |
| `f-email` | email | E-mail (required) |
| `f-role` | select | Papel no escritório — **usado para qualificação** |
| `f-nicho` | select | Área de atuação (9 opções) |
| `f-tamanho` | select | Tamanho do escritório (4 opções) |
| `f-fat` | select | Faturamento mensal médio (5 faixas) |

### Qualificação de lead (`isQualified`)

```js
const isQualified = role === 'dono-consolidado' || role === 'dono-novo';
```

- **Qualificado:** exibe o bloco `#pb-cta-box` (CTA de diagnóstico gratuito + WhatsApp da Luíza)
- **Não qualificado** (colaborador/estagiário): gera o playbook completo mas oculta o CTA de upsell

---

## Dados Mockados (JS puro)

### `NICHOS` — 9 entradas

Chaves: `previdenciario`, `trabalhista`, `familia`, `criminal`, `empresarial`, `imobiliario`, `tributario`, `civel`, `outro`

Campos por nicho:
```js
{
  label, icon, cliente, dor, proposta,
  ticket,          // ex: 'R$3.000–R$8.000'
  concorrencia,    // 'alta' | 'muito alta' | 'média' | 'baixa a média'
  dif,             // diferencial de posicionamento
  script_s,        // pergunta SPIN — Situação
  script_p,        // pergunta SPIN — Problema
  script_i,        // pergunta SPIN — Implicação
  script_n,        // pergunta SPIN — Necessidade
  tip,             // dica crítica específica do nicho
}
```

### `TAMANHOS` — 4 entradas

Chaves: `solo`, `pequeno`, `medio`, `grande`

Campos: `label`, `meta`, `conv` (taxa de conversão alvo), `estrutura`, `foco`

### `FATS` — 5 entradas

Chaves: `ate5k`, `'5a15k'`, `'15a30k'`, `'30a60k'`, `acima60k`

> Nota: as chaves com números (ex: `'5a15k'`) devem ser acessadas via bracket notation — `FATS['5a15k']`.

Campos: `label`, `meta90` (projeção em 90 dias), `novos` (novos clientes/mês), `fase`

---

## `buildPlaybook(nome, nicho, tamanho, fat, isQualified)`

Função principal do gerador. Chamada após submit do form. Popula o DOM do `#playbook-result` com innerHTML personalizado.

**Fluxo interno:**
1. Resolve `n = NICHOS[nicho]`, `t = TAMANHOS[tamanho]`, `f = FATS[fat]` (com fallbacks)
2. Extrai o primeiro nome (`fn`) de `nome`
3. Preenche `#pb-title` e `#pb-subtitle`
4. Exibe/oculta `#pb-cta-box` com base em `isQualified`
5. Popula as 6 seções com innerHTML combinando dados dos 3 maps

**Após submit:**
```js
closeModal();
buildPlaybook(...);
document.getElementById('landing-page').style.display = 'none';
document.getElementById('playbook-result').classList.add('show');
window.scrollTo({ top: 0, behavior: 'smooth' });
setTimeout(initReveal, 200); // reconfigura scroll reveal para a nova seção
```

---

## Tracking

### GTM

- Container: **GTM-W98WXF4D**
- Carregado via **Stape CDN** (`www.googletagmanager.com/gtm.js`)
- Mesmo container do projeto principal `wladmirbonadio.com.br`
- Meta Pixel, GA4 e CAPI passam por aqui via Stape server-side

### UTM Capture (inline no `<head>`)

```js
// Lê UTMs da URL → salva em sessionStorage('avestra_utms') → expõe window._avestra_utms
// Se não há UTMs na URL, recupera da sessionStorage (sobrevive a recarregamentos)
// Dispara dataLayer.push({ event: 'utm_captured', utm_data: {...} })
```

Chaves capturadas: `utm_source`, `utm_medium`, `utm_campaign`, `utm_content`, `utm_term`

### Eventos dataLayer

| Evento | Quando dispara | Dados enviados |
|---|---|---|
| `utm_captured` | Se UTMs detectados na URL | `{ utm_data }` |
| `page_view_playbook` | Na carga da página | `{ page_type, page_url, utm_data }` |
| `generate_lead` | Submit do form — **só para leads qualificados** | `{ user_data, form_data, utm_data }` |

**Estrutura do `generate_lead`** (idêntica ao Form.jsx do projeto Sala Secreta):
```js
{
  event: 'generate_lead',
  user_data: {
    email: '...',
    phone_number: '+55...',   // +55 + dígitos limpos
    address: { first_name: '...', last_name: '...' }
  },
  form_data: { role, nicho, tamanho, fat },
  utm_data: window._avestra_utms || {}
}
```

### UTM Pass-Through

No evento `window load`, propaga as UTMs para todos os links que apontam para:
- `wladmirbonadio.com.br`
- `calendly.com`

Se a URL atual não tem UTMs, recupera da `sessionStorage` antes de propagar.

---

## Conexão com o Projeto Principal

| Item | Detalhe |
|---|---|
| GTM Container | Mesmo (`GTM-W98WXF4D`) — mesmos tags do site principal |
| Formato `generate_lead` | Idêntico ao `Form.jsx` do projeto Sala Secreta |
| `window._avestra_utms` | Mesmo padrão de exposição global |
| `sessionStorage key` | `avestra_utms` — igual nos dois projetos |
| UTM pass-through | Propaga para `wladmirbonadio.com.br` e `calendly.com` |

---

## Responsividade

Breakpoint principal: `@media (max-width: 700px)`

Ajustes mobile:
- `.phase-grid` → 1 coluna
- `.script-grid` → 1 coluna
- `.compare-grid` → 1 coluna
- `.metric-grid` → mantém 2 colunas

---

## Decisões de Design Importantes

- **Sem build step, sem framework** — arquivo HTML único para facilitar deploy e edição direta
- **Sem chamadas de API** — todo playbook gerado com dados estáticos em JS; velocidade percebida de "IA" vem do `setTimeout` de 1.4s
- **CTA de upsell condicional** — colaboradores e estagiários recebem o playbook mas não são chamados para o diagnóstico (Avestra só quer conversar com donos/sócios)
- **Método SPIN** — cada nicho tem 4 perguntas personalizadas para o roteiro de reunião
- **Modal estilo clean/branco** (#fff card, borda #e4e4e4) — contraste intencional com o fundo escuro da página; botão submit em âmbar (#f59e0b) para destacar
