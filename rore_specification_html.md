# RORE MARKUP LANGUAGE — TECHNICAL SPECIFICATION v2.0
## (HTML5 → RORE Mapping)
## Full Syntax, Rules, Structures & Element Reference

---

# 1. INTRODUCTION
Rore — bu HTML5 strukturasini soddalashtirilgan, qatʼiy sintaksisga ega DSL formatiga o‘tkazish uchun yaratilgan markup tilidir. U HTML5 ning barcha semantik, strukturaviy, atributiv imkoniyatlarini to‘liq saqlagan holda, minimal va qatʼiy belgilangan sintaksis orqali ishlab chiqiladi.

Ushbu hujjat **HTML5 ning deyarli barcha teg va atributlarini** Rore formatiga moslab rasmiy tarzda qayta hujjatlashtiradi.

---

# 2. CORE SYNTAX RULES

## 2.1 TEXT LITERALS — `{ ... }`
- `{ ... }` — faqat matn literalidir.
- Ichidagi barcha narsa o‘zgarmas va matn sifatida DOMga uzatiladi.

**Misollar:**
```
{Hello}:h1
{Submit}:button
```

---

## 2.2 FUNCTION / ELEMENT ARGUMENTS — `()` va `("...")`

### 2.2.1 `()` — mantiqiy bo‘sh funksiyalar
- `()` ichida **qo‘shtirnoq yo‘q** bo‘lsa — bu bo‘sh/mantiqiy funksiya.
- Odatda bo‘sh elementlar uchun ishlatiladi.

**Misollar:**
```
():img
():meta
():br
```

### 2.2.2 `("...")` — qiymatga ega atribut/argument
- Agar qavs ichida **qo‘shtirnoq** bo‘lsa, bu qiymat, o‘zgaruvchi, yoki atribut qiymati hisoblanadi.

**Misollar:**
```
::href("/home")
::alt("Logo")
::lang("uz")
```

### 2.2.3 **RAQAM ARGUMENTLAR (YANGI QOIDA)**
Agar argument **faqat raqam** bo‘lsa:
- qo‘shtirnoq ishlatish shart emas.
- Piksellar avtomatik qo‘llanadi (width, height, font-size kabi funksiyalarda).

**Misollar:**
```
::width(500)        → width="500px"
::height(240)       → height="240px"
::max(10)           → max="10"
::value(3)          → value="3"
```

**Eʼtibor:** raqam + birlik ("500px") yozilsa — qo‘lbola qiymat sifatida qabul qilinadi.

---

# 3. ELEMENT DECLARATION RULES

## 3.1 ELEMENTNING ASOSIY SHAKLI
Har bir Rore elementi quyidagicha tuziladi:
```
{Text}:tag
():tag
():tag::attr(...)
{Text}:tag::attr(...)
```

---

## 3.2 NESTING — `->` CHILDREN OPERATORI
`->` operatori HTML dagi nesting bilan bir xil, lekin qatʼiy qoidaga ega.

### QOIDA:
1. `Parent ->` yoziladi.
2. Keyingi qatordagi har bir satr **children** hisoblanadi.
3. Agar **bitta bo‘sh qator** kelsa — children tugaydi.
4. Undan keyingi kod yangi parent sifatida boshlanadi.

**Misol:**
```
{Header}:header ->
    {Logo}:img::src("logo.png")
    {Nav}:nav ->
        {Home}:a::href("/")
        {Blog}:a::href("/blog")

{Footer}:footer
```

---

# 4. ATTRIBUTE SYNTAX — `::attr(...)`
HTML5 dagi barcha atributlar quyidagi shaklda yoziladi:
```
::name(value)
::name("value")
```

**Raqamli qiymatlar** avtomatik formatlanadi:
```
::width(500) → width="500px"
```

**Custom attribute:**
```
::attr("data-id", "550")
```

---

# 5. GLOBAL ATTRIBUTES
Barcha HTML teglarda mavjud bo‘lgan atributlar.

| HTML Atribut | Rore | Izoh |
|--------------|------|------|
| id | #id | shorthand |
| class | .className | shorthand |
| title | ::title("...") | tooltip |
| lang | ::lang("uz") | til |
| dir | ::dir("rtl") | yo‘nalish |
| hidden | ::hidden() | boolean |
| tabindex | ::tabindex(0) | |
| contenteditable | ::contenteditable(true) | |
| draggable | ::draggable(true) | |
| spellcheck | ::spellcheck(true) | |
| data-* | ::data-name("value") | |
| aria-* | ::aria-label("...") | accessibility |

**Misol:**
```
{Banner}:div.hero#main::lang("uz")::aria-label("Homepage banner")
```

---

# 6. HEAD & META TAGS

## 6.1 `<title>`
```
{My Site}:title
```

## 6.2 `<meta>`
```
():meta::charset("utf-8")
():meta::name("viewport")::content("width=device-width, initial-scale=1")
```

## 6.3 `<link>`
```
():link::rel("stylesheet")::href("/style.css")
():link::rel("preload")::href("main.wasm")::as("script")
():link::rel("icon")::href("/favicon.ico")
```

## 6.4 `<base>`
```
():base::href("/")
```

---

# 7. SEMANTIC STRUCTURE TAGS

## 7.1 `<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<aside>`, `<footer>`
```
{Site Title}:header
{Main}:main
{Section}:section
```

Misol nested:
```
{Layout}:main ->
    {Top}:header ->
        {Logo}:img::src("logo.png")
    {Content}:section
```

---

# 8. TEXT & INLINE ELEMENTS

| HTML | Rore |
|------|-------|
| a | {Text}:a::href("...") |
| strong | {Text}:strong |
| em | {Text}:em |
| small | {Text}:small |
| code | {Code}:code |
| span | {Text}:span |
| br | ():br |
| wbr | ():wbr |
| mark | {Highlight}:mark |
| time | {12:00}:time::datetime("12:00") |

Misol:
```
{Click here}:a::href("/")::target("_blank")
```

---

# 9. LISTS

## `<ul>`, `<ol>`, `<li>`
```
{Menu}:ul ->
    {Home}:li
    {Profile}:li
```

## `<dl>`, `<dt>`, `<dd>`
```
():dl ->
    {CPU}:dt
    {16-core}:dd
```

---

# 10. FORMS & INPUTS
To‘liq HTML5 form kontrollerlari qo‘llab-quvvatlanadi.

## 10.1 `<form>`
```
{Login}:form::action("/login")::method("post")
```

## 10.2 `<input>` + barcha turlari
```
():input::type("text")::name("username")
():input::type("email")::required()
():input::type("file")::multiple()::accept("image/*")
():input::type("checkbox")::checked()
```

### Numeric attributes:
```
::min(0)
::max(100)
::step(1)
```

## 10.3 `<textarea>`
```
{Message}:textarea::rows(5)::cols(40)
```

## 10.4 `<select>`
```
{Choose City}:select::name("city") ->
    {Tokyo}:option::value("tokyo")
    {Dubai}:option::value("dubai")
```

## 10.5 `<label>`
```
{Name}:label::for("idname")
```

---

# 11. MEDIA ELEMENTS

## 11.1 `<img>`
```
():img::src("a.jpg")::alt("Photo")::width(400)
```

## 11.2 `<picture>` + `<source>`
```
():picture ->
    ():source::srcset("x-small.jpg")::media("(max-width: 600px)")
    ():img::src("x.jpg")::alt("Image")
```

## 11.3 `<video>`
```
{Not supported}:video::controls()::width(640)::height(360) ->
    ():source::src("v.mp4")::type("video/mp4")
```

## 11.4 `<audio>`
```
{Not supported}:audio::controls() ->
    ():source::src("a.mp3")::type("audio/mpeg")
```

## 11.5 `<track>`
```
():track::kind("subtitles")::src("en.vtt")::srclang("en")
```

---

# 12. TABLE ELEMENTS
```
{Products}:table ->
    ():thead ->
        ():tr ->
            {Name}:th
            {Price}:th
    ():tbody ->
        ():tr ->
            {Apple}:td
            {5000}:td
```

---

# 13. EMBEDDED CONTENT

## 13.1 `<iframe>`
```
():iframe::src("https://example.com")::width(800)::height(600)::sandbox("allow-scripts")
```

## 13.2 `<object>` + `<param>`
```
{Fallback}:object::data("x.svg")::type("image/svg+xml") ->
    {Browser not supported}:p
():param::name("auto")::value("yes")
```

## 13.3 `<embed>`
```
():embed::src("file.pdf")::type("application/pdf")
```

---

# 14. INTERACTIVE ELEMENTS

## 14.1 `<details>` + `<summary>`
```
():details ->
    {More}:summary
    {Hidden text}:p
```

## 14.2 `<dialog>`
```
{Hello}:dialog::open()
```

---

# 15. ACCESSIBILITY (ARIA)
To‘liq ARIA qo‘llab-quvvatlanadi.

**Misol:**
```
{Menu}:nav::role("navigation")::aria-label("Main menu")
```

Rore funksiyalari:
```
::aria-label("...")
::aria-hidden(true)
::aria-expanded(false)
::aria-controls("id")
```

---

# 16. SVG & CANVAS

## 16.1 `<canvas>`
```
():canvas::id("stage")::width(800)::height(600)
```

## 16.2 `<svg>` inline qo‘llab-quvvatlanadi
```
{SVG content}:svg
```

---

# 17. SECURITY ATTRIBUTES

| HTML | Rore |
|------|-------|
| rel="noopener noreferrer" | ::rel("noopener noreferrer") |
| sandbox | ::sandbox("allow-scripts") |
| crossorigin | ::crossorigin("anonymous") |
| integrity | ::integrity("sha384-...") |

---

# 18. MICRODATA / RDFa
```
{Item}:div::itemtype("http://schema.org/Product")::itemprop("name")
```

---

# 19. QUICK REFERENCE

### ELEMENT
```
{Text}:tag
():tag
```

### ATTRIBUTES
```
::attr(value)
::width(300)
::href("/home")
```

### CHILDREN
```
Parent ->
    Child1
    Child2
```

### MEDIA
```
():img::src("a.jpg")::width(400)
```

### FORM
```
():input::type("text")
```

---

# END OF SPECIFICATION

Bu hujjat Rore DSL ning to‘liq, formal va minimal sintaksisga ega bo‘lgan versiyasi hisoblanadi. Kerak bo‘lsa men sizga:
- EBNF grammatikasi
- Parser dizayni
- HTML transpiler
- VSCode extension
- Test suite
ham yozib bera olaman.

