# Skill: Landing Page Single-File (Dark, GHL-Compatible)

## O que é este skill

Cria landing pages de alta conversão num único ficheiro HTML/CSS/JS — sem frameworks, sem dependências externas. Compatível com GoHighLevel, Webflow, qualquer servidor estático.

---

## Stack & Decisões de Arquitectura

| Decisão | Escolha | Porquê |
|---|---|---|
| Ficheiro único | Sim | GHL-compatible, zero dependências |
| Framework CSS | Nenhum (custom) | Sem overhead, total controlo |
| Fonts | Inter via Google Fonts | Leve, legível, profissional |
| Animações | CSS + IntersectionObserver | GPU-accelerated, sem JS pesado |
| Imagens | Ficheiros externos referenciados | Fácil de trocar |

---

## CSS Custom Properties (Design Tokens)

```css
:root {
  --green: #6aebaf;          /* cor de acento principal */
  --bg:    #0a0a0a;          /* fundo escuro base */
  --bg2:   #111111;          /* fundo secções alternadas */
  --bg3:   #161616;          /* cards, elementos elevados */
  --border: rgba(255,255,255,0.07);
  --muted:  rgba(255,255,255,0.45);
  --text:   #f0f0f0;
}
```

---

## Estrutura de Secções (ordem optimizada para tráfego Instagram)

Ordem validada para ICP que vem do Instagram com intenção prévia (comentou "mentoria"):

```
1.  Navbar            — logo + CTA fixo ("Candidatar-me", white-space:nowrap)
2.  Hero              — headline dor + sub + VSL embutido + CTA baixo risco
3.  Stats Bar         — 4 números reais (credibilidade imediata)
4.  About             — foto + "Não sou consultor. Sou operador." + 3 parágrafos
5.  Image Montage     — prints de resultados de clientes
6.  Prova Real        — before/after slider (ex: ROAS 1.4 → 8.0)
7.  Triângulo Dourado — framework visual de posicionamento
8.  Spider Chart      — radar comparativo: sem sistema vs com sistema vs com IA
9.  9 Pilares         — módulos do programa, descrições orientadas ao resultado
10. Inspiração        — carrossel de referências (Hormozi, Brunson, etc.)
11. Logos Marquee     — ferramentas / integrações
12. Cenário A vs B    — comparação "continuar sozinho" vs "com mentoria"
13. Testemunhos       — DMs / prints de resultados de clientes reais
14. CTA Final         — headline de dor + sub de baixo risco + botão
15. Footer
16. Modal             — formulário de candidatura
```

**Lógica da ordem:** Credibilidade primeiro (stats + about) → Prova concreta (prints + ROAS) → Framework (spider + pilares) → Social proof (referências + testemunhos) → Conversão.

---

## Componentes Reutilizáveis

### Navbar fixa
```html
<nav class="navbar" id="navbar">
  <div class="navbar__inner container">
    <a href="#" class="navbar__brand">Nome<span> | Tagline</span></a>
    <button class="btn btn--green open-modal">Candidatar-me</button>
  </div>
</nav>
```

### Tag / Pill de secção
```html
<div class="tag">NOME DA SECÇÃO</div>
```

### Before/After Slider (clip-path reveal)
```html
<div class="ba-reveal" id="baReveal" style="--pos:50%">
  <div class="ba-reveal__layer">
    <img src="depois.jpg" alt="Depois" id="baAfterImg">
  </div>
  <div class="ba-reveal__before-layer" id="baBeforeLayer">
    <img src="antes.jpg" alt="Antes" id="baBeforeImg">
  </div>
  <div class="ba-lbl ba-lbl--before">Antes – 31 dias</div>
  <div class="ba-lbl ba-lbl--after">Depois – 10 dias</div>
  <div class="ba-reveal__handle" id="baHandle">
    <div class="ba-reveal__btn"><!-- chevron SVG --></div>
  </div>
</div>
```

CSS chave:
```css
.ba-reveal { position: relative; overflow: hidden; --pos: 50%; }
.ba-reveal__before-layer {
  position: absolute; inset: 0;
  clip-path: inset(0 calc(100% - var(--pos)) 0 0);
}
```

JS chave (drag + touch):
```js
handle.addEventListener('pointerdown', e => {
  e.preventDefault();
  const move = ev => {
    const r = reveal.getBoundingClientRect();
    const pct = Math.min(100, Math.max(0, (ev.clientX - r.left) / r.width * 100));
    reveal.style.setProperty('--pos', pct + '%');
  };
  window.addEventListener('pointermove', move);
  window.addEventListener('pointerup', () => window.removeEventListener('pointermove', move), {once:true});
});
```

### Logos Marquee (scroll infinito)
```html
<div class="marquee-track">
  <div class="marquee-inner" id="marqueeInner">
    <!-- Duplicar os items para loop contínuo -->
    <div class="logo-item"><!-- SVG logo --></div>
    ...
  </div>
</div>
```

```css
@keyframes marquee { from { transform: translateX(0); } to { transform: translateX(-50%); } }
.marquee-inner { display: flex; animation: marquee 28s linear infinite; }
```

### Spider/Radar Chart (SVG puro)
- ViewBox recomendado: `-10 0 420 410`
- Centro: `(200, 200)`, raio máximo: `~105px`
- 8 eixos em `polygon` + labels SVG `<text>`
- Legenda como HTML abaixo do SVG (não dentro)

### Foto com fade gradient
```css
.foto-container {
  position: relative; overflow: hidden;
  aspect-ratio: 3/4; border-radius: 16px;
}
.foto-container img {
  position: absolute; inset: 0;
  width: 100%; height: 100%;
  object-fit: cover; object-position: center 15%;
}
.foto-container::after {
  content: ''; position: absolute;
  bottom: 0; left: 0; right: 0; height: 22%;
  background: linear-gradient(to top, var(--bg) 0%, transparent 100%);
  pointer-events: none;
}
```

### Animação de entrada (IntersectionObserver)
```css
.anim { opacity: 0; transform: translateY(24px); transition: opacity .6s ease, transform .6s ease; }
.anim.visible { opacity: 1; transform: none; }
.anim-delay-1 { transition-delay: .12s; }
.anim-delay-2 { transition-delay: .24s; }
```

```js
const io = new IntersectionObserver(entries => {
  entries.forEach(e => { if (e.isIntersecting) { e.target.classList.add('visible'); io.unobserve(e.target); } });
}, { threshold: 0.15 });
document.querySelectorAll('.anim').forEach(el => io.observe(el));
```

---

## Alinhamento de Imagens Before/After

Se as duas imagens foram tiradas em momentos diferentes (dimensões diferentes), usar PowerShell para alinhar os cabeçalhos de colunas:

```powershell
Add-Type -AssemblyName System.Drawing

# Detectar linha do separador em ambas as imagens
# Comparar Y positions → calcular offset
# Criar nova imagem com conteúdo deslocado N px para cima/baixo

$shift = 15  # píxeis a deslocar
$stream = [System.IO.File]::OpenRead("depois.jpg")
$src = [System.Drawing.Image]::FromStream($stream)
$bmp = New-Object System.Drawing.Bitmap($src.Width, $src.Height)
$g = [System.Drawing.Graphics]::FromImage($bmp)
$g.Clear([System.Drawing.Color]::White)
$srcRect = New-Object System.Drawing.Rectangle(0, $shift, $src.Width, ($src.Height - $shift))
$dstRect = New-Object System.Drawing.Rectangle(0, 0, $src.Width, ($src.Height - $shift))
$g.DrawImage($src, $dstRect, $srcRect, [System.Drawing.GraphicsUnit]::Pixel)
$g.Dispose(); $stream.Close(); $src.Dispose()
# Guardar com qualidade 95
$encoder = [System.Drawing.Imaging.ImageCodecInfo]::GetImageEncoders() | Where-Object { $_.MimeType -eq "image/jpeg" }
$params = New-Object System.Drawing.Imaging.EncoderParameters(1)
$params.Param[0] = New-Object System.Drawing.Imaging.EncoderParameter([System.Drawing.Imaging.Encoder]::Quality, 95L)
$bmp.Save("depois-aligned.jpg", $encoder, $params)
```

---

## Responsividade — Breakpoints

```css
@media (max-width: 768px) {
  .about__inner { grid-template-columns: 1fr; }
  .about__photo { aspect-ratio: 4/3; }
  .stats-grid { grid-template-columns: 1fr 1fr; }
  .btn--green { white-space: nowrap; }
}
```

---

## Minificação (Node.js)

```js
const fs = require('fs');
let html = fs.readFileSync('page.html', 'utf8');
html = html.replace(/<!--(?!\[if)[\s\S]*?-->/g, ''); // remove comentários HTML
html = html.replace(/\s{2,}/g, ' ');                 // colapsa whitespace
html = html.replace(/> </g, '><');                   // remove espaço entre tags
html = html.trim();
fs.writeFileSync('page.min.html', html);
```

---

## Checklist antes de entregar

- [ ] Todas as imagens locais referenciadas existem na pasta
- [ ] Botão CTA principal abre modal
- [ ] Before/after slider funciona com touch em mobile
- [ ] Animações de entrada visíveis no primeiro scroll
- [ ] Navbar muda background ao fazer scroll
- [ ] `white-space: nowrap` no botão da navbar em mobile
- [ ] Logos marquee pausam ao hover
- [ ] Spider chart legenda fora do SVG (HTML abaixo)
- [ ] Compact gerado e testado
- [ ] Testado em mobile 375px e desktop 1280px

---

---

## Copy Framework — ICP com Intenção Prévia (Instagram → Landing Page)

Para ICPs que já viram conteúdo orgânico e têm intenção, o copy segue este padrão:

### Hero
- **Headline:** nomeia o estado actual do ICP, não o sonho. Ex: "Tens uma loja que vende. Mas ainda não chegaste onde sabes que consegues."
- **Sub:** identifica o problema real (sistema, não esforço). Ex: "Trabalhas mais do que nunca, mas os números não acompanham. O problema não é o esforço — é o sistema."
- **CTA:** baixo risco + tempo definido. Ex: "Quero candidatar-me — 2 minutos, sem compromisso →"
- **Note abaixo do CTA:** prova de atenção pessoal. Ex: "Respondo a todas as candidaturas pessoalmente."

### Stats Bar — formato validado
```
[número]   [número]        [número]             [número]
13 anos    €1M+            +25 000              3 clientes
a construir  faturados com   encomendas           vagas ativas —
negócios     marcas próprias  processadas —       não aceito todos
online       — não em        sei o que falha
             consultoria     à escala
```
Padrão: número grande → label que qualifica a credibilidade + destrói objecções ("não em consultoria", "não aceito todos").

### About — estrutura de 3 parágrafos
1. **Experiência pessoal no mesmo caminho** — "Já passei pelo 3k/mês, pelo 10k, pelo 50k."
2. **Prova de resultados reais** — números concretos, não consultoria teórica
3. **Escassez legítima** — máximo de clientes, acesso real ao operador

### Pilares / Módulos — formato das descrições
Cada descrição: **O que faz** (1 frase) + **Resultado directo** (1 frase).  
Máximo 2 frases. Sem introduções. Começa sempre pelo benefício ou pelo mecanismo.

### CTA Final — fórmula
```
Headline: [dor que continua se não agir] — não promessa
Sub:      [o que acontece a seguir] — ultra-concreto, sem risco
Botão:    verbo de acção + contexto de baixo risco
```

---

## Ficheiro de Referência

`mentoria-manuel-leitao.html` — landing page completa de Manuel Leitão, Growth Partner.  
Copy e ordem de secções validados para tráfego Instagram com intenção prévia.  
Usa este ficheiro como base para futuros projectos similares (mentoria, consultoria, serviços premium B2C).
