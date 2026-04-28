---
name: crear-sesion
description: >
  Crea una nueva sesión HTML institucional (diapositivas 16/9, 4 horas de clase) y
  la guía Markdown para el profesor correspondiente, para el curso Fundamentos /
  Lógica de Programación. Genera: archivo sesiones/sesionN_0.html con explicaciones
  detalladas, elementos visuales CSS/SVG/animaciones y taller guiado integrado;
  y sesiones/guia_sesionN_0.md con respuestas esperadas, instrucciones de aula y
  formato de entrega de evidencias. NUNCA incluye duración ni agenda. Solo usa
  imágenes con licencia libre verificada; preferir SVG/CSS inline.
applyTo: "sesiones/sesion*.html"
---

# Skill: Crear sesión HTML institucional

## 1. Contexto del curso

**Curso**: Lógica de Programación / Fundamentos de Programación  
**Grupos**: 810 / 811  
**Idioma de los archivos**: español (sin tildes en código ni SVG para evitar encoding issues)  
**Lenguaje de programación enseñado**: C# con .NET SDK  
**Herramientas de clase**: VS Code + extensión C#, .NET SDK, GitHub, Draw.io/Lucidchart, PSeInt (referencia)

### Unidades y sesiones ya creadas

| Unidad | Sesiones | Estado |
|--------|----------|--------|
| Unidad 1 | 0 – 8 | Completadas |
| Unidad 2 | 9 – 14 | Completadas |
| Unidad 3 | **15 – 24** | **Por crear** |

### Saberes de Unidad 3 (mapeo a sesiones)

| Sesión | Foco principal |
|--------|---------------|
| 15 | Problema del código monolítico y principio DRY |
| 16 | Concepto de función, firma y contrato |
| 17 | Parámetros, retorno y separación UI / lógica |
| 18 | Descomposición en subproblemas y jerarquía de funciones |
| 19 | Esqueleto modular y alcance de variables |
| 20 | Implementación completa de solución modular |
| 21 | Refinamiento, sobrecarga pertinente y calidad de código |
| 22 | Laboratorio de refactorización (de iterativo a modular) |
| 23 | Pruebas y depuración en interacción entre funciones |
| 24 | Integración U1-U3, ejercicios tipo examen |

---

## 2. Convenciones de archivos

| Recurso | Ruta | Nombre |
|---------|------|--------|
| Sesión HTML | `sesiones/sesionN_0.html` | `sesion15_0.html`, etc. |
| Guía docente | `sesiones/guia_sesionN_0.md` | `guia_sesion15_0.md`, etc. |
| Registro en índice | `index.html` | Añadir tarjeta al grid |

**Fondos institucionales** (ya existen en `sesiones/`):
- `sesiones/Portada.png` → slide de portada
- `sesiones/Contenido.png` → slides de contenido

> Si los fondos no están en `sesiones/`, referenciarlos con ruta relativa `./Portada.png` y `./Contenido.png` (igual que las sesiones existentes).

---

## 3. Variables de marca institucional

### ⚠️ CRITICAL: Dos conjuntos de márgenes — NO SON INTERCAMBIABLES

**La portada (Portada.png) y el contenido (Contenido.png) tienen diseños radicalmente diferentes.
SIEMPRE debes usar las variables CORRECTAS para cada tipo.**

```css
:root {
  --texto:    #201F4B;   /* Azul oscuro — texto principal */
  --azul-medio: #015583; /* Azul medio — títulos, headers */
  --turquesa: #05ADBC;   /* Turquesa — acentos, subtítulo portada */
  --muted:    #6b7280;   /* Gris — notas, metadatos */
  --white:    #ffffff;

  /* Tipografía fluida */
  --h1: clamp(30px, 3.4vw, 48px);
  --h2: clamp(22px, 2.3vw, 34px);
  --p:  clamp(15px, 1.3vw, 19px);
  --li: clamp(15px, 1.3vw, 19px);

  /* PORTADA (slide.cover + Portada.png) */
  /* Imagen tiene logo/espacio GRANDE en izquierda → margen izquierdo MÁS GRANDE */
  --safe-top-cover:    clamp(208px, 9vh, 128px);
  --safe-left-cover:   clamp(585px, 5.5vw, 110px);   /* ← GRANDE (logo) */
  --safe-right-cover:  clamp(28px,  3vw, 72px);
  --safe-bottom-cover: clamp(54px,  6vh, 92px);

  /* CONTENIDO (slide.content-slide + Contenido.png) */
  /* Imagen tiene espacio UNIFORME → márgenes SIMÉTRICOS y MENORES */
  --safe-top:    clamp(100px, 9vh, 132px);
  --safe-side:   clamp(66px,  5.5vw, 108px);         /* ← SIMÉTRICO */
  --safe-bottom: clamp(52px,  6vh, 90px);
}
```

### Aplicación en HTML:

```html
<!-- PORTADA: SIEMPRE slide.cover -->
<section class="slide cover">
  <div class="inner">
    <!-- Contenido aquí -->
  </div>
</section>

<!-- CONTENIDO: SIEMPRE slide.content-slide -->
<section class="slide content-slide">
  <div class="inner">
    <!-- Contenido aquí -->
  </div>
</section>
```

### CSS que aplica cada variable:

```css
.inner {
  padding: var(--safe-top) var(--safe-side) var(--safe-bottom) var(--safe-side);
}
.cover .inner {
  padding: var(--safe-top-cover) var(--safe-right-cover) var(--safe-bottom-cover) var(--safe-left-cover);
  /* Orden: top right bottom left (porque izquierda es diferente) */
}
```

---

## 4. Plantilla HTML completa

El bloque de código siguiente es la plantilla canónica. Reemplaza los marcadores
`{{TITULO}}`, `{{SUBTITULO}}`, `{{UNIDAD_INFO}}`, `{{SLIDE_N}}`, `{{TOTAL}}`
y el contenido de cada sección.

```html
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Lógica de Programación - {{TITULO}}</title>

  <style>
    :root{
      --texto:#201F4B; --azul-medio:#015583; --turquesa:#05ADBC;
      --muted:#6b7280; --white:#ffffff;
      --h1:clamp(30px,3.4vw,48px); --h2:clamp(22px,2.3vw,34px);
      --p:clamp(15px,1.3vw,19px); --li:clamp(15px,1.3vw,19px);
      --safe-top-cover:clamp(208px,9vh,128px);
      --safe-left-cover:clamp(585px,5.5vw,110px);
      --safe-right-cover:clamp(28px,3vw,72px);
      --safe-bottom-cover:clamp(54px,6vh,92px);
      --safe-top:clamp(100px,9vh,132px);
      --safe-side:clamp(66px,5.5vw,108px);
      --safe-bottom:clamp(52px,6vh,90px);
    }
    body{ margin:0; font-family:Arial,Helvetica,sans-serif; background:#ffffff; }
    .deck{ min-height:100vh; display:flex; align-items:center;
           justify-content:center; padding:clamp(10px,2vw,22px);
           box-sizing:border-box; background:#ffffff; }
    .stage{ width:min(96vw,calc(92vh*(16/9))); aspect-ratio:16/9; position:relative; }
    .slide{ position:absolute; inset:0; display:none;
            box-shadow:0 14px 40px rgba(0,0,0,.35); overflow:hidden; }
    .slide.active{ display:block; }
    .slide.cover{
      background:url("./Portada.png") center/cover no-repeat; color:#ffffff;
    }
    .slide.content-slide{
      background:url("./Contenido.png") top left/contain no-repeat, #ffffff;
      color:var(--texto);
    }
    .inner{
      height:100%; box-sizing:border-box;
      padding:var(--safe-top) var(--safe-side) var(--safe-bottom) var(--safe-side);
      display:flex; flex-direction:column; gap:clamp(10px,1.1vw,18px);
    }
    .cover .inner{
      padding:var(--safe-top-cover) var(--safe-right-cover)
              var(--safe-bottom-cover) var(--safe-left-cover);
    }
    .content{ flex:1; display:flex; flex-direction:column;
              gap:clamp(10px,1.1vw,16px); min-height:0; }
    .header{ color:var(--azul-medio); font-size:clamp(18px,1.6vw,22px);
             font-weight:800; letter-spacing:.08em; text-transform:uppercase; }
    h1{ margin:0; font-size:var(--h1); line-height:1.08; color:var(--texto); }
    h2{ margin:0; font-size:var(--h2); line-height:1.15; color:var(--azul-medio); }
    p{ margin:0; font-size:var(--p); line-height:1.4; color:var(--texto); }
    li{ font-size:var(--li); line-height:1.4; color:var(--texto); }
    ul,ol{ margin:0; padding-left:1.2em; }
    .subtitle{ color:var(--turquesa); font-weight:800;
               font-size:clamp(18px,1.8vw,24px); margin-top:clamp(6px,1vw,10px); }
    .cover h1{ color:#ffffff; }
    .cover p{ color:#e5edf8; }
    .cover .subtitle{ color:#f7df00; }
    .note{ color:var(--muted); font-size:clamp(12px,1.05vw,14px); line-height:1.35; }
    .callout{
      background:rgba(255,255,255,0.94); border-left:6px solid var(--turquesa);
      padding:clamp(10px,1.1vw,14px); border-radius:6px;
    }
    .two-col{ display:grid; grid-template-columns:1.15fr 0.9fr;
              gap:clamp(12px,1.6vw,22px); align-items:flex-start; min-height:0; }
    .equal-col{ display:grid; grid-template-columns:1fr 1fr;
                gap:clamp(12px,1.6vw,22px); align-items:flex-start; min-height:0; }
    @media(max-width:980px){ .two-col,.equal-col{ grid-template-columns:1fr; } }
    .card{ background:rgba(255,255,255,0.96); border:1px solid #dde2f1;
           border-radius:10px; padding:clamp(10px,1.1vw,14px); }
    .kbd{
      font-family:ui-monospace,SFMono-Regular,Menlo,Monaco,Consolas,monospace;
      background:#111827; color:#e5e7eb; padding:2px 8px; border-radius:7px;
      font-size:clamp(11px,1vw,13px); border:1px solid rgba(255,255,255,.08);
    }
    .code{
      font-family:ui-monospace,SFMono-Regular,Menlo,Monaco,Consolas,monospace;
      font-size:clamp(12px,1.05vw,14px); line-height:1.35;
      background:#0b1020; color:#e5e7eb; border-radius:10px;
      padding:clamp(10px,1.1vw,14px); overflow:auto; max-height:44vh;
      border:1px solid rgba(17,24,39,.25); white-space:pre-wrap;
    }
    table{ width:100%; border-collapse:collapse; background:rgba(255,255,255,0.96); }
    th,td{ border:1px solid #dde2f1; padding:clamp(8px,1vw,12px);
           font-size:clamp(14px,1.2vw,18px); color:var(--texto); vertical-align:top; }
    th{ background:#f3f6fb; text-transform:uppercase;
        font-size:clamp(12px,1.1vw,14px); letter-spacing:.06em; color:var(--azul-medio); }
    .footer{ display:flex; justify-content:space-between; align-items:center;
             color:var(--muted); font-size:clamp(12px,1.05vw,14px); gap:14px; }
    .counter{ font-weight:800; color:var(--azul-medio); }
    .cover .counter{ color:#e5edf8; }
    .touch-nav{ position:absolute; top:50%; left:0; right:0;
                transform:translateY(-50%); display:flex;
                justify-content:space-between; padding:0 12px;
                pointer-events:none; z-index:10; }
    .touch-nav button{
      pointer-events:auto; width:44px; height:44px; border-radius:999px; padding:0;
      border:1px solid rgba(221,226,241,0.95); background:rgba(255,255,255,0.92);
      color:var(--texto); font-weight:900; box-shadow:0 10px 22px rgba(0,0,0,.12);
      cursor:pointer;
    }
    #touchNav{ opacity:0; transform:translateY(-50%) translateY(6px); transition:.15s ease; }
    .stage:hover #touchNav{ opacity:1; transform:translateY(-50%) translateY(0); }
    @media(hover:none){ #touchNav{ opacity:1; transform:translateY(-50%); } }
  </style>
</head>

<body>
<div class="deck">
  <div class="stage" id="stage">

    <!-- 1 · PORTADA -->
    <section class="slide cover active">
      <div class="inner">
        <div class="content" style="justify-content:center;">
          <h1>{{TITULO}}</h1>
          <div class="subtitle">{{SUBTITULO}}</div>
          <p style="margin-top:clamp(10px,1.4vw,18px);color:#e5edf8;">
            {{UNIDAD_INFO}}
          </p>
          <p class="note" style="margin-top:clamp(12px,1.6vw,20px);">
            Navegacion: ← → · Home / End · F pantalla completa · Tactil: desliza
          </p>
        </div>
        <div class="footer">
          <span style="color:#e5edf8;">Logica de Programacion · Grupos 810 / 811</span>
          <span class="counter">1 / {{TOTAL}}</span>
        </div>
      </div>
    </section>

    <!-- 2 · PROPOSITO (reemplaza agenda) -->
    <section class="slide content-slide">
      <div class="inner">
        <div class="header">Proposito de la sesion</div>
        <div class="content">
          <h2>{{TITULO_PROPOSITO}}</h2>
          <ul>
            <!-- Listar 3-5 competencias concretas que el estudiante logra al terminar la sesion -->
          </ul>
        </div>
        <div class="footer">
          <span>Sesion {{N}} · Objetivos</span>
          <span class="counter">2 / {{TOTAL}}</span>
        </div>
      </div>
    </section>

    <!-- 3…N · slides de contenido, actividad y cierre -->

  </div><!-- /stage -->

  <div class="touch-nav" id="touchNav">
    <button id="btnPrev" aria-label="Anterior">&#9664;</button>
    <button id="btnNext" aria-label="Siguiente">&#9654;</button>
  </div>
</div><!-- /deck -->

<script>
  (function(){
    const slides   = Array.from(document.querySelectorAll('.slide'));
    const counters = Array.from(document.querySelectorAll('.counter'));
    const stage    = document.getElementById('stage');
    const btnPrev  = document.getElementById('btnPrev');
    const btnNext  = document.getElementById('btnNext');
    let idx = 0;

    function clamp(n,min,max){ return Math.max(min,Math.min(max,n)); }

    function show(i){
      idx = clamp(i,0,slides.length-1);
      slides.forEach((s,k) => s.classList.toggle('active', k===idx));
      const label = (idx+1)+" / "+slides.length;
      counters.forEach(el => el.textContent = label);
      history.replaceState(null,"","#"+(idx+1));
    }

    function next(){ show(idx+1); }
    function prev(){ show(idx-1); }

    const m = (location.hash||"").match(/^#(\d+)$/);
    if(m){ const s=parseInt(m[1],10); if(!isNaN(s)) idx=clamp(s-1,0,slides.length-1); }
    show(idx);

    btnPrev.addEventListener('click',prev);
    btnNext.addEventListener('click',next);

    document.addEventListener('keydown',(e)=>{
      const k=e.key;
      if(k==='f'||k==='F'){
        if(!document.fullscreenElement) document.documentElement.requestFullscreen?.();
        else document.exitFullscreen?.();
        return;
      }
      if(k==='ArrowRight'||k==='PageDown'||k===' '){ e.preventDefault(); next(); }
      else if(k==='ArrowLeft'||k==='PageUp'){         e.preventDefault(); prev(); }
      else if(k==='Home'){                             e.preventDefault(); show(0); }
      else if(k==='End'){                              e.preventDefault(); show(slides.length-1); }
    });

    const isTouch = ('ontouchstart' in window)||(navigator.maxTouchPoints>0);
    if(isTouch){
      let x0=null,y0=null,t0=0;
      const SWIPE_MIN_X=50,SWIPE_MAX_Y=80,SWIPE_MAX_MS=800;
      stage.addEventListener('touchstart',(e)=>{
        const t=e.changedTouches[0]; x0=t.clientX; y0=t.clientY; t0=Date.now();
      },{passive:true});
      stage.addEventListener('touchend',(e)=>{
        if(x0===null||y0===null) return;
        const t=e.changedTouches[0];
        const dx=t.clientX-x0, dy=t.clientY-y0, dt=Date.now()-t0;
        x0=null; y0=null;
        if(dt>SWIPE_MAX_MS||Math.abs(dy)>SWIPE_MAX_Y) return;
        if(dx<=-SWIPE_MIN_X) next();
        else if(dx>=SWIPE_MIN_X) prev();
      },{passive:true});
    }
  })();
</script>
</body>
</html>
```

---

## 5. Arquitectura de slides para 4 horas de clase

Una sesión de 4 horas debe tener **mínimo 16 y máximo 22 slides** estructurados así:

| Bloque | Slides | Tipo de contenido |
|--------|--------|-------------------|
| Portada | 1 | `.slide.cover` |
| Propósito | 1 | Qué logra el estudiante (sin mencionar duración) |
| Recordatorio | 1-2 | Conexión con sesión anterior (¿qué ya saben?) |
| Concepto A | 2-3 | Teoría con ejemplo guiado + diagrama/animación |
| Concepto B | 2-3 | Teoría con ejemplo guiado + código |
| Práctica guiada | 2-3 | Paso a paso resuelto en clase |
| Taller guiado docente | 2-3 | Ejemplo completo paso a paso y evidencia de practica |
| Análisis / errores comunes | 1-2 | Contraejemplos o detección de fallos |
| Cierre | 1 | Síntesis de lo aprendido + qué viene |

**Reglas de contenido por slide**:
- Máximo 6 puntos de lista por slide.
- Si hay código, máximo 20 líneas visibles (usar `max-height` + scroll).
- Cada slide de concepto con **al menos un ejemplo concreto** en C#.
- Todo concepto abstracto debe ir acompañado de un diagrama SVG o animación CSS (ver §7).

---

## 6. Clases de layout disponibles

### `.two-col` — dos columnas (1.15fr / 0.9fr)
```html
<div class="two-col">
  <div class="card"> … izquierda … </div>
  <div class="card"> … derecha … </div>
</div>
```

### `.equal-col` — dos columnas iguales
```html
<div class="equal-col">
  <div class="card"> … </div>
  <div class="card"> … </div>
</div>
```

### `.card` — tarjeta blanca con borde
```html
<div class="card">
  <h2>Título</h2>
  <p>…</p>
</div>
```

### `.callout` — destacado con borde turquesa
```html
<div class="callout">
  <p><strong>Regla clave:</strong> …</p>
</div>
```

### `.code` — bloque de código oscuro
```html
<div class="code">int suma = 0;
for (int i = 0; i &lt; n; i++)
    suma += i;</div>
```

### `.note` — texto secundario gris
```html
<p class="note">Contexto adicional o advertencia menor.</p>
```

### `.kbd` — etiqueta de palabra clave inline
```html
<span class="kbd">static</span>
```

### `table` — tabla institucional
```html
<table>
  <thead><tr><th>Col A</th><th>Col B</th></tr></thead>
  <tbody><tr><td>…</td><td>…</td></tr></tbody>
</table>
```

---

## 7. Diagramas, gráficos y animaciones

### Regla general de imágenes externas
- **NO uses** imágenes de internet de fuente desconocida.
- Si debes usar una imagen externa, verifica que tenga licencia **CC0, CC BY, MIT o dominio público**, cita la fuente en un comentario HTML y en la guía docente.
- **Preferir siempre**: SVG inline, animaciones CSS, Canvas API.

---

### 7.1 Diagrama SVG inline (flujo, jerarquía, comparación)

Ejemplo: jerarquía de funciones (apto para sesión 18+)

```html
<svg viewBox="0 0 420 200" style="width:100%;max-height:38vh;"
     aria-label="Jerarquia de funciones: Main llama a Ingresar, Calcular y Mostrar">
  <!-- Main -->
  <rect x="150" y="10" width="120" height="36" rx="8"
        fill="#015583" stroke="none"/>
  <text x="210" y="33" text-anchor="middle" fill="#fff"
        font-family="Arial" font-size="14" font-weight="bold">Main()</text>
  <!-- Conectores -->
  <line x1="210" y1="46" x2="60"  y2="90" stroke="#05ADBC" stroke-width="2"/>
  <line x1="210" y1="46" x2="210" y2="90" stroke="#05ADBC" stroke-width="2"/>
  <line x1="210" y1="46" x2="360" y2="90" stroke="#05ADBC" stroke-width="2"/>
  <!-- Hijas -->
  <rect x="10"  y="90" width="100" height="36" rx="8" fill="#05ADBC"/>
  <text x="60"  y="113" text-anchor="middle" fill="#fff"
        font-family="Arial" font-size="12">Ingresar()</text>
  <rect x="160" y="90" width="100" height="36" rx="8" fill="#05ADBC"/>
  <text x="210" y="113" text-anchor="middle" fill="#fff"
        font-family="Arial" font-size="12">Calcular()</text>
  <rect x="310" y="90" width="100" height="36" rx="8" fill="#05ADBC"/>
  <text x="360" y="113" text-anchor="middle" fill="#fff"
        font-family="Arial" font-size="12">Mostrar()</text>
  <!-- Nietas -->
  <line x1="210" y1="126" x2="210" y2="156" stroke="#6b7280" stroke-width="1.5" stroke-dasharray="4"/>
  <rect x="160" y="156" width="100" height="30" rx="6" fill="#f3f6fb" stroke="#dde2f1"/>
  <text x="210" y="175" text-anchor="middle" fill="#201F4B"
        font-family="Arial" font-size="11">ObtenerPromedio()</text>
</svg>
```

---

### 7.2 Animación CSS (diagrama de flujo de ejecución paso a paso)

Incluir en el `<style>` del archivo:

```css
/* Animacion: aparicion secuencial de pasos */
@keyframes fadeIn {
  from { opacity:0; transform:translateY(8px); }
  to   { opacity:1; transform:translateY(0); }
}
.step { opacity:0; animation: fadeIn .4s ease forwards; }
.step:nth-child(1){ animation-delay:.3s; }
.step:nth-child(2){ animation-delay:.8s; }
.step:nth-child(3){ animation-delay:1.3s; }
.step:nth-child(4){ animation-delay:1.8s; }
```

Uso en el slide:
```html
<div class="content">
  <div class="step callout">
    <strong>Paso 1:</strong> El usuario ingresa datos (UI).
  </div>
  <div class="step callout">
    <strong>Paso 2:</strong> <span class="kbd">Main()</span> llama a
    <span class="kbd">Calcular()</span>.
  </div>
  <div class="step callout">
    <strong>Paso 3:</strong> <span class="kbd">Calcular()</span> retorna resultado.
  </div>
  <div class="step callout">
    <strong>Paso 4:</strong> <span class="kbd">Main()</span> llama a
    <span class="kbd">Mostrar()</span> con el resultado.
  </div>
</div>
```

---

### 7.3 Diagrama de pila de llamadas (animado con JS)

Para explicar alcance de variables y call stack (sesión 19+), usar un
`<canvas>` o un div-stack animado con JS puro. Ejemplo de stack visual:

```html
<div id="callstack" style="display:flex;flex-direction:column-reverse;
     gap:6px;max-width:260px;margin:0 auto;"></div>
<button onclick="pushFrame()" class="kbd"
        style="cursor:pointer;padding:6px 12px;margin-top:8px;">
  Llamar siguiente función ▶
</button>
<script>
  const frames = [
    { name:"Main()",                bg:"#015583" },
    { name:"ObtenerDatos()",        bg:"#05ADBC" },
    { name:"ValidarRango(double x)",bg:"#2563eb" },
  ];
  let fi = 0;
  const stack = document.getElementById('callstack');
  function pushFrame(){
    if(fi >= frames.length) return;
    const f = frames[fi++];
    const el = document.createElement('div');
    el.textContent = f.name;
    el.style.cssText =
      `background:${f.bg};color:#fff;padding:10px 16px;border-radius:8px;
       font-family:monospace;font-size:14px;text-align:center;
       animation:fadeIn .3s ease;`;
    stack.appendChild(el);
  }
  pushFrame(); // mostrar Main al cargar
</script>
```

---

### 7.4 Gráficos de datos (Chart.js — MIT License)

Solo para sesiones que requieran visualización de datos. Usar CDN oficial:

```html
<!-- Chart.js v4 · Licencia MIT · https://www.chartjs.org -->
<script src="https://cdn.jsdelivr.net/npm/chart.js@4/dist/chart.umd.min.js"></script>
```

Ejemplo de uso (barra comparativa):
```html
<canvas id="grafico" style="max-height:36vh;"></canvas>
<script>
  new Chart(document.getElementById('grafico'), {
    type: 'bar',
    data: {
      labels: ['Array', 'List<T>'],
      datasets:[{
        label:'Operaciones posibles',
        data:[3,6],
        backgroundColor:['#015583','#05ADBC']
      }]
    },
    options:{ plugins:{ legend:{display:false} }, scales:{ y:{ beginAtZero:true } } }
  });
</script>
```

---

### 7.5 Diagrama de flujo con Mermaid.js (MIT License)

Para diagramas de flujo de algoritmos. Cargar desde CDN:

```html
<!-- Mermaid.js · Licencia MIT · https://mermaid.js.org -->
<script src="https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.min.js"></script>
<script>mermaid.initialize({startOnLoad:true,theme:'default'});</script>
```

```html
<div class="mermaid" style="max-height:40vh;overflow:auto;">
flowchart TD
    A([Inicio]) --> B[Leer numero]
    B --> C{es divisible entre 2?}
    C -- Si --> D[Imprimir par]
    C -- No --> E[Imprimir impar]
    D --> F([Fin])
    E --> F
</div>
```

---

## 8. Slides de taller guiado (NO evaluativo)

Cada sesion debe incluir taller guiado para desarrollo en clase. Estructura sugerida:

### Slide de enunciado tecnico (slide N-2)
```html
<section class="slide content-slide">
  <div class="inner">
    <div class="header">Taller guiado</div>
    <div class="content">
      <h2>{{TITULO_TALLER}}</h2>
      <div class="callout">
        <p><strong>Reto tecnico:</strong> {{DESCRIPCION_CONCRETA}}</p>
      </div>
      <ul>
        <li>Paso 1: …</li>
        <li>Paso 2: …</li>
        <li>Paso 3: …</li>
      </ul>
      <p class="note">Evidencia: archivo <span class="kbd">.cs</span>
        en repositorio del estudiante, carpeta
        <span class="kbd">sesion{{N}}/</span>.</p>
    </div>
    <div class="footer">
      <span>Taller sesion {{N}}</span>
      <span class="counter">{{SLIDE_N}} / {{TOTAL}}</span>
    </div>
  </div>
</section>
```

### Slide de ejemplo completo para desarrollo del profesor (slide N-1)
```html
<section class="slide content-slide">
  <div class="inner">
    <div class="header">Desarrollo guiado del profesor</div>
    <div class="content">
      <div class="code">// Pegar codigo C# completo
// que el docente desarrolla en vivo
{{CODIGO_COMPLETO_DOCENTE}}</div>
      <p class="note">Recomendacion: dividir explicacion en bloques cortos
      (entrada, proceso, salida) con compilacion intermedia.</p>
    </div>
    <div class="footer">
      <span>Desarrollo en clase sesion {{N}}</span>
      <span class="counter">{{SLIDE_N}} / {{TOTAL}}</span>
    </div>
  </div>
</section>
```

---

## 9. Slide de cierre

El último slide siempre es un cierre. NO incluir duración. Estructura:

```html
<section class="slide content-slide">
  <div class="inner">
    <div class="header">Cierre de sesion</div>
    <div class="content">
      <h2>{{TITULO_CIERRE}}</h2>
      <div class="equal-col">
        <div class="card">
          <h2>Lo que construimos hoy</h2>
          <ul>
            <li>…</li>
            <li>…</li>
          </ul>
        </div>
        <div class="card">
          <h2>Que viene en la siguiente sesion</h2>
          <ul>
            <li>…</li>
          </ul>
        </div>
      </div>
      <p class="note" style="margin-top:12px;">
        {{NOTA_CIERRE}}
      </p>
    </div>
    <div class="footer">
      <span>Sesion {{N}} · Cierre</span>
      <span class="counter">{{TOTAL}} / {{TOTAL}}</span>
    </div>
  </div>
</section>
```

---

## 10. Plantilla de guía docente (guia_sesionN_0.md)

Crear el archivo `sesiones/guia_sesionN_0.md` con la siguiente estructura:

```markdown
# Guía docente — Sesión N: {{TITULO}}

**Curso:** Lógica de Programación  
**Grupos:** 810 / 811  
**Saberes de unidad abordados:** {{LISTA_SABERES}}

---

## Objetivo de la sesión

{{OBJETIVO_MEDIBLE_SIN_DURACION}}

---

## Slide a slide — orientaciones

### Slide 1 · Portada
- Esperar a que todos los estudiantes estén conectados/presentes antes de avanzar.

### Slide 2 · Propósito
- Leer en voz alta cada punto con los estudiantes.
- Preguntar: "¿Alguien ha escuchado el término …?"

### Slide 3 · {{TEMA_SLIDE3}}
**Pregunta de activación:** {{PREGUNTA_ABIERTA}}  
**Respuesta esperada:** {{RESPUESTA_ESPERADA}}

<!-- Repetir para cada slide relevante -->

---

## Taller guiado en clase — respuestas esperadas

### Enunciado
{{ENUNCIADO_ACTIVIDAD}}

### Solución de referencia (C#)

```csharp
// {{COMENTARIO_EXPLICATIVO}}
{{CODIGO_SOLUCION_COMPLETO}}
```

### Casos de prueba

| Entrada | Salida esperada |
|---------|-----------------|
| …       | …               |
| …       | …               |

### Errores frecuentes a anticipar

| Error típico | Corrección |
|-------------|------------|
| …            | …          |

---

## Formato de entrega esperado por el estudiante

**Plataforma:** GitHub (repositorio personal del estudiante)  
**Ruta:** `sesionN/` en la rama `main`

### Estructura mínima del repositorio del estudiante

```
sesionN/
  ├── Programa.cs          ← código fuente principal
  ├── sesionN.csproj       ← (si aplica, proyecto .NET)
  └── README.md            ← explicación breve del ejercicio
```

### Ejemplo de README.md esperado

```markdown
# Sesión N — {{TITULO_BREVE}}

## ¿Qué hace el programa?
{{DESCRIPCION_BREVE_DEL_ESTUDIANTE}}

## Casos probados
| Entrada | Salida obtenida |
|---------|-----------------|
| …       | …               |
```

### Lista de verificación antes de revisar

- [ ] El repositorio es público o el estudiante compartió acceso.
- [ ] Existe la carpeta `sesionN/`.
- [ ] El archivo `Programa.cs` compila con `dotnet run`.
- [ ] El README describe qué hace el programa.
- [ ] Los casos de prueba coinciden con los esperados.

---

## Recursos de apoyo para el docente

- Documentación oficial C# / .NET: <https://learn.microsoft.com/es-es/dotnet/csharp/>
- Extensión C# para VS Code: ms-dotnettools.csharp
- Repositorio del curso (material de profesor): `.material/`
```

---

## 11. Actualizar index.html

Después de crear la sesión, añadir la tarjeta al grid en `index.html`:

```html
<div class="card">
    <h3>Sesión {{N}}.0</h3>
    <a href="sesiones/sesion{{N}}_0.html">Abrir sesión</a>
</div>
```

Insertar en orden numérico dentro de `<div class="grid">`.

---

## 12. Checklist final antes de entregar

- [ ] `sesiones/sesion{{N}}_0.html` creado con mínimo 16 slides.
- [ ] Slide 1 es portada con `.slide.cover`.
- [ ] Slide 2 es "Propósito" (sin agenda, sin duración).
- [ ] Al menos un diagrama SVG inline o animación CSS.
- [ ] Al menos un bloque `.code` con ejemplo C# funcional.
- [ ] Taller guiado con ejemplo completo para desarrollo del profesor.
- [ ] Slide de cierre con lo aprendido y lo que sigue.
- [ ] Contadores `N / {{TOTAL}}` actualizados en cada footer.
- [ ] JS de navegación completo e intacto (btnPrev, btnNext, teclado, touch).
- [ ] `sesiones/guia_sesion{{N}}_0.md` creado con solución de referencia.
- [ ] Tarjeta añadida en `index.html`.
- [ ] No hay imágenes externas sin licencia libre verificada.
- [ ] Sin menciones de duración en ningún slide.
