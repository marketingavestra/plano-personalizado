# Status do Rastreamento — Operação Wladmir Bonadio

Este documento detalha como está configurada toda a inteligência de rastreamento, atribuição e dados da landing page de captação de advogados (Gerador de Propostas).

## 1. Infraestrutura Base
- **Google Tag Manager (GTM):** `GTM-W98WXF4D`
- **Método de Carregamento:** Via **Stape CDN** (`https://www.googletagmanager.com/gtm.js`).
- **Vantagem:** Melhora a performance, aumenta a precisão do rastreamento (contorna adblockers) e centraliza GA4, Meta Pixel e Conversion API (CAPI) no servidor da Stape.

---

## 2. Inteligência de UTMs (Atribuição)
O sistema possui uma lógica de captura e persistência de UTMs para garantir que saibamos exatamente de onde veio o lead, mesmo que ele navegue entre páginas.

- **Captura Automática:** Captura `utm_source`, `utm_medium`, `utm_campaign`, `utm_content` e `utm_term`.
- **Persistência:** Salva os dados no `sessionStorage` (`avestra_utms`). Se o usuário recarregar a página ou voltar depois, os dados originais são mantidos.
- **Detecção Inteligente (Fallback):** Se o usuário chegar sem UTMs (tráfego orgânico/direto), o sistema detecta automaticamente:
    - `instagram`: Acesso via app ou link na bio.
    - `whatsapp`: Cliques vindos do app de mensagens.
    - `google`: Busca orgânica.
    - `facebook`: Acesso via perfil/post.
    - `direto`: Digitado no browser.
- **Normalização:** Todos os valores são convertidos para minúsculas e espaços são substituídos por hifens (ex: `Google Ads` → `google-ads`).



## 4. Eventos e DataLayer
Eventos disparados para o GTM processar e enviar para Meta/Google/TikTok:

| Evento | Quando ocorre | Dados enviados |
| :--- | :--- | :--- |
| `utm_captured` | No carregamento da página | Dados de UTM e tipo de tráfego (`pago` ou `organico`). |
| `page_view_playbook` | No carregamento da página | Identifica o tipo específico de página (`captacao_playbook_advogado`). |
| `generate_lead` | No destaque do formulário | **Dados Completos:** E-mail, Telefone (formato E.164), Nome, Lead Score (se houver), Tier (Frio/Morno/Quente), Origem do Produto (`ia-advogados` / `ia-proposta`) e UTMs. |

---

## 5. Armazenamento de Leads (Database)
Além do rastreamento de marketing, os leads são salvos em tempo real no **Supabase**.
- **Tabela `leads_propostas`:** Leads do funil principal de diagnóstico comercial.
- **Tabela `leads_ia`:** Leads interessados nos Agentes de IA (`ia-advogados`).
- **Campos salvos:** Dados do form + Score de qualificação + Atribuição completa (UTMs) + Origem (`product_origin`).
- **Status da Integração:** ✅ Ativa e Operacional.

---

## 6. Fluxo de Conversão e Redirects
Após o formulário, o lead é enviado para uma página de agradecimento específica baseada na sua nota (Score):
- **Lead Quente:** `/obrigado/quente.html` (Foco em agendamento imediato).
- **Lead Morno:** `/obrigado/morno.html`.
- **Lead Frio:** `/obrigado/frio.html`.

> [!TIP]
> **Consistência:** O formato do evento `generate_lead` é idêntico ao projeto "Sala Secreta", garantindo que as tags de conversão do Meta Pixel e GA4 funcionem perfeitamente sem necessidade de novas configurações no GTM.

---
**Última Atualização:** 14/04/2026
**Responsável:** Antigravity (Agência Avestra)
