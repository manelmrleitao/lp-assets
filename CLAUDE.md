# Landing Page Mentoria — Manuel Leitão

## Descrição
Landing page de candidatura para mentoria de e-commerce (Shopify). Público-alvo: lojas Shopify entre 3k–10k€/mês. Construída em 4 Custom Code blocks no GoHighLevel (GHL).

---

## Stack
- **Plataforma:** GoHighLevel (GHL) — Custom Code blocks (4 partes)
- **CSS externo:** GitHub Pages → `https://manelmrleitao.github.io/lp-assets/mentoria-styles.css`
- **Fontes:** Google Fonts — Inter
- **Analytics:** GTM (`GTM-M8FH6N6L`) + Meta Pixel (`1901126810554894`)
- **Form:** GHL Native Form (iframe embed) — "Candidatura Mentoria LP"
- **Workflow GHL:** "Lead Capture and Notification"

---

## Ficheiros

| Ficheiro | Conteúdo | GHL |
|---|---|---|
| `ghl-part1.html` | Hero, navbar, logos, stats, about, montage, **form nativo GHL (iframe)**, comparison, starfield JS | Custom Code 1 |
| `ghl-part2.html` | BA reveal sliders (3x) + Triângulo Dourado + testemunhos marquee + slider JS | Custom Code 2 |
| `ghl-part3.html` | 9 pilares + radar (spider) | Custom Code 3 |
| `ghl-part4.html` | Footer | Custom Code 4 |
| `mentoria-styles.css` | CSS global (este repo, raiz) | GitHub Pages |

**Versão única (Jun 2026):** a antiga variante "fable" (camada imersiva + fundo seamless + form nativo) foi promovida a versão oficial — os ghl-part1-4 SÃO a versão fable. Usar os mesmos ficheiros na página ao vivo (funnel "Mentoria — Landing Page") e na página de teste ("...fable", lp1-page-7743). Pasta `antigo/` contém versões antigas e assets-fonte.

---

## Tracking

### Meta Pixel — `1901126810554894`
- Init + PageView: **ghl-part1.html** (início)
- `ViewContent`: dispara quando utilizador chega ao form (`#candidaturaForm`)
- `Lead`: dispara no submit do form (qualificado ou não)
- Form GHL também dispara `Submit Application` via pixel configurado nas form settings

### GTM — `GTM-M8FH6N6L`
- Instalado em **ghl-part1.html**
- dataLayer push: `lead_submit` (com `qualificada` e `faturacao`)
- dataLayer push: `scroll_to_form`

### GA4
- `gtag('event', 'form_submit')` com `qualificada` e `faturacao`
- `gtag('event', 'scroll_to_form')`

### Cookies guardados no submit
- `_em_hash` — SHA-256 do email (para Meta CAPI)
- `_lead_nome` — nome do lead
- `_fbclid` — fbclid da URL se existir

---

## Formulário GHL

**Form ID:** `kUBQa9Ttoa4dTjkE15db`
**Form name:** Candidatura Mentoria LP
**Embed URL:** `https://api.leadconnectorhq.com/widget/form/kUBQa9Ttoa4dTjkE15db`
**Script:** `https://link.msgsndr.com/js/form_embed.js`

### Campos do form
1. First Name (Nome) — obrigatório
2. Last Name (Apelido) — obrigatório
3. Email — obrigatório
4. Nome do negócio (custom text) — obrigatório
5. Faturação mensal (dropdown): Menos de 1.000€ / 1.000–3.000€ / 3.000–10.000€ / Mais de 10.000€
6. Há quanto tempo tens o negócio (dropdown): <6m / 6–12m / 1–3 anos / +3 anos
7. Maior desafio (textarea) — obrigatório
8. Expectativa com a mentoria (textarea) — obrigatório
9. Checkbox RGPD — obrigatório

### Form settings (REGRAS CRÍTICAS — ver SKILL-landing-page.md secção GHL)
- **Form nativo em iframe** — NUNCA form custom via API (perde leads: endpoint 404/Cloudflare)
- On Submit: redirect com **URL COMPLETO** `https://manelmrleitao.com/obrigado` (sem domínio → https://obrigado/ = erro)
- **Não há split qualificado/não-qualificado** — tudo vai para /obrigado; qualificação é feita na call
- Todos os campos **Required**
- Facebook Pixel ID: `1901126810554894` (evento no submit)
- Email Notification: ON → manelmrleitao@gmail.com (chega com PDF da submissão)
- A página (part1) tem listener postMessage que dispara fbq Lead + redirect /obrigado como fallback

---

## Workflow GHL — "Lead Capture and Notification"
- Trigger: Form Submitted (sem filtros)
- Acções: Create Opportunity → Mentoria / Manuel Leitão, emails

**Armadilhas conhecidas (resolvidas Jun 2026):**
- Time Window nas ações prende emails em "Waiting" — manter 24/7
- Re-entry bloqueada por defeito — testes com o mesmo contacto dão "Skipped"; Allow re-entry ativado
- Ação "Email" envia para o CONTACTO (não para o user) — notificação ao Manuel vem das form settings (com PDF)
- Debug: Execution logs do workflow (Executed/Waiting/Skipped/Failed)
- O Inbound Webhook é premium — não usar; o form nativo dispara o workflow grátis

## Email — Dedicated Domain (configurado Jun 2026)
- `mail.manelmrleitao.com` verificado no GHL (Settings → Email Services → Dedicated Domain)
- 6 registos DNS na GoDaddy (SPF, DKIM, CNAME, 2×MX prio 10, DMARC) — detalhe no SKILL
- Emails caem na caixa Principal do Gmail (validado)
- NÃO tocar nos registos `@` do domínio raiz (email pessoal Google/Microsoft)

---

## BA Reveal Sliders (ghl-part2.html)

Três sliders antes/depois com animação de entrada (sweep 90→50%):
- **baReveal1** / baHandle1 — ROAS Meta Ads
- **baReveal2** / baHandle2 — Suplementos Shopify
- **baReveal3** / baHandle3 — Merchandise

### Stats finais

**ROAS (baReveal1):**
- Título: "De ROAS 1.55 para ROAS 7.3"
- Antes: Ad Spend ~€1.100 (1 mês) · ROAS 1.55× · CPA alto · €1.772 valor gerado
- Depois: Ad Spend ~€460 (10 dias) · ROAS 7.3× · 76 compras · €5.87 CPA · €3.382 valor gerado

**Suplementos (baReveal2):**
- Antes: Ad Spend ~€1.100 (1 mês) · 2.668 visitas · Tx 1.87% · 257 pedidos · €2.675
- Depois: Ad Spend ~€460 (10 dias) · 2.553 visitas · Tx 3.76% · 119 pedidos · €4.975

**Merchandise (baReveal3):**
- Antes: 861 visitas · Tx 0.34% · 4 pedidos · €94
- Depois: 9.827 visitas · Tx 1.08% · 113 pedidos · €3.523

### JS crítico (ghl-part2.html)
- Usa `bl.style.clipPath` directamente (não só CSS variables) — necessário no GHL
- `overflow: visible` no `.ba-reveal` para handle chegar às pontas
- `overflow: hidden` no `.ba-reveal__layer` para cortar imagens
- Retry intervals: `[100, 300, 700, 1500, 3000]ms`
- Per-element flag `el._baOK = true` para evitar double-binding
- Touch events com `{passive: false}`

---

## CSS (mentoria-styles.css — GitHub Pages)

Ficheiro local: `mentoria-styles.css` (raiz deste repo)
URL produção: `https://manelmrleitao.github.io/lp-assets/mentoria-styles.css`
Repo: `https://github.com/manelmrleitao/lp-assets`

Para actualizar: editar → commit → push (GitHub Pages atualiza em ~1-2 min; hard refresh).
O HTML dos blocos é colado À MÃO no GHL (a automação de browser não funciona no builder).

---

## Páginas de Redirect

| URL | Quando |
|---|---|
| `/obrigado` | Todos os submits (único redirect) |

Decisão Jun 2026: **não há split qualificado/não-qualificado** — o Manuel qualifica na call.

---

## Pendente / A fazer

- [ ] Gravar e publicar o vídeo VSL (placeholder ainda no hero)
- [ ] Finalizar copy (textos marcados para revisão)
- [ ] Criar páginas `/obrigado` e `/obrigado-qualificado` no GHL
- [ ] Adicionar screenshots reais nas dash-cards (actualmente placeholder)
- [ ] Testar submissão do form end-to-end (lead aparece em GHL Contacts)
- [ ] (Opcional) Separar redirect qualificado/não-qualificado via workflow GHL

---

## Erros conhecidos / Regras

- **Slider não funciona no GHL:** Sempre usar `bl.style.clipPath` directamente em JS. Não depender só de CSS variables (`--pos`) — o GHL pode não propagar.
- **`window._baRevealsInit` bloqueava re-init:** Removido. Usar `el._baOK` por elemento.
- **Slider cortava nas pontas:** `Math.max(0, Math.min(100, ...))` — não limitar a 4–96%.
- **GHL iframe altura:** `min-height: 680px` no iframe do form + `form_embed.js` para auto-resize.
- **Inbound Webhook = premium:** Não usar — usar form nativo GHL com trigger "Form Submitted".
