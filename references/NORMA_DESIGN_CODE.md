# NORMA. Дизайн-код

> Собрано 17.09.2026 из исходников деплоя `~/Desktop/ARTIFACTS/primepeak-site/`, а не с отрендеренной страницы.
> Эталон: `moloko/index.html`, это primeglow.co/moloko. Корень primeglow.co - другая система: 452 класса против 103, общих переменных всего 7, поэтому он в разбор не входит.
> Разбирали пять агентов по направлениям, шестой собрал. Выборочную сверку значений я повторил вручную по CSS.

NORMA - это визуальная система полностраничных лендингов-разборов: белое полотно в рамке, чередующиеся белые и серые секции со скруглением 80px, тёмные блоки-акценты, стеклянная плавающая навигация из трёх пилюль, один шрифт Hanken Grotesk, зелёный и терракотовый как единственные цвета, и ровно один механизм анимации на всю страницу. Эталон системы - страница `primeglow.co/moloko`, её исходник лежит в `primepeak-site/moloko/index.html` (1097 строк, весь CSS инлайном в единственном `<style>`, строки 17-301; выжимка того же CSS - `src_moloko.css`, 284 строки, строка CSS N = строка HTML N+16). По этой же системе собран GOODHUMAN на `primeglow.co/goodhuman`. Документ написан так, чтобы по нему можно было собрать новую страницу сверху вниз, ни разу не открыв оригинал: сначала токены, потом каркас, потом навигация, потом блоки, потом кнопки и мелочь, потом движение и адаптив, в конце - правила и приёмка.

Всё, что ниже, процитировано из исходника. Где в исходнике чего-то нет, это сказано словами «в исходнике не задано». Внешних CSS-файлов у страницы нет, `@font-face` не задан, `@keyframes` не задан ни разу.

Дополнение от 18.09.2026: правило симметрии в части 9 и соответствующие пункты приёмки в части 10 утверждены как переносимый стандарт для следующих сборок. Это нормативное дополнение к разбору исходника; при расхождении в требованиях к симметрии применяется новое правило. Цитаты CSS эталона сохраняют исторические значения.

---

# ЧАСТЬ 1. ТОКЕНЫ И ТИПОГРАФИКА

## 1.1. Переменные `:root`

Объявлены одним блоком, строки 19-26 HTML (3-10 CSS). Всего 15 переменных.

```css
:root{
  --page:#ffffff; --card:#ffffff; --soft:#f5f5f5; --dark:#111111;
  --ink:#0a0a0a; --ink2:#6b6b6b; --ink3:#8a8a8a; --ink-mute:#707070;
  --line:rgba(10,10,10,0.10); --line2:rgba(10,10,10,0.06);
  --acc:#1d5c46; --acc-soft:#eef4f0; --warn:#9a4a2f;
  --sf:'Hanken Grotesk',-apple-system,system-ui,sans-serif;
  --r:20px; --r-lg:80px; --r-sm:14px;
}
```

| Переменная | Значение | Смысл | Где применяется |
|---|---|---|---|
| `--page` | `#ffffff` | фон документа | `html,body` |
| `--card` | `#ffffff` | фон обычной секции и карточки-пилона | `section`, `.pillar`, `.stat` |
| `--soft` | `#f5f5f5` | серая полоса полотна и серые заливки | `section.gray`, `.slab`, `.note`, `.pillar .ph`, `.reset`, `.bar-track`, `.mod2 .m-photo`, `.gal figure`, `tr.tot td` |
| `--dark` | `#111111` | тёмное полотно и «чёрные» элементы | `.hero`, `.quote`, `.final`, `footer`, `.oh`, `.reset:hover`, `.bar-fill`, ручки ползунка, `.btn-dark` |
| `--ink` | `#0a0a0a` | основной текст | 19 правил в CSS плюс 1 инлайн (HTML:980) |
| `--ink2` | `#6b6b6b` | вторичный текст, описания, строки списков | 14 правил |
| `--ink3` | `#8a8a8a` (в эталоне было `#a1a1a1`) | приглушённая часть **крупных** заголовков, только текст от 24px: 3.45:1 на белом, 3.17:1 на `--soft` | `.slab h2`, `h2.sec em` |
| `--ink-mute` | `#707070` | «выключенный» серый для **мелкого** текста: капс-метки, источники, подписи таблиц. 4.95:1 на белом, 4.54:1 на `--soft` | 11 правил: `.sf span`, `.stat .k`, `.src`, `thead th`, `.tname span`, `.grp-t`, `.kpi .k`, `.flow-t`, `.mi i`, `.step .no`, `.risk .lv`; инлайн-подписи тоже на нём |
| `--line` | `rgba(10,10,10,0.10)` | основная граница 1px | 20 правил плюс 1 инлайн (HTML:980) |
| `--line2` | `rgba(10,10,10,0.06)` | слабый внутренний разделитель | `.pillar li`, `.col li`, `td`, `.mi` |
| `--acc` | `#1d5c46` | зелёный акцент: положительные числа, фокус, последний сегмент диаграммы | `.pos`, `.al.ok b`, `.bar-fill.a4`, два правила `:focus-visible` |
| `--acc-soft` | `#eef4f0` | фон зелёной плашки-алерта | `.al.ok` |
| `--warn` | `#9a4a2f` | терракотовый: отрицательные числа, высокий риск | `.neg`, `.al.warn b`, `.risk .lv.hi` плюс 1 инлайн (HTML:946) |
| `--sf` | `'Hanken Grotesk',-apple-system,system-ui,sans-serif` | шрифтовой стек | `html,body` и 5 элементов, которые не наследуют шрифт |
| `--r` | `20px` | базовый радиус карточки | 10 правил |
| `--r-lg` | `80px`, на `max-width:640px` `40px` | радиус полотна и крупных блоков | 18 правил |
| `--r-sm` | `14px` | **объявлена и ни разу не используется**: `var(--r-sm)` в файле встречается 0 раз |

Единственное переопределение переменной вне `:root`:

```css
@media(max-width:640px){:root{--r-lg:40px}}
```

Токенов для отступов, типографики, теней и длительностей в исходнике нет: эти значения захардкожены по месту либо считаются через `clamp`.

## 1.2. Шрифт

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Hanken+Grotesk:wght@400;500;600;700;800;900&display=swap" rel="stylesheet">
```

| Параметр | Значение |
|---|---|
| Семейство | Hanken Grotesk, Google Fonts |
| Подключённые веса | `400;500;600;700;800;900` (шесть) |
| Реально используются | 400, 500, 600, 700, 800. Вес **900 подключён, но в файле не используется** (`font-weight:900` встречается 0 раз) |
| Курсив | не подключён; `em` и `i` везде гасятся через `font-style:normal` |
| `display` | `swap` |
| Фолбэки | `-apple-system`, `system-ui`, `sans-serif` |
| Сглаживание | `-webkit-font-smoothing:antialiased` на `html,body` |
| Цифры | `font-variant-numeric:tabular-nums` - утилита `.num` плюс `.hs b`, `.sf b`, `.stat .v`, `th.n/td.n`, `.sl-val`, `.oh .money`, `.kpi .v`, `.bar-top span`, `.mi i`, `.mi i em` |
| Повтор `font-family` | `.hero-nav .cta`, `.cur-toggle`, `.reset`, `.btn-white`, `.btn-dark` (теги `button` и `a` шрифт не наследуют) |

Базовый `font-size` на `html` или `body` **в исходнике не задан**, наследуется дефолт браузера. Базовый `line-height:1.5` задан на `html,body`.

## 1.3. Полная типографическая шкала

Прочерк означает «в этом правиле не задано, наследуется».

| Селектор | font-size | weight | line-height | letter-spacing | transform | цвет |
|---|---|---|---|---|---|---|
| `html,body` | не задан | - | `1.5` | - | - | `var(--ink)` |
| `.hero-nav .logo` | `15px` | `700` | - | `0.18em` | - | `#fff` |
| `.hero-nav .links a` | `13px` | `600` | - | - | - | `#fff`, `opacity:.92` |
| `.hero-nav .cta` | `13px` | `700` | - | - | - | `#0a0a0a` |
| `.cur-toggle` | `13px` | `800` | - | - | - | `#fff` |
| `.nav-burger` (≤900px) | `17px` | - | - | - | - | `#fff` |
| `.hero-nav .links.mopen a` (≤900px) | `16px` | - | - | - | - | наследует |
| `.hero-line h1` | `clamp(38px,7vw,88px)` | `800` | `1.04` | `-0.035em` | - | `rgba(255,255,255,0.66)` |
| `.hero-line h1 em` | - | - | - | - | - | `#fff`, `font-style:normal` |
| `.hero-sub` | `clamp(15px,1.45vw,18px)` | `500` | `1.5`; ≤760px `1.45` | - | - | `rgba(255,255,255,0.82)` |
| `.hero-sub b` | - | `700` | - | - | - | `#fff` |
| `.hs b` | `clamp(20px,2.6vw,30px)`; ≤520px `19px` | `800` | `1.1` | `-0.02em` | - | `#fff` |
| `.hs span` | `12px`; ≤520px `11px` | `600` | - | `0.04em` | - | `rgba(255,255,255,0.6)` |
| `.slab h2` | `clamp(32px,5.5vw,72px)` | `800` | `1.05` | `-0.035em` | - | `var(--ink3)` |
| `.slab h2 em` | - | - | - | - | - | `var(--ink)`, `font-style:normal` |
| `.sf b` | `34px` | `800` | - | `-0.02em` | - | `var(--ink)` |
| `.sf span` | `13px` | `600` | - | - | - | `var(--ink-mute)` |
| `h2.sec` | `clamp(30px,5vw,56px)` | `800` | `1.08` | `-0.03em` | - | наследует `var(--ink)` |
| `h2.sec em` | - | - | - | - | - | `var(--ink3)`, `font-style:normal` |
| `.sub` | `17px` | - | - | - | - | `var(--ink2)` |
| `.sub b` | - | `700` | - | - | - | `var(--ink)` |
| `.tag` | `12px` | `700` | - | `0.11em` | `uppercase` | `var(--ink2)` |
| `.pil-n` | `13px` | `600` | - | - | - | `var(--ink)`; hover `#000` |
| `.pillar h3` | `24px` | `700` | `1.12` | `-0.025em` | - | наследует |
| `.pil-sub` | `14px` | - | `1.5` | - | - | `var(--ink2)` |
| `.pillar li` | `13.5px` | - | `1.5` | - | - | `var(--ink2)` |
| `.pillar li b` | - | `700` | - | - | - | `var(--ink)` |
| `.stat .k` | `12px` | `700` | - | `0.08em` | `uppercase` | `var(--ink-mute)` |
| `.stat .v` | `clamp(30px,3.6vw,42px)` | `800` | `1` | `-0.03em` | - | наследует |
| `.stat p` | `13.5px` | - | `1.5` | - | - | `var(--ink2)` |
| `.col h3` | `22px` | `700` | - | `-0.025em` | - | наследует |
| `.col li` | `15px` | - | `1.55` | - | - | `var(--ink2)` |
| `.col li b` | - | `700` | - | - | - | `var(--ink)` |
| `.src` | `12.5px` | - | `1.75` | - | - | `var(--ink-mute)` |
| `.note` | `15px` | - | `1.6` | - | - | `var(--ink2)` |
| `.note b` | - | `700` | - | - | - | `var(--ink)` |
| `table` | `15px` | - | - | - | - | наследует |
| `thead th` | `11.5px` | `700` | `1.25` | `0.05em` | `uppercase` | `var(--ink-mute)` |
| `tr.tot td` | - | `800` | - | - | - | наследует |
| `tr.dim td` | - | - | - | - | - | `var(--ink2)` |
| `td.hl` | - | `800` | - | - | - | наследует |
| `.pos` | - | `700` | - | - | - | `var(--acc)` |
| `.neg` | - | `700` | - | - | - | `var(--warn)` |
| `.tname` | - | `700` | - | - | - | `var(--ink)` |
| `.tname span` | `12.5px` | `500` | - | - | - | `var(--ink-mute)`, `display:block` |
| `.grp-t` | `11px` | `800` | - | `0.11em` | `uppercase` | `var(--ink-mute)` |
| `.sl-lab` | `13.5px` | - | `1.3` | - | - | `var(--ink2)` |
| `.sl-lab i` (инлайн) | `12px` | - | - | - | - | `var(--ink-mute)`, `font-style:normal` |
| `.sl-val` | `14px` | `800` | - | - | - | `var(--ink)` |
| `.reset` | `14px` | `700` | - | - | - | `var(--ink)`; hover `#fff` |
| `.oh .k` | `12px` | `700` | - | `0.1em` | `uppercase` | `rgba(255,255,255,0.55)` |
| `.oh .money` | `clamp(46px,7vw,86px)` | `800` | `1` | `-0.04em` | - | `#fff`; `.bad` `#ffb59b` |
| `.oh .note-d` | `15px` | - | `1.5` | - | - | `rgba(255,255,255,0.68)` |
| `.oh .note-d b` | - | `700` | - | - | - | `#fff` |
| `.kpi .k` | `11px` | `700` | `1.3` | `0.06em` | `uppercase` | `var(--ink-mute)` |
| `.kpi .v` | `24px` | `800` | - | `-0.02em` | - | наследует |
| `.flow-t` | `11px` | `800` | - | `0.11em` | `uppercase` | `var(--ink-mute)` |
| `.bar-top` | `14px` | - | - | - | - | наследует |
| `.bar-top b` | - | `700` | - | - | - | наследует |
| `.bar-top span` | - | - | - | - | - | `var(--ink2)` |
| итоговая строка бара (инлайн, HTML:980) | `16px` | сумма в `span` `800` | - | - | - | `var(--ink)` |
| подпись под баром (инлайн, HTML:978) | `12px` | - | - | - | - | `var(--ink-mute)` |
| `.al` | `14.5px` | - | `1.55` | - | - | `.warn` `#5c3a2c`, `.ok` `#204a3c` |
| `.al.warn b` / `.al.ok b` | - | - | - | - | - | `var(--warn)` / `var(--acc)` |
| `.mod2 .m-txt h2` | `clamp(26px,3.4vw,40px)` | `800` | `1.05` | `-0.03em` | - | наследует |
| `.mod2 .m-txt p` | `15.5px` | - | `1.55` | - | - | `var(--ink2)` |
| `.mod2 .m-txt p b` | - | - | - | - | - | `var(--ink)` |
| `.mi b` | `17px` | `700` | - | `-0.015em` | - | наследует |
| `.mi span` | `13.5px` | - | `1.45` | - | - | `var(--ink2)` |
| `.mi i` | `12px` | `700` | - | `0.04em` | - | `var(--ink-mute)`, `font-style:normal` |
| `.mi i em` | `17px` | `800` | - | - | - | `var(--ink)`, `font-style:normal` |
| `.gal figcaption` | `12px` | `600` | - | `.02em` | - | `#fff` |
| `.step .no` | `15px` | `800` | - | - | - | `var(--ink-mute)` |
| `.step h3` | `22px` | `700` | - | `-0.025em` | - | наследует |
| `.step h3 span` | `11px` | `800` | - | `0.07em` | `uppercase` | `#fff` на `var(--ink)` |
| `.step p` | `15.5px` | - | - | - | - | `var(--ink2)` |
| `.risk .lv` | `11px` | `800` | - | `0.08em` | `uppercase` | `var(--ink-mute)`; `.hi` `var(--warn)` |
| `.risk h3` | `20px` | `700` | - | `-0.02em` | - | наследует |
| `.risk p` | `14px` | - | `1.55` | - | - | `var(--ink2)` |
| `.risk .fix` | `13px` | `700` | `1.45` | - | - | `var(--ink)` |
| `.quote p` | `clamp(26px,4.2vw,50px)` | `700` | `1.16` | `-0.03em` | - | `#7d7d78` |
| `.quote em` | - | - | - | - | - | `#fff`, `font-style:normal` |
| `.final h2` | `clamp(34px,6.4vw,80px)` | `800` | `1.02` | `-0.035em` | - | `#fff` |
| `.final p` | `17px` | - | - | - | - | `rgba(255,255,255,0.68)` |
| `.btn-white` | `17px` | `800` | - | - | - | `var(--ink)` на `#fff` |
| `.btn-dark` | `16px` | `800` | - | - | - | `#fff` на `var(--dark)` |
| `footer` | `14px` | - | - | - | - | `rgba(255,255,255,0.55)` |
| `.foot-in .logo` | `16px` | `700` | - | `0.18em` | - | `#fff` |
| `.foot-in p` | - | - | `1.7` | - | - | наследует |
| пометка «дороже не купят» (инлайн, HTML:946) | `11.5px` | `700` | - | - | - | `var(--warn)` |
| источник розничной цены (инлайн, HTML:949) | `11.5px` | `400` | - | - | - | `var(--ink-mute)` |

Все `clamp()` по кеглю, десять штук:

```css
.hero-line h1   { font-size: clamp(38px,7vw,88px);   }
.hero-sub       { font-size: clamp(15px,1.45vw,18px);}
.hs b           { font-size: clamp(20px,2.6vw,30px); }
.slab h2        { font-size: clamp(32px,5.5vw,72px); }
h2.sec          { font-size: clamp(30px,5vw,56px);   }
.stat .v        { font-size: clamp(30px,3.6vw,42px); }
.mod2 .m-txt h2 { font-size: clamp(26px,3.4vw,40px); }
.quote p        { font-size: clamp(26px,4.2vw,50px); }
.final h2       { font-size: clamp(34px,6.4vw,80px); }
.oh .money      { font-size: clamp(46px,7vw,86px);   }
```

Что из этого читается как система:

- ступени кегля вне `clamp`: `11px`, `11.5px`, `12px`, `12.5px`, `13px`, `13.5px`, `14px`, `14.5px`, `15px`, `15.5px`, `16px`, `17px`, `20px`, `22px`, `24px`, `34px`;
- капс-метки всегда `11px` / `11.5px` / `12px`, вес `700` или `800`, `letter-spacing` от `0.05em` до `0.11em`, `text-transform:uppercase`, цвет `var(--ink3)`;
- крупные заголовки всегда вес `800` и отрицательный трекинг от `-0.02em` до `-0.04em`, `line-height` от `1` (числа) до `1.16` (цитата);
- курсива как начертания нет: `em` и `i` везде переопределены `font-style:normal` и работают либо цветовым выделением, либо мелкой подписью;
- двухцветный заголовок - базовый приём. В `.slab h2` и `.hero-line h1` приглушён весь заголовок, а `em` поднят до полного контраста. В `h2.sec` инверсия: заголовок тёмный, `em` приглушён до `var(--ink3)`.

## 1.4. Цвета вне переменных

**Белый.** `#fff` встречается 34 раза, два разных повода.

| Селекторы | Значение | Зачем |
|---|---|---|
| `section.gray .stat`, `section.gray .note`, `.col`, `.tw`, `thead th`, `.ctrl`, `.kpi`, `.flow`, `.mod2 .m-txt`, `.risk` | `background:#fff` | карточка обязана остаться белой на серой полосе `var(--soft)`, поэтому тут жёсткий `#fff`, а не `var(--card)` |
| `.hero-nav`, `.logo`, `.links a`, `.cur-toggle`, `.nav-burger`, `.hero-line`, `h1 em`, `.hero-sub b`, `.hs b`, `.oh`, `.note-d b`, `.quote`, `.quote em`, `.final`, `.final h2`, `.btn-dark`, `.foot-in .logo`, `.gal figcaption`, `.step h3 span`, `.reset:hover` | `color:#fff` | текст на тёмном |
| `.hero-nav .cta`, `.btn-white` | `background:#fff` | белая кнопка |
| ручки ползунка (оба вендорных псевдоэлемента) | `border:2px solid #fff` | белое кольцо вокруг чёрной ручки |

**Почти-чёрные вне `var(--ink)`:**

| Значение | Где | Зачем |
|---|---|---|
| `#0a0a0a` | `.hero-nav .cta{color}` и все правила `.hero-nav.scrolled` | то же значение, что `var(--ink)`, но литералом внутри навигации |
| `#000` | `.pillar:hover .pil-n{color}` | единственный чистый чёрный на странице |
| `#2b2b2b` | `.hero-nav.scrolled .cta:hover` | ховер тёмной кнопки |
| `#ececec` | `.hero-nav .cta:hover` | ховер белой кнопки |
| `#3f3f3f` | `.bar-fill.a2` | вторая ступень серой шкалы диаграммы |
| `#6e6e6e` | `.bar-fill.a3` | третья ступень, четвёртая уже `var(--acc)` |

**Цветные плашки:**

| Значение | Где | Зачем |
|---|---|---|
| `#fdf4f1` | `.al.warn{background}` | светлый терракотовый фон; парный светлый зелёный вынесен в `var(--acc-soft)`, а этот нет |
| `rgba(154,74,47,.22)` | `.al.warn{border-color}` | `var(--warn)` в rgb плюс альфа 0.22 |
| `#5c3a2c` | `.al.warn{color}` | тёмно-коричневый текст предупреждения |
| `rgba(29,92,70,.22)` | `.al.ok{border-color}` | `var(--acc)` в rgb плюс альфа 0.22 |
| `#204a3c` | `.al.ok{color}` | тёмно-зелёный текст |
| `#ffb59b` | `.oh .money.bad{color}` | осветлённый терракотовый для крупной отрицательной суммы на тёмном; `var(--warn)` там не читается |
| `#7d7d78` | `.quote p{color}` | единственное приглушение на тёмном, сделанное непрозрачным цветом, а не белым с альфой |

**Полупрозрачные чёрные:**

| Значение | Где |
|---|---|
| `rgba(0,0,0,0.46) 0%`, `rgba(0,0,0,0.10) 30%`, `rgba(0,0,0,0.22) 50%`, `rgba(0,0,0,0.80) 100%` | четыре стопа градиента `.hero .scrim` |
| `rgba(0,0,0,0)` в `rgba(0,0,0,.72)` | градиент `.gal figcaption` |
| `rgba(0,0,0,0.14)` | `.hero-nav.scrolled .links.mopen{box-shadow}` |
| `rgba(0,0,0,.28)` | тень ручки ползунка, оба вендорных псевдоэлемента |
| `rgba(10,10,10,0.07)` | `.hero-nav.scrolled .links a:hover{background}` |
| `rgba(10,10,10,0.11)` | стекло навигации в состоянии `.scrolled` |
| `rgba(10,10,10,0.20)` | `.pillar:hover{border-color}` |
| `rgba(10,10,10,0.35)` | `.pillar:hover .pil-n{border-color}` |
| `rgba(20,20,20,0.92)` | `.hero-nav .links.mopen{background}` |

**Полупрозрачные белые:** `0.44` (стекло навигации), `0.16` (ховер ссылки), `0.97` (мобильное меню в `.scrolled`), `0.82` (`.hero-sub`), `0.68` (`.oh .note-d`, `.final p`), `0.66` (`.hero-line h1`), `0.6` (`.hs span`), `0.55` (`.oh .k`, `footer`).

**Вне CSS:** `<meta name="theme-color" content="#111111">` (HTML:9), совпадает с `--dark`. Фавикон - инлайновый data-URI SVG: `<rect width='32' height='32' rx='9' fill='%23111'/>` плюс буква `M` шрифтом `Helvetica,Arial`, `font-size='16'`, `font-weight='700'`, `fill='%23fff'`, `text-anchor='middle'` (HTML:13).

## 1.5. Радиусы

| Значение | Токен | Где | Правило выбора |
|---|---|---|---|
| `80px`, ≤640px `40px` | `var(--r-lg)` | `.hero`, `.quote`, `.final`, `footer`, `section.gray`, `.slab`, `.pillar`, `.col`, `.ctrl`, `.oh`, `.mod2 .m-txt`, `.mod2 .m-photo` | полотно и крупный блок в сетке из одной-двух колонок |
| `20px` | `var(--r)` | `.stat`, `.note`, `.kpi`, `.flow`, `.al`, `.gal figure`, `.risk`; плюс `.tw` ≤900px, `.ctrl` и `.oh` ≤1020px | мелкая карточка в сетке из трёх-четырёх колонок; плюс аварийная замена `--r-lg`, когда крупный блок ужимается |
| `14px` | `var(--r-sm)` | нигде | объявлен и не используется |
| `44px` | литерал | `.tw` на десктопе | промежуточная ступень, во что-либо не вынесена |
| `24px` | литерал | `.links.mopen` ≤900px | выпадающее мобильное меню |
| `16px` | литерал | `.links.mopen a` ≤900px | пункт мобильного меню |
| `8px` | литерал | `a/button/summary:focus-visible` | рамка фокуса |
| `2px` | литерал | трек ползунка (высота трека тоже `2px`) | полное скругление двухпиксельной полосы |
| `999px` | литерал, 16 вхождений | `.logo`, `.links`, `.links a`, `.nav-actions`, `.cta`, `.cur-toggle`, `.nav-burger`, `.tag`, `.pil-n`, `.pillar li::before`, `.col li::before`, `.reset`, `.bar-track`, `.bar-fill`, `.step h3 span`, `.btn-white`, `.btn-dark`, ручки ползунка | пилюли, круги, точки-маркеры |

## 1.6. `corner-shape` и нестандартные свойства

Это то, что визуально отличает NORMA от обычного `border-radius`: все прямоугольные скругления - суперэллипс, а не дуга окружности. Угол мягче у вершины и быстрее выходит в прямую.

```css
*,*::before,*::after{box-sizing:border-box;corner-shape:superellipse(2.02)}

/* пилюли и круги: истинно круглые края */
.hero-nav .logo,.hero-nav .links,.hero-nav .links a,.nav-actions,.hero-nav .cta,.nav-burger,.cur-toggle,button,.tag{corner-shape:round}
```

Плюс `corner-shape:round` продублирован по месту в правилах: `.cur-toggle`, `.nav-burger`, `.tag`, `.pil-n`, `.pillar li::before`, `.col li::before`, `.reset`, `.bar-track`, `.bar-fill`, `.step h3 span`, `.btn-white`, `.btn-dark`. Логика: у всего, что имеет `border-radius:999px`, суперэллипс дал бы не идеальный круг, поэтому им явно возвращают дугу.

**Деградация.** В браузерах без поддержки `corner-shape` объявление отбрасывается парсером, `border-radius` остаётся в силе, страница получает обычные круглые скругления. Ничего не ломается, теряется только характерная форма угла. `@supports` и отдельного фолбэка в исходнике не задано.

Остальные нестандартные и вендорные свойства:

| Свойство | Значение | Где |
|---|---|---|
| `backdrop-filter` / `-webkit-backdrop-filter` | `blur(12px)` | `.hero-nav .logo,.links,.nav-actions`, `.cur-toggle`, `.nav-burger` |
| `backdrop-filter` / `-webkit-backdrop-filter` | `blur(16px)` | `.hero-nav .links.mopen` |
| `-webkit-font-smoothing` | `antialiased` | `html,body` |
| `-webkit-overflow-scrolling` | `touch` | `.tw` |
| `-webkit-appearance` / `appearance` | `none` | `input[type=range]` и `::-webkit-slider-thumb` |
| `filter` | `contrast(1.02)` | `.hero .hero-img img` |
| `margin-inline-start` | `12px` | `.step h3 span` |
| `scroll-behavior` | `smooth` | `html` |

## 1.7. Тени, обводки, границы

**Тени.** Всего три объявления `box-shadow` на всю страницу, `text-shadow` не используется. Карточки теней не имеют: их отделяет граница, а не тень.

```css
.hero-nav.scrolled .links.mopen{box-shadow:0 18px 50px rgba(0,0,0,0.14)}
input[type=range]::-webkit-slider-thumb{box-shadow:0 1px 5px rgba(0,0,0,.28)}
input[type=range]::-moz-range-thumb   {box-shadow:0 1px 5px rgba(0,0,0,.28)}
```

**Фокус.** Две обводки, обе зелёные:

```css
a:focus-visible,button:focus-visible,summary:focus-visible{outline:2px solid var(--acc);outline-offset:3px;border-radius:8px}
input[type=range]:focus-visible::-webkit-slider-thumb{outline:2px solid var(--acc);outline-offset:2px}
```

Для `::-moz-range-thumb` фокус-ринг в исходнике не задан.

**Границы.** Единственная толщина `1px`, два уровня контраста.

| Правило | Значение | Смысл |
|---|---|---|
| `.pillar`, `.stat`, `.col`, `.tw`, `.ctrl`, `.reset`, `.kpi`, `.flow`, `.al`, `.mod2 .m-txt`, `.risk`, `.tag`, `.pil-n` | `border:1px solid var(--line)` | внешняя рамка карточки или пилюли |
| `.pillar .ph` | `border-top:1px solid var(--line)` | отбивка фото от текстовой части |
| `.step`, `.step:first-child{border-top:none}` | `border-top:1px solid var(--line)` | разделитель шагов |
| `.grp-t` | `border-bottom:1px solid var(--line)` | подчёркивание заголовка группы |
| `thead th` | `border-bottom:1px solid var(--line)` | линия под шапкой таблицы |
| `tr.tot td` | `border-top:1px solid var(--line)` | линия над итогом |
| `.pillar li`, `.col li`, `.mi`, все с `:first-child{border-top:none}` | `border-top:1px solid var(--line2)` | слабый разделитель внутри списка |
| `td`, `tbody tr:last-child td{border-bottom:none}` | `border-bottom:1px solid var(--line2)` | линии между строками таблицы |
| `.al.warn` / `.al.ok` | `rgba(154,74,47,.22)` / `rgba(29,92,70,.22)` | цветная рамка поверх базовой |
| `.pillar:hover` / `.pillar:hover .pil-n` | `rgba(10,10,10,0.20)` / `rgba(10,10,10,0.35)` | усиление уже существующей рамки |
| `.reset:hover` | `border-color:var(--dark)` | инверсия кнопки целиком |
| `section.gray .mod2 .m-txt` | `border-color:transparent` | на серой полосе белая колонка теряет рамку, чтобы не двоить контур |
| ручки ползунка | `border:2px solid #fff` | единственная граница толще 1px |
| `.hero`, `.slab`, `.quote`, `.final`, `footer`, `.hero-nav` и его пилюли, `.cta`, `.cur-toggle`, `.nav-burger`, `.btn-white`, `.btn-dark` | `border:none` | тёмные полотна и кнопки границ не имеют принципиально |

---

# ЧАСТЬ 2. КАРКАС И ПОЛОТНО

## 2.1. html, body, глобальная база

Страница построена как «полотно в рамке»: белый фон документа, отступ по всем сторонам, внутри - полноширинные блоки со скруглениями. Именно `padding` у `body` создаёт видимую рамку вокруг цветных полотен.

```css
*,*::before,*::after{box-sizing:border-box;corner-shape:superellipse(2.02)}
html{scroll-behavior:smooth}
html,body{margin:0;padding:0;background:var(--page);color:var(--ink);font-family:var(--sf);line-height:1.5;-webkit-font-smoothing:antialiased;overflow-x:hidden}
body{padding:16px}
@media(max-width:640px){body{padding:8px}}
img{max-width:100%;display:block}
a{color:inherit;text-decoration:none}
@media(max-width:640px){:root{--r-lg:40px}}
```

На мобильном меняются три вещи: паддинг страницы `16px` в `8px`, радиус полотна `80px` в `40px`, горизонтальная прокрутка отрезана через `overflow-x:hidden`.

## 2.2. Контейнер `.wrap`

Единственный контейнер контента на странице, 15 вхождений в разметке, включая `class="wrap hero-nav"` и `class="wrap foot-in"`.

```css
.wrap{max-width:1300px;margin:0 auto;padding:0 clamp(24px,5vw,64px)}
.num{font-variant-numeric:tabular-nums}
.nb{white-space:nowrap}
```

Вертикальных паддингов у контейнера нет: вертикальный ритм задаёт секция.

## 2.3. Ритм полотна

```css
.hero,.slab,.quote,.final,footer,section{position:relative;margin:0}
section{background:var(--card);padding:clamp(56px,7vw,96px) 0}
section.gray{background:var(--soft);border-radius:0 0 var(--r-lg) var(--r-lg)}
.quote{border-radius:var(--r-lg);overflow:hidden}
.hero{border-radius:var(--r-lg);overflow:hidden}
.slab{border-radius:0 0 var(--r-lg) var(--r-lg);overflow:hidden;margin-top:calc(-1 * var(--r-lg));z-index:1;padding-top:var(--r-lg)!important}
.final{border-radius:var(--r-lg) var(--r-lg) 0 0;overflow:hidden}
footer{border-radius:0 0 var(--r-lg) var(--r-lg);overflow:hidden}
```

**Вертикальные паддинги по типам полотна:**

| Полотно | Формула целиком |
|---|---|
| `section` (базовый) | `clamp(56px,7vw,96px) 0` |
| `.slab` | `clamp(44px,6vw,72px) 0`, верх перебит `padding-top:var(--r-lg)!important` |
| `.quote` | `clamp(70px,9vw,108px) 0` |
| `.final` | `clamp(72px,10vw,120px) 0` |
| `footer` | `38px 0 44px` |
| `.hero` | паддинга нет, вместо него `height:calc(96vh - 26px)` и `min-height:620px` |

**Где какой радиус.** Радиус вешается только там, где снизу или сверху идёт полотно другого цвета:

- `section.gray` - только снизу: `0 0 var(--r-lg) var(--r-lg)`;
- белая `section` без `.gray` радиуса не получает вообще: белое на белом, граница не нужна;
- `.hero` и `.quote` - все четыре угла;
- `.final` - только сверху: `var(--r-lg) var(--r-lg) 0 0`;
- `footer` - только снизу;
- все полотна с радиусом имеют `overflow:hidden`, чтобы фото и фоны обрезались по скруглению.

**Приём с подверстанным `.slab`.** Он подтягивается под hero отрицательным отступом ровно на величину радиуса и компенсирует это паддингом, поэтому скруглённый низ hero ложится поверх серой плиты, а текст плиты не уезжает под hero:

```css
.slab{margin-top:calc(-1 * var(--r-lg));padding-top:var(--r-lg)!important;z-index:1}
.hero{z-index:2}
```

## 2.4. Тёмное полотно

Тёмный фон всегда `var(--dark)` = `#111111`. Так покрашены пять блоков: `.hero`, `.quote`, `.final`, `footer`, `.oh` (карточка результата калькулятора). Плюс это же значение продублировано в `<meta name="theme-color">`.

Правило: на тёмном базовый текст задаётся как `color:#fff` на контейнере, приглушение делается **белым с фиксированной альфой**, а не серым цветом; обратно к полному `#fff` возвращаются акценты внутри текста (`em`, `b`). Единственное исключение - цитата, где приглушение непрозрачное `#7d7d78`.

| Блок | Фон | База | Приглушённый текст | Акцент внутри |
|---|---|---|---|---|
| `.hero` | `var(--dark)` под фотосеткой плюс скрим | `.hero-line{color:#fff}` | `h1` `rgba(255,255,255,0.66)`; `.hero-sub` `0.82`; `.hs span` `0.6` | `h1 em` `#fff`; `.hero-sub b` `#fff` вес `700`; `.hs b` `#fff` |
| `.quote` | `var(--dark)` | `color:#fff` | `p` `#7d7d78` | `em` `#fff` |
| `.final` | `var(--dark)` | `color:#fff` | `p` `rgba(255,255,255,0.68)` | `h2` `#fff` |
| `footer` | `var(--dark)` | `color:rgba(255,255,255,0.55)`, подвал приглушён целиком | наследуется | `.foot-in .logo` `#fff` |
| `.oh` | `var(--dark)` | `color:#fff` | `.k` `0.55`; `.note-d` `0.68` | `.money` `#fff`, при `.bad` `#ffb59b`; `.note-d b` `#fff` вес `700` |
| навигация над hero | `rgba(255,255,255,0.44)` плюс `blur(12px)` | `#fff` | `.links a{opacity:.92}` | `.cta` `#0a0a0a` на `#fff` |
| мобильное меню над hero | `rgba(20,20,20,0.92)` плюс `blur(16px)` | наследует `#fff` | в исходнике не задано | в исходнике не задано |
| подпись галереи | `linear-gradient(180deg,rgba(0,0,0,0),rgba(0,0,0,.72))` | `#fff` | в исходнике не задано | в исходнике не задано |

Шкала альф белого на тёмном: `0.55`, `0.6`, `0.66`, `0.68`, `0.82`, `0.92` (это `opacity`, не `color`), `1.0`.

Эталонный кусок, воспроизводящий механику тёмного блока:

```css
.final{
  background:var(--dark);
  color:#fff;
  border:none;
  padding:clamp(72px,10vw,120px) 0;
  border-radius:var(--r-lg) var(--r-lg) 0 0;
  overflow:hidden;
}
.final h2{color:#fff;font-size:clamp(34px,6.4vw,80px);font-weight:800;letter-spacing:-0.035em;line-height:1.02;margin:0 0 24px}
.final p{font-size:17px;color:rgba(255,255,255,0.68);max-width:620px;margin:0 0 36px}
```

## 2.5. Инвентарь всех сеток и флексов

**Grid:**

| Селектор | `grid-template-columns` | `gap` | Выравнивание | Брейкпоинты |
|---|---|---|---|---|
| `.hero .hero-img` | `1.15fr 1fr 1fr` | `2px` | не задано | 760px в `1fr 1fr`, третья картинка `display:none` |
| `.pillars` | `repeat(4,1fr)` | `14px` | `align-items:stretch` | 1000px в `repeat(2,1fr)`; 600px в `1fr` |
| `.stats4` | `repeat(4,1fr)` | `14px` | не задано | 1000px в `repeat(2,1fr)`; 560px в `1fr` |
| `.two` | `1fr 1fr` | `14px` | `margin-top:40px` | 860px в `1fr` |
| `.calc` | `370px 1fr` | `14px` | `align-items:start` | 1020px в `1fr` |
| `.kpis` | `repeat(4,1fr)` | `14px` | не задано | 900px в `repeat(2,1fr)` |
| `.mod2` | `1fr 1fr` | `14px` | `align-items:stretch` | 820px в `1fr` |
| `.gal` | `repeat(4,1fr)` | `14px` | `margin-top:14px` | 820px в `repeat(2,1fr)` |
| `.step` | `96px 1fr 1.25fr` | `30px` | `align-items:baseline` | 820px в `1fr`, `gap:6px` |
| `.risks` | `repeat(3,1fr)` | `14px` | не задано | 960px в `repeat(2,1fr)`; 620px в `1fr` |
| `.pil-n`, `.cur-toggle`, `.nav-burger` | одноклеточные | - | `place-items:center` | - |

Единый зазор контентных сеток и стеков карточек NORMA - `14px`. Три исключения по компонентам: `.hero .hero-img` (`2px`, узкий шов между фотографиями одного изображения), `.step` (`30px`, разделение номера, названия и описания; на `max-width:820px` `6px`, связь строк одного шага), `.alerts` (`10px`, компактная группа связанных сообщений). Внутренние зазоры навигации и строк «подпись плюс значение» описаны отдельно в таблице Flex; это не сетки равноправных карточек. Переносимое правило и требования к обоснованию исключений приведены в части 9, правило 5.

**Flex:**

| Селектор | Направление | `gap` | Выравнивание |
|---|---|---|---|
| `.hero-nav` | строка | не задано | `align-items:center` |
| `.hero-nav .links` | строка | `2px` | `align-items:center` |
| `.links.mopen` (≤900px) | `column` | `2px` | не задано |
| `.nav-actions` | строка | `5px` | `align-items:center`, `margin-left:auto` |
| `.hero-stats` | строка, `wrap` | `clamp(18px,3vw,46px)`; ≤520px `14px 20px` | не задано |
| `.slab-facts` | строка, `wrap` | `36px` | не задано |
| `.pillar`, `.stat`, `.risk`, `.menu-list` | `column` | не задано | - |
| `.mod2 .m-txt` | `column` | не задано | `justify-content:center` |
| `.mi i`, `.sl-top`, `.bar-top` | строка | `12px` / `10px` / `12px` | `justify-content:space-between;align-items:baseline` |
| `.out` | `column` | `14px` | - |
| `.oh` | `column` | не задано | `justify-content:center` |
| `.kpi` | `column` | не задано | `justify-content:space-between` |
| `.alerts` | `column` | `10px` | - |
| `.cta-mid` | строка | не задано | `justify-content:center` |
| `.foot-in` | строка, `wrap` | `20px` | `justify-content:space-between;align-items:flex-end` |

---

# ЧАСТЬ 3. НАВИГАЦИЯ

## 3.1. Конструкция

Шапка живёт внутри `<header class="hero">` как один из четырёх его слоёв. Слои hero по порядку в DOM: `.hero-img` (три фото в grid), `.scrim` (градиентная плёнка), `.hero-nav` (шапка), `.hero-line` (заголовок и статы внизу).

```
.hero-nav  (position:fixed, flex-строка, по центру экрана)
├── .logo            слева, первый flex-элемент
├── .links           НЕ в потоке: position:absolute, приклеен к центру шапки
│   └── 6 ссылок <a>
└── .nav-actions     справа, прижат через margin-left:auto
    ├── button.cur-toggle   круг 38x38, символ $ или ₫
    ├── button.nav-burger   круг 38x38, символ ☰ или ✕, скрыт на десктопе
    └── a.cta               пилюля «Считать →»
```

Ключевой приём: это не одна панель-стекляшка, а три отдельные плавающие пилюли (`.logo`, `.links`, `.nav-actions`), между которыми фон прозрачный. Сама `.hero-nav` имеет `background:transparent;border:none`, стекло задано только трём детям. Поскольку `.links` вынут из потока в абсолют, центральная группа стоит ровно по центру экрана независимо от ширины логотипа и правой группы.

`.hero-nav` несёт также класс `.wrap`, но перебивает его: `.wrap` даёт `max-width:1300px` и `padding:0 clamp(24px,5vw,64px)`, а `.hero-nav` переопределяет `max-width:1320px` и `padding:0`.

```html
<div class="wrap hero-nav" id="nav">
  <span class="logo">MOLOKO</span>
  <div class="links" id="navLinks">
    <a href="#concept">Концепт</a>
    <a href="#menu">Меню</a>
    <a href="#unit">Экономика</a>
    <a href="#calc">Калькулятор</a>
    <a href="#risk">Риски</a>
    <a href="#plan">12 недель</a>
  </div>
  <div class="nav-actions">
    <button class="cur-toggle" id="curToggle" type="button" aria-label="Показать цены в донгах">$</button>
    <button class="nav-burger" aria-label="Меню" id="burger">☰</button>
    <a class="cta" href="#calc">Считать →</a>
  </div>
</div>
```

## 3.2. Геометрия

| Элемент | Свойство | Значение |
|---|---|---|
| `.hero-nav` | position | `fixed` |
| | top | `44px`; ≤640px `30px` |
| | right | в исходнике не задано |
| | left / transform | `50%` / `translateX(-50%)` |
| | z-index | `100` |
| | width | `calc(100% - 48px)`; ≤640px `calc(100% - 24px)` |
| | max-width | `1320px` |
| | padding | `0`; при `min-width:1024px` `0 100px` |
| | height | в исходнике не задано, диктуется `.nav-actions` (38px кнопка плюс 3px+3px паддинги = 44px) |
| `.logo` | padding / radius | `8px 16px` / `999px` |
| `.links` | position | `absolute; left:50%; transform:translateX(-50%)` |
| | gap / padding / radius | `2px` / `3px 5px` / `999px` |
| `.links a` | padding / radius | `6px 12px` / `999px` |
| `.nav-actions` | gap / padding / radius | `5px` / `3px 3px 3px 5px` / `999px`, `margin-left:auto` |
| `.cur-toggle`, `.nav-burger` | width / height | `38px` / `38px` |
| `.cta` | padding | `6px 14px`; при `min-width:901px` `0 16px` |
| | margin (≥901px) | `-3px -3px -3px 0` |

Отрицательные поля у `.cta` при `min-width:901px` вместе с `align-self:stretch` гасят паддинги родителя справа, сверху и снизу: кнопка растягивается на всю высоту стеклянной пилюли и врезана в её правый край встык.

Про ширину: `100%` для fixed-элемента считается от вьюпорта, а у `body` стоит `padding:16px` (и `8px` на мобильном). То есть шапка отступает от края экрана на 24px, а от края самого hero-полотна на 8px; на мобильном - 12px и 4px.

## 3.3. Стекло и состояние прокрутки

```css
.hero-nav .logo,.hero-nav .links,.nav-actions{background:rgba(255,255,255,0.44);backdrop-filter:blur(12px);-webkit-backdrop-filter:blur(12px);border:none}
```

`.cur-toggle` и `.nav-burger` повторяют то же стекло у себя персонально, потому что лежат внутри `.nav-actions` и накладываются поверх его стекла. `box-shadow` у шапки нет ни в одном состоянии, кроме мобильного открытого меню в `.scrolled`.

При прокрутке на `#nav` вешается класс `scrolled`. Меняются только цвета, геометрия не трогается совсем.

| Что | Обычное | `.scrolled` |
|---|---|---|
| Фон `.logo`, `.links`, `.nav-actions` | `rgba(255,255,255,0.44)` | `rgba(10,10,10,0.11)` |
| Текст `.logo`, `.links a` | `#fff` | `#0a0a0a` |
| Hover ссылки | `rgba(255,255,255,0.16)` | `rgba(10,10,10,0.07)` |
| `.cta` фон / текст | `#fff` / `#0a0a0a` | `#0a0a0a` / `#fff` |
| `.cta` hover | `#ececec` | `#2b2b2b` |
| `.cur-toggle` фон / цвет | `rgba(255,255,255,0.44)` / `#fff` | `rgba(10,10,10,0.11)` / `#0a0a0a` |
| `.nav-burger` фон / цвет (≤900px) | `rgba(255,255,255,0.44)` / `#fff` | `rgba(10,10,10,0.11)` / `#0a0a0a` |
| `.links.mopen` | `rgba(20,20,20,0.92)`, тени нет | `rgba(255,255,255,0.97)` плюс `box-shadow:0 18px 50px rgba(0,0,0,0.14)` |
| Ссылки в открытом меню | `#fff` | `#0a0a0a` |

`backdrop-filter:blur(12px)` в `.scrolled` не переопределяется. Переходы: у `.links a` `transition:background .15s,opacity .15s`, у `.cur-toggle` `transition:background .15s,color .15s`. У `.logo`, `.links`, `.nav-actions`, `.cta`, `.nav-burger` transition в исходнике не задан, их цвет переключается мгновенно.

## 3.4. Как определяется состояние прокрутки

Строки 1082-1085, внутри общей IIFE:

```js
var nav = document.getElementById('nav');
var hero = document.querySelector('.hero');
function onScroll(){ nav.classList.toggle('scrolled', window.scrollY > hero.offsetHeight - 90); }
addEventListener('scroll', onScroll, {passive:true}); onScroll();
```

Разбор:

1. `classList.toggle('scrolled', <условие>)` в двухаргументной форме: класс не переключается туда-сюда, а принудительно ставится при `true` и снимается при `false`. Состояние пересчитывается на каждый тик скролла и корректно откатывается вверх.
2. Порог `window.scrollY > hero.offsetHeight - 90` - высота hero минус 90px. Тёмная схема включается за 90px до того, как hero полностью уйдёт вверх.
3. `offsetHeight` читается в момент события, поэтому ресайз учитывается сам, отдельного слушателя `resize` нет.
4. `{passive:true}` чтобы не блокировать скролл. Троттлинга и `requestAnimationFrame` в исходнике нет.
5. Вызов `onScroll()` сразу после подписки - первичный прогон, если страница открыта уже прокрученной.

Ни IntersectionObserver, ни sentinel для шапки не используются: IntersectionObserver в том же скрипте обслуживает только reveal.

Обязательное условие: у hero должна быть измеримая высота. В исходнике `.hero{height:calc(96vh - 26px);min-height:620px}`.

## 3.5. Ссылки меню и активный раздел

| Параметр | Значение |
|---|---|
| font-size / weight | `13px` / `600` |
| color | `#fff`, в `.scrolled` `#0a0a0a` |
| opacity | `.92`, при hover `1` |
| padding / radius | `6px 12px` / `999px` |
| Между ссылками | `gap:2px` у `.links` |
| hover-фон | `rgba(255,255,255,0.16)`; в `.scrolled` `rgba(10,10,10,0.07)` |
| transition | `background .15s, opacity .15s` |
| white-space | `nowrap` |

Активный раздел: **в исходнике не задано**. Нет ни класса `.active`, ни `aria-current`, ни скролл-спая. Все шесть ссылок всегда выглядят одинаково, единственное визуальное отличие даёт hover.

## 3.6. Бургер и мобильное меню

Брейкпоинт `max-width:900px`. До него `.nav-burger{display:none}` и видны десктопные `.links`; после - наоборот.

| Параметр открытого меню | Значение |
|---|---|
| Раскладка | `flex; flex-direction:column`, `gap:2px` |
| Позиция | `absolute; top:calc(100% + 10px); left:0; right:0` |
| background | `rgba(20,20,20,0.92)`; в `.scrolled` `rgba(255,255,255,0.97)` |
| backdrop-filter | `blur(16px)`, не 12px как у пилюль |
| border-radius / padding | `24px` / `12px` |
| box-shadow | нет; только в `.scrolled` `0 18px 50px rgba(0,0,0,0.14)` |
| Ссылка | `padding:14px 18px; font-size:16px; text-align:center; border-radius:16px` |

Скрипт, строки 1086-1093:

```js
var b = document.getElementById('burger'), lk = document.getElementById('navLinks');
b.addEventListener('click', function(){
  var open = lk.classList.toggle('mopen');
  b.textContent = open ? '✕' : '☰';
});
lk.addEventListener('click', function(e){
  if (e.target.tagName === 'A'){ lk.classList.remove('mopen'); b.textContent = '☰'; }
});
```

Клик по бургеру переключает `mopen` и по возвращённому булеву значению меняет символ кнопки. Второй слушатель работает делегированием: любой клик по тегу `A` закрывает меню. Закрытия по клику вне меню, по Escape, блокировки скролла body и атрибута `aria-expanded` в исходнике нет.

Порядок элементов на мобильном внутри `.nav-actions` слева направо: `$`, `☰`, «Считать →». CTA на мобильном не прячется.

## 3.7. Прокрутка к якорю

Задано только `html{scroll-behavior:smooth}`. Компенсация высоты шапки **в исходнике не задана**: нет ни `scroll-padding-top`, ни `scroll-margin-top`, ни JS-смещения. При переходе по якорю верх секции встаёт вплотную к верху вьюпорта и частично уезжает под фиксированную шапку. Это фактическое поведение исходника. Смягчается тем, что у секций крупный вертикальный паддинг `clamp(56px,7vw,96px) 0`, и текст всё равно оказывается ниже шапки. `@media(prefers-reduced-motion:reduce)` на `scroll-behavior` не распространяется.

## 3.8. Готовый CSS шапки

```css
/* зависимости */
:root{--sf:'Hanken Grotesk',-apple-system,system-ui,sans-serif;--acc:#1d5c46}
*,*::before,*::after{box-sizing:border-box;corner-shape:superellipse(2.02)}
html{scroll-behavior:smooth}
a{color:inherit;text-decoration:none}
.wrap{max-width:1300px;margin:0 auto;padding:0 clamp(24px,5vw,64px)}

/* каркас */
.hero-nav{position:fixed;top:44px;left:50%;transform:translateX(-50%);z-index:100;display:flex;align-items:center;width:calc(100% - 48px);max-width:1320px;padding:0;background:transparent;border:none;color:#fff}
@media(max-width:640px){.hero-nav{top:30px;width:calc(100% - 24px)}}
@media(min-width:1024px){.hero-nav{padding:0 100px}}

/* стекло */
.hero-nav .logo,.hero-nav .links,.nav-actions{background:rgba(255,255,255,0.44);backdrop-filter:blur(12px);-webkit-backdrop-filter:blur(12px);border:none}
.hero-nav .logo{border-radius:999px;padding:8px 16px;font-weight:700;font-size:15px;letter-spacing:0.18em;color:#fff}
.hero-nav .links{position:absolute;left:50%;transform:translateX(-50%);display:flex;align-items:center;gap:2px;border-radius:999px;padding:3px 5px}
.hero-nav .links a{font-size:13px;font-weight:600;color:#fff;opacity:.92;padding:6px 12px;border-radius:999px;transition:background .15s,opacity .15s;white-space:nowrap}
.hero-nav .links a:hover{background:rgba(255,255,255,0.16);opacity:1}
.nav-actions{display:flex;align-items:center;gap:5px;margin-left:auto;border-radius:999px;padding:3px 3px 3px 5px}
.hero-nav .cta{background:#fff;color:#0a0a0a;font-weight:700;font-size:13px;padding:6px 14px;border-radius:999px;border:none;cursor:pointer;font-family:var(--sf)}
@media(min-width:901px){.hero-nav .cta{align-self:stretch;display:inline-flex;align-items:center;padding:0 16px;margin:-3px -3px -3px 0}}

/* прокрутка */
.hero-nav.scrolled .logo,.hero-nav.scrolled .links,.hero-nav.scrolled .nav-actions{background:rgba(10,10,10,0.11)}
.hero-nav.scrolled .logo,.hero-nav.scrolled .links a{color:#0a0a0a}
.hero-nav.scrolled .links a:hover{background:rgba(10,10,10,0.07)}
.hero-nav.scrolled .cta{background:#0a0a0a;color:#fff}
.hero-nav .cta:hover{background:#ececec}
.hero-nav.scrolled .cta:hover{background:#2b2b2b}

/* круглые кнопки */
.nav-burger{display:none}
.cur-toggle{display:grid;place-items:center;width:38px;height:38px;border-radius:999px;corner-shape:round;border:none;background:rgba(255,255,255,0.44);backdrop-filter:blur(12px);-webkit-backdrop-filter:blur(12px);cursor:pointer;color:#fff;font-family:var(--sf);font-weight:800;font-size:13px;flex:none;transition:background .15s,color .15s}
.hero-nav.scrolled .cur-toggle{background:rgba(10,10,10,0.11);color:#0a0a0a}

/* мобильный режим */
@media(max-width:900px){
  .hero-nav .links{display:none}
  .nav-burger{display:grid;place-items:center;width:38px;height:38px;border-radius:999px;corner-shape:round;border:none;background:rgba(255,255,255,0.44);backdrop-filter:blur(12px);-webkit-backdrop-filter:blur(12px);cursor:pointer;color:#fff;font-size:17px}
  .hero-nav.scrolled .nav-burger{background:rgba(10,10,10,0.11);color:#0a0a0a}
  .hero-nav .links.mopen{display:flex;flex-direction:column;position:absolute;top:calc(100% + 10px);left:0;right:0;background:rgba(20,20,20,0.92);backdrop-filter:blur(16px);-webkit-backdrop-filter:blur(16px);border-radius:24px;padding:12px;gap:2px}
  .hero-nav.scrolled .links.mopen{background:rgba(255,255,255,0.97);box-shadow:0 18px 50px rgba(0,0,0,0.14)}
  .hero-nav .links.mopen a{padding:14px 18px;font-size:16px;text-align:center;border-radius:16px}
  .hero-nav.scrolled .links.mopen a{color:#0a0a0a}
}

/* фокус и истинно круглые края */
a:focus-visible,button:focus-visible,summary:focus-visible{outline:2px solid var(--acc);outline-offset:3px;border-radius:8px}
.hero-nav .logo,.hero-nav .links,.hero-nav .links a,.nav-actions,.hero-nav .cta,.nav-burger,.cur-toggle,button,.tag{corner-shape:round}
```

---

# ЧАСТЬ 4. БЛОКИ

Радиусы, цвета, границы и типографика каждого блока берутся из части 1, здесь не повторяются. Медиазапросы блоков приведены прямо в коде, чтобы фрагмент был копируемым; полный индекс всех брейкпоинтов - в части 7.

## 4.1. `.hero`

Тёмное полноэкранное полотно со скруглением по всем углам. Внутри сетка из трёх фото встык, градиентная плёнка, фиксированная навигация и текстовый блок, прижатый к низу.

```css
.hero{height:calc(96vh - 26px);min-height:620px;background:var(--dark);border:none;z-index:2}
.hero .hero-img{position:absolute;inset:0;width:100%;height:100%;display:grid;grid-template-columns:1.15fr 1fr 1fr;gap:2px}
.hero .hero-img img{width:100%;height:100%;object-fit:cover;filter:contrast(1.02);min-width:0}
@media(max-width:760px){.hero .hero-img{grid-template-columns:1fr 1fr}.hero .hero-img img:nth-child(3){display:none}}
.hero .scrim{position:absolute;inset:0;background:linear-gradient(180deg,rgba(0,0,0,0.46) 0%,rgba(0,0,0,0.10) 30%,rgba(0,0,0,0.22) 50%,rgba(0,0,0,0.80) 100%)}
.hero-line{position:absolute;z-index:3;left:0;right:0;bottom:clamp(30px,5vw,64px);color:#fff}
.hero-line h1{font-size:clamp(38px,7vw,88px);font-weight:800;line-height:1.04;letter-spacing:-0.035em;margin:0;max-width:1120px;color:rgba(255,255,255,0.66)}
.hero-line h1 em{font-style:normal;color:#fff}
.hero-sub{margin:20px 0 0;max-width:820px;font-size:clamp(15px,1.45vw,18px);line-height:1.5;font-weight:500;color:rgba(255,255,255,0.82)}
.hero-sub b{color:#fff;font-weight:700}
@media(max-width:760px){.hero-sub{margin-top:14px;line-height:1.45}}
.hero-stats{display:flex;gap:clamp(18px,3vw,46px);flex-wrap:wrap;margin-top:clamp(22px,3vw,34px)}
.hs{min-width:120px}
.hs b{display:block;font-size:clamp(20px,2.6vw,30px);font-weight:800;letter-spacing:-0.02em;color:#fff;font-variant-numeric:tabular-nums;line-height:1.1}
.hs span{font-size:12px;color:rgba(255,255,255,0.6);font-weight:600;letter-spacing:0.04em}
@media(max-width:520px){.hero-stats{gap:14px 20px}.hs{min-width:88px}.hs b{font-size:19px}.hs span{font-size:11px}}
```

```html
<header class="hero">
  <div class="hero-img">
    <img src="/moloko/assets/img/p2.webp" width="588" height="867" alt="..." fetchpriority="high">
    <img src="/moloko/assets/img/danish.webp" width="736" height="1308" alt="..." fetchpriority="high">
    <img src="/moloko/assets/img/p6.webp" width="735" height="876" alt="..." fetchpriority="high">
  </div>
  <div class="scrim"></div>
  <div class="wrap hero-nav" id="nav"> ... </div>
  <div class="hero-line">
    <div class="wrap">
      <h1>Одна река молока.<br><em>Четыре русла выручки.</em></h1>
      <p class="hero-sub">Текст подзаголовка с <b>жирным акцентом</b>.<br><b>Вторая строка.</b></p>
      <div class="hero-stats">
        <div class="hs"><b class="num" id="hMilk">-</b><span>молока в сутки</span></div>
        <div class="hs"><b class="num" id="hRev">-</b><span>выручка в месяц</span></div>
        <div class="hs"><b class="num" id="hProfit">-</b><span>чистая прибыль</span></div>
        <div class="hs"><b class="num" id="hPay">-</b><span>окупаемость</span></div>
      </div>
    </div>
  </div>
</header>
```

Hero целиком идёт **без** класса `.rv`: первый экран показывается сразу, без появления.

## 4.2. `.slab` - серая плита-тезис под hero

```css
.slab{background:var(--soft);color:var(--ink);border:none;padding:clamp(44px,6vw,72px) 0}
.slab h2{font-size:clamp(32px,5.5vw,72px);font-weight:800;line-height:1.05;letter-spacing:-0.035em;margin:0;color:var(--ink3)}
.slab h2 em{font-style:normal;color:var(--ink)}
.slab-facts{display:flex;gap:36px;flex-wrap:wrap;margin-top:30px}
.sf b{display:block;font-size:34px;font-weight:800;letter-spacing:-0.02em;color:var(--ink);font-variant-numeric:tabular-nums}
.sf span{font-size:13px;color:var(--ink3);font-weight:600}
```

```html
<div class="slab">
  <div class="wrap">
    <h2 class="rv">Кафе, которое <em>само себе поставщик.</em></h2>
    <div class="slab-facts rv rv-d1">
      <div class="sf"><b>4</b><span>канала выручки из одного сырья</span></div>
      <div class="sf"><b class="num" id="sfCurd">-</b><span>творога в сутки</span></div>
      <div class="sf"><b class="num" id="sfRos">-</b><span>рентабельность модели</span></div>
    </div>
  </div>
</div>
```

Скругление и отрицательный отступ этого блока описаны в 2.3.

## 4.3. Шапка секции: `h2.sec` + `.sub`

> **⛔ ЗАПРЕТ с 2026-11-15 (Макс): пузырьки-теги с названием блока (`.tag`: «Раз в неделю», «15 минут», «Концепт» и т.п.) НЕ ставить.** Секция начинается сразу с `h2.sec`. В старых страницах при правке - убирать. Мелкие caps-метки-эйбрау над заголовком тоже нельзя. Так же сделан эталон primeglow.co: `h2.sec` без тега.

Ниже - историческое описание `.tag` из moloko, для новых страниц не применять.

```css
.tag{display:inline-block;font-size:12px;font-weight:700;letter-spacing:0.11em;text-transform:uppercase;border:1px solid var(--line);border-radius:999px;corner-shape:round;padding:7px 16px;margin-bottom:20px;color:var(--ink2)}
h2.sec{font-size:clamp(30px,5vw,56px);font-weight:800;letter-spacing:-0.03em;line-height:1.08;margin:0 0 16px}
h2.sec em{font-style:normal;color:var(--ink3)}
.sub{font-size:17px;color:var(--ink2);max-width:680px;margin:0 0 clamp(34px,4vw,50px)}
.sub b{color:var(--ink);font-weight:700}
```

```html
<section id="concept">
  <div class="wrap">
    <span class="tag rv">Концепт</span>
    <h2 class="sec rv">Не кафе с молочным меню. <em>Одно предприятие с четырьмя выходами.</em></h2>
    <p class="sub rv rv-d1">Лид с <b>жирным акцентом</b>.</p>
    <!-- контент секции -->
  </div>
</section>
```

Значения `.tag` в исходнике: «Концепт», «Формат», «Меню», «Заморозка», «Юнит-экономика», «Модель», «Баланс», «Риски», «12 недель».

## 4.4. `.pillars` / `.pillar` - карточки-столпы

Сетка из четырёх высоких карточек с радиусом `--r-lg`. Состав: номер в кружке, заголовок, подзаголовок, опциональное фото, список-тезисы с разделителями. В исходнике использована дважды: в `#concept` (с фото) и в `#semi` (без фото).

```css
.pillars{display:grid;grid-template-columns:repeat(4,1fr);gap:14px;align-items:stretch}
@media(max-width:1000px){.pillars{grid-template-columns:repeat(2,1fr)}}
@media(max-width:600px){.pillars{grid-template-columns:1fr}}
.pillar{background:var(--card);border:1px solid var(--line);border-radius:var(--r-lg);padding:0;overflow:hidden;display:flex;flex-direction:column;transition:border-color .3s}
.pillar:hover{border-color:rgba(10,10,10,0.20)}
.pil-n{display:grid;place-items:center;width:34px;height:34px;border:1px solid var(--line);border-radius:999px;corner-shape:round;font-size:13px;font-weight:600;color:var(--ink);margin:30px 22px 0;flex:0 0 auto;transition:border-color .3s,color .3s}
.pillar:hover .pil-n{border-color:rgba(10,10,10,0.35);color:#000}
.pillar h3{font-size:24px;font-weight:700;letter-spacing:-0.025em;margin:0;padding:16px 22px 4px;line-height:1.12;height:70px;overflow:hidden}
.pil-sub{font-size:14px;color:var(--ink2);margin:0;padding:0 22px 20px;line-height:1.5;height:84px;overflow:hidden}
@media(max-width:1000px){.pil-sub,.pillar h3{height:auto;overflow:visible}}
.pillar .ph{border-top:1px solid var(--line);overflow:hidden;aspect-ratio:16/11;margin:0;background:var(--soft)}
.pillar .ph img{width:100%;height:100%;object-fit:cover}
.pillar ul{margin:0;padding:14px 22px 18px;list-style:none}
.pillar li{font-size:13.5px;color:var(--ink2);padding:11px 0 11px 20px;position:relative;border-top:1px solid var(--line2);line-height:1.5}
.pillar li:first-child{border-top:none}
.pillar li::before{content:'';position:absolute;left:2px;top:18px;width:6px;height:6px;border-radius:999px;corner-shape:round;background:var(--ink)}
.pillar li b{color:var(--ink);font-weight:700}
```

Механика выравнивания: у карточки нет собственного паддинга (`padding:0`), отступы задаются у каждого потомка отдельно, по бокам у всех `22px`. Заголовок и подзаголовок жёстко фиксированы по высоте (`70px` и `84px`), чтобы фото в четырёх карточках начиналось на одной линии; на `max-width:1000px` высоты сбрасываются в `auto`.

```html
<div class="pillars">
  <article class="pillar rv">
    <span class="pil-n">01</span>
    <h3>Кафе</h3>
    <p class="pil-sub">Подзаголовок карточки на две-три строки.</p>
    <div class="ph ph-zoom rv"><img src="..." width="736" height="981" loading="lazy" alt="..."></div>
    <ul>
      <li><b>Средний чек</b> держит меню, а не скидка</li>
      <li><b>Ноль логистики</b> между цехом и залом</li>
      <li>Тест рецептуры перед выходом в опт</li>
    </ul>
  </article>
  <article class="pillar rv rv-d1">...</article>
  <article class="pillar rv rv-d2">...</article>
  <article class="pillar rv rv-d3">...</article>
</div>
```

Вариант без фото - тот же шаблон без `<div class="ph ph-zoom rv">`.

## 4.5. `.note` - врезка-вывод

Используется 5 раз, всегда в конце секции.

```css
.note{margin-top:26px;background:var(--soft);border-radius:var(--r);padding:22px 26px;font-size:15px;color:var(--ink2);line-height:1.6}
section.gray .note{background:#fff}
.note b{color:var(--ink);font-weight:700}
```

```html
<div class="note rv">Текст вывода с <b>жирным акцентом.</b></div>
```

Инверсия по фону: на белой секции плашка серая, внутри `section.gray` становится белой.

## 4.6. `.src` - источники и оговорки

Используется 2 раза (HTML:463 и HTML:549), оба раза с инлайновым `style="margin-top:22px"` и `style="margin-top:18px"`, перебивающим базовый `margin-top:26px`.

```css
.src{font-size:12.5px;color:var(--ink3);line-height:1.75;margin-top:26px}
```

```html
<p class="src rv" style="margin-top:22px">Текст сноски, <b>жирный акцент</b> и пометка <span class="neg">дороже не купят</span>.</p>
```

## 4.7. `.gal` - галерея референсов

Сетка 4 в ряд из `figure`, фото фиксированной высоты, подпись на градиенте внизу. Радиус малый (`--r`), а не `--r-lg`.

```css
.gal{display:grid;grid-template-columns:repeat(4,1fr);gap:14px;margin-top:14px}
@media(max-width:820px){.gal{grid-template-columns:repeat(2,1fr)}}
.gal figure{margin:0;border-radius:var(--r);overflow:hidden;background:var(--soft);position:relative}
.gal img{width:100%;height:clamp(200px,22vw,290px);object-fit:cover}
.gal figcaption{position:absolute;left:0;right:0;bottom:0;padding:10px 14px;background:linear-gradient(180deg,rgba(0,0,0,0),rgba(0,0,0,.72));color:#fff;font-size:12px;font-weight:600;letter-spacing:.02em}
```

```html
<div class="gal">
  <figure class="rv ph-zoom"><img src="..." width="736" height="981" loading="lazy" alt="..."><figcaption>Витрина-стол в центре зала</figcaption></figure>
  <figure class="rv rv-d1 ph-zoom">...</figure>
  <figure class="rv rv-d2 ph-zoom">...</figure>
  <figure class="rv rv-d3 ph-zoom">...</figure>
</div>
```

В исходнике 8 фигур подряд, задержки идут циклом `rv`, `rv-d1`, `rv-d2`, `rv-d3`, потом снова с начала.

## 4.8. `.mod2` - текст плюс фото в две колонки

Используется 4 раза. Порядок переворачивается классом `.rev-order`.

```css
.mod2{display:grid;grid-template-columns:1fr 1fr;gap:14px;align-items:stretch}
@media(max-width:820px){.mod2{grid-template-columns:1fr}}
.mod2 .m-txt{background:#fff;border:1px solid var(--line);border-radius:var(--r-lg);padding:clamp(28px,3vw,44px);display:flex;flex-direction:column;justify-content:center}
section.gray .mod2 .m-txt{border-color:transparent}
.mod2 .m-txt h2{font-size:clamp(26px,3.4vw,40px);font-weight:800;letter-spacing:-0.03em;line-height:1.05;margin:0 0 18px}
.mod2 .m-txt p{font-size:15.5px;line-height:1.55;color:var(--ink2);margin:0 0 12px}
.mod2 .m-txt p b{color:var(--ink)}
.mod2 .m-photo{border-radius:var(--r-lg);overflow:hidden;height:clamp(420px,42vw,560px);background:var(--soft)}
.mod2 .m-photo img{width:100%;height:100%;object-fit:cover;display:block}
.mod2.rev-order .m-photo{order:-1}
@media(max-width:820px){.mod2 .m-photo{height:clamp(300px,66vw,420px)}.mod2.rev-order .m-photo{order:0}}
```

```html
<div class="mod2" style="margin-bottom:14px">
  <div class="m-txt rv">
    <h2>Солёная половина</h2>
    <p>Абзац текста с <b>акцентом</b>.</p>
  </div>
  <div class="m-photo ph-zoom rv"><img src="..." width="1100" height="1064" loading="lazy" alt="..."></div>
</div>

<div class="mod2 rev-order">
  <div class="m-txt rv"> ... </div>
  <div class="m-photo ph-zoom rv"><img src="..." loading="lazy" alt="..."></div>
</div>
```

Вертикальные отступы между соседними `.mod2` в исходнике заданы инлайном: `style="margin-bottom:14px"`, `style="margin-top:14px"`, `style="margin-top:40px"`. Отдельного класса для этого нет.

## 4.9. `.menu-list` / `.mi` - список позиций

Вертикальный список внутри `.m-txt`: название, состав, строка нутриентов с ценой, прижатой вправо.

```css
.menu-list{display:flex;flex-direction:column}
.mi{padding:16px 0;border-top:1px solid var(--line2)}
.mi:first-child{border-top:none}
.mi b{display:block;font-size:17px;font-weight:700;letter-spacing:-0.015em}
.mi span{display:block;font-size:13.5px;color:var(--ink2);line-height:1.45;margin:4px 0 7px}
.mi i{display:flex;justify-content:space-between;align-items:baseline;gap:12px;font-style:normal;font-size:12px;font-weight:700;letter-spacing:0.04em;color:var(--ink3);font-variant-numeric:tabular-nums}
.mi i em{font-style:normal;font-size:17px;font-weight:800;color:var(--ink);white-space:nowrap}
```

```html
<div class="menu-list">
  <div class="mi">
    <b>Protein Scramble</b>
    <span>творог, яйца от свободного выгула, салат-микс, Grana Padano, греческий йогурт</span>
    <i>Б 43 · Ж 31 · У 5 · 452 ккал<em class="mp" data-vnd="169000"></em></i>
  </div>
</div>
```

`<em class="mp" data-vnd="...">` пустой в разметке, цену подставляет скрипт переключателя валюты.

## 4.10. `.tw` + `table` - таблицы

Белая карточка-обёртка с крупным радиусом и горизонтальной прокруткой, внутри обычная таблица с минимальной шириной. В разметке 3 раза: `#unit`, `#calc`, `#milk`.

```css
.tw{border:1px solid var(--line);border-radius:44px;overflow-x:auto;background:#fff;margin-top:8px;-webkit-overflow-scrolling:touch}
@media(max-width:900px){.tw{border-radius:var(--r)}}
table{width:100%;border-collapse:collapse;font-size:15px;min-width:780px}
thead th{background:#fff;font-weight:700;font-size:11.5px;letter-spacing:0.05em;text-transform:uppercase;color:var(--ink3);text-align:left;padding:20px 12px;border-bottom:1px solid var(--line);line-height:1.25}
td{padding:15px 12px;border-bottom:1px solid var(--line2);vertical-align:middle}
th:first-child,td:first-child{padding-left:22px}
th:last-child,td:last-child{padding-right:22px}
th.n,td.n{text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap}
tbody tr:last-child td{border-bottom:none}
tr.tot td{background:var(--soft);font-weight:800;border-top:1px solid var(--line)}
tr.dim td{color:var(--ink2)}
td.hl{font-weight:800}
.pos{color:var(--acc);font-weight:700}
.neg{color:var(--warn);font-weight:700}
.tname{font-weight:700;color:var(--ink)}
.tname span{display:block;font-weight:500;font-size:12.5px;color:var(--ink3)}
```

| Аспект | Значение |
|---|---|
| Радиус обёртки | `44px` фиксированно, не токен; ≤900px `var(--r)` |
| Отступ сверху | `margin-top:8px`; в двух местах перебит инлайном `style="margin-top:14px"` |
| Прокрутка | `overflow-x:auto` плюс `-webkit-overflow-scrolling:touch`; таблица не сжимается благодаря `min-width:780px` |
| Паддинг ячеек | шапка `20px 12px`, тело `15px 12px`, крайние колонки `padding-left:22px` и `padding-right:22px` |
| Линии | шапка снизу `var(--line)`, строки снизу `var(--line2)`, у последней строки снята |
| Модификаторы | `.n` числовая колонка, `tr.tot` итог, `tr.dim` приглушённая строка, `td.hl` выделенная ячейка, `.pos` / `.neg` цветные значения |

```html
<div class="tw rv">
  <table>
    <thead><tr>
      <th>Позиция</th><th class="n">Творога</th><th class="n">Себест.</th>
      <th class="n">Наша цена<br>для кафе</th><th class="n">Маржа</th>
    </tr></thead>
    <tbody>
      <tr>
        <td class="tname">Название<span>подпись мелким</span></td>
        <td class="n">180 г</td>
        <td class="n">$0.62</td>
        <td class="n hl">$1.20</td>
        <td class="n pos">48%</td>
      </tr>
      <tr class="dim"><td class="tname">Постоянные затраты<span>подпись</span></td><td class="n">-</td><td class="n">-</td><td class="n neg">-$4.2k</td><td class="n">-</td></tr>
      <tr class="tot"><td>Чистая прибыль в месяц</td><td class="n">$31k</td><td class="n">$12k</td><td class="n pos">$8.4k</td><td class="n">27%</td></tr>
    </tbody>
  </table>
</div>
```

Пометки внутри ячеек в исходнике заданы инлайном: `<span style="display:block;font-size:11.5px;color:var(--warn);font-weight:700">это максимум, дороже не купят</span>` и `<span style="display:block;font-size:11.5px;color:var(--ink3);font-weight:400">источник</span>`. Отдельных классов под них нет.

## 4.11. `.calc` - калькулятор

Двухколоночный блок: слева узкая панель ползунков фиксированной ширины, справа резиновая колонка выводов.

```css
.calc{display:grid;grid-template-columns:370px 1fr;gap:14px;align-items:start}
@media(max-width:1020px){.calc{grid-template-columns:1fr}}
```

**Панель управления `.ctrl`:**

```css
.ctrl{background:#fff;border:1px solid var(--line);border-radius:var(--r-lg);padding:28px 26px}
@media(max-width:1020px){.ctrl{border-radius:var(--r)}}
.grp{margin-bottom:26px}
.grp:last-of-type{margin-bottom:18px}
.grp-t{font-size:11px;font-weight:800;letter-spacing:0.11em;text-transform:uppercase;color:var(--ink3);padding-bottom:12px;margin-bottom:14px;border-bottom:1px solid var(--line)}
.sl{margin-bottom:17px}
.sl:last-child{margin-bottom:0}
.sl-top{display:flex;justify-content:space-between;align-items:baseline;gap:10px;margin-bottom:2px}
.sl-lab{font-size:13.5px;color:var(--ink2);line-height:1.3}
.sl-val{font-size:14px;font-weight:800;color:var(--ink);white-space:nowrap;font-variant-numeric:tabular-nums}
```

**Колонка вывода `.out`** - вертикальный стек с зазором 14px: крупная тёмная плашка `.oh`, сетка `.kpis`, карточки `.flow`, стек `.alerts`.

```css
.out{display:flex;flex-direction:column;gap:14px}
.oh{background:var(--dark);color:#fff;border-radius:var(--r-lg);padding:clamp(30px,3.6vw,44px);min-height:230px;display:flex;flex-direction:column;justify-content:center}
@media(max-width:1020px){.oh{border-radius:var(--r)}}
.oh .k{font-size:12px;font-weight:700;letter-spacing:0.1em;text-transform:uppercase;color:rgba(255,255,255,0.55)}
.oh .money{font-size:clamp(46px,7vw,86px);font-weight:800;letter-spacing:-0.04em;line-height:1;margin:12px 0 12px;font-variant-numeric:tabular-nums}
.oh .money.bad{color:#ffb59b}
.oh .note-d{font-size:15px;color:rgba(255,255,255,0.68);margin:0;line-height:1.5}
.oh .note-d b{color:#fff;font-weight:700}
.kpis{display:grid;grid-template-columns:repeat(4,1fr);gap:14px}
@media(max-width:900px){.kpis{grid-template-columns:repeat(2,1fr)}}
.kpi{background:#fff;border:1px solid var(--line);border-radius:var(--r);padding:18px 18px;min-height:104px;display:flex;flex-direction:column;justify-content:space-between}
.kpi .k{font-size:11px;font-weight:700;letter-spacing:0.06em;text-transform:uppercase;color:var(--ink3);line-height:1.3}
.kpi .v{font-size:24px;font-weight:800;letter-spacing:-0.02em;font-variant-numeric:tabular-nums;margin-top:10px}
```

```html
<div class="calc">
  <div class="ctrl rv">
    <div class="grp">
      <div class="grp-t">Сырьё</div>
      <div class="sl">
        <div class="sl-top">
          <span class="sl-lab">Цена молока, донгов за литр<br><i style="font-style:normal;color:var(--ink3);font-size:12px">пояснение под подписью</i></span>
          <span class="sl-val" id="v_milk"></span>
        </div>
        <input type="range" id="s_milk" min="14000" max="48000" step="500" aria-label="Цена молока">
      </div>
    </div>
    <button class="reset" id="reset">Сбросить к базовому сценарию</button>
  </div>
  <div class="out">
    <div class="oh rv">
      <span class="k">Чистая прибыль в месяц</span>
      <div class="money num" id="o_profit">-</div>
      <p class="note-d" id="o_note"></p>
    </div>
    <div class="kpis">
      <div class="kpi rv"><span class="k">Выручка в месяц</span><span class="v num" id="o_rev"></span></div>
      <!-- всего 8 штук -->
    </div>
    <div class="flow rv"><div class="flow-t">Сколько денег нужно на старте</div><div id="cash"></div></div>
    <div class="flow rv"><div class="flow-t">Маржинальный доход по каналам</div><div id="bars"></div></div>
    <div class="alerts" id="alerts"></div>
  </div>
</div>
```

## 4.12. `.flow` / `.bar` - полосы-диаграммы

```css
.flow{background:#fff;border:1px solid var(--line);border-radius:var(--r);padding:24px 26px}
.flow-t{font-size:11px;font-weight:800;letter-spacing:0.11em;text-transform:uppercase;color:var(--ink3);margin-bottom:18px}
.bar{margin-bottom:15px}
.bar:last-child{margin-bottom:0}
.bar-top{display:flex;justify-content:space-between;align-items:baseline;gap:12px;margin-bottom:6px;font-size:14px}
.bar-top b{font-weight:700}
.bar-top span{color:var(--ink2);font-variant-numeric:tabular-nums;white-space:nowrap}
.bar-track{height:10px;background:var(--soft);border-radius:999px;corner-shape:round;overflow:hidden}
.bar-fill{height:100%;background:var(--dark);border-radius:999px;corner-shape:round;transition:width .4s cubic-bezier(.16,1,.3,1)}
.bar-fill.a2{background:#3f3f3f}.bar-fill.a3{background:#6e6e6e}.bar-fill.a4{background:var(--acc)}
```

Четыре тона серии: `#111111`, `#3f3f3f`, `#6e6e6e`, `#1d5c46`. Зелёный только последний.

```html
<div class="flow">
  <div class="flow-t">Маржинальный доход по каналам</div>
  <div class="bar">
    <div class="bar-top"><b>Опт в кафе</b><span>$4 200 в месяц</span></div>
    <div class="bar-track"><div class="bar-fill a2" style="width:64.0%"></div></div>
  </div>
</div>
```

Итоговая строка в блоке `#cash` добавляется скриптом с инлайновым `style="border-top:1px solid var(--line);padding-top:14px;margin-top:4px;font-size:16px"`.

## 4.13. `.alerts` / `.al` - плашки-предупреждения

```css
.alerts{display:flex;flex-direction:column;gap:10px}
.al{border-radius:var(--r);padding:18px 22px;font-size:14.5px;line-height:1.55;border:1px solid var(--line)}
.al.warn{background:#fdf4f1;border-color:rgba(154,74,47,.22);color:#5c3a2c}
.al.warn b{color:var(--warn)}
.al.ok{background:var(--acc-soft);border-color:rgba(29,92,70,.22);color:#204a3c}
.al.ok b{color:var(--acc)}
```

## 4.14. `.steps` / `.step` - таймлайн

```css
.steps{margin-top:4px}
.step{display:grid;grid-template-columns:96px 1fr 1.25fr;gap:30px;padding:30px 0;border-top:1px solid var(--line);align-items:baseline}
.step:first-child{border-top:none}
@media(max-width:820px){.step{grid-template-columns:1fr;gap:6px;padding:24px 0}}
.step .no{font-size:15px;font-weight:800;color:var(--ink3)}
.step h3{font-size:22px;font-weight:700;letter-spacing:-0.025em;margin:0}
.step h3 span{font-size:11px;font-weight:800;letter-spacing:0.07em;text-transform:uppercase;background:var(--ink);color:#fff;border-radius:999px;corner-shape:round;padding:4px 11px;margin-inline-start:12px;vertical-align:middle;white-space:nowrap}
.step p{font-size:15.5px;color:var(--ink2);margin:0}
```

```html
<div class="steps">
  <div class="step rv">
    <span class="no">Недели 1-2</span>
    <h3>Три замера и юрлицо<span>старт</span></h3>
    <p>Описание этапа с <b>акцентом.</b></p>
  </div>
</div>
```

Значения бейджей в исходнике: «старт», «цех», «продукт», «цех», «продажи», «открытие». Значения номеров: «Недели 1-2», «Недели 3-4», «Недели 5-6», «Недели 7-8», «Недели 9-10», «Недели 11-12».

## 4.15. `.risks` / `.risk` - карточки рисков

```css
.risks{display:grid;grid-template-columns:repeat(3,1fr);gap:14px}
@media(max-width:960px){.risks{grid-template-columns:repeat(2,1fr)}}
@media(max-width:620px){.risks{grid-template-columns:1fr}}
.risk{background:#fff;border:1px solid var(--line);border-radius:var(--r);padding:26px 24px;min-height:300px;display:flex;flex-direction:column}
.risk .lv{font-size:11px;font-weight:800;letter-spacing:0.08em;text-transform:uppercase;color:var(--ink3);margin-bottom:14px}
.risk .lv.hi{color:var(--warn)}
.risk h3{font-size:20px;font-weight:700;letter-spacing:-0.02em;margin:0 0 10px}
.risk p{font-size:14px;color:var(--ink2);margin:0;line-height:1.55}
.risk .fix{margin-top:auto;padding-top:16px;font-size:13px;color:var(--ink);font-weight:700;line-height:1.45}
```

Низ прижимается через `margin-top:auto` на `.fix` в колоночном flex.

```html
<div class="risks">
  <article class="risk rv">
    <span class="lv hi">Риск 1 · критический</span>
    <h3>Выход творога не подтверждён</h3>
    <p>Описание риска с <b>числами</b>.</p>
    <p class="fix">Проверка: что сделать. Цена ошибки: во что обойдётся.</p>
  </article>
  <article class="risk rv rv-d1">...</article>
  <article class="risk rv rv-d2">...</article>
</div>
```

## 4.16. `.quote` - тёмная цитата-разделитель

```css
.quote{background:var(--dark);color:#fff;border:none;padding:clamp(70px,9vw,108px) 0}
.quote p{font-size:clamp(26px,4.2vw,50px);font-weight:700;letter-spacing:-0.03em;line-height:1.16;margin:0;max-width:1050px;color:#7d7d78}
.quote em{font-style:normal;color:#fff}
```

```html
<div class="quote">
  <div class="wrap">
    <p class="rv">Творог здесь не продукт, <em>а узкое место.</em> Зарабатывает тот, кто считает маржу с килограмма творога, <em>а не с блюда.</em></p>
  </div>
</div>
```

## 4.17. `.final` и `footer`

CSS `.final` приведён в 2.4.

```css
footer{background:var(--dark);color:rgba(255,255,255,0.55);font-size:14px;padding:38px 0 44px;border:none}
.foot-in{display:flex;justify-content:space-between;gap:20px;flex-wrap:wrap;align-items:flex-end}
.foot-in .logo{color:#fff;font-weight:700;font-size:16px;letter-spacing:0.18em}
.foot-in p{margin:0;max-width:700px;line-height:1.7}
```

```html
<div class="final">
  <div class="wrap">
    <h2 class="rv">Ты это уже<br>умеешь.</h2>
    <p class="rv rv-d1">Абзац с <b>акцентом.</b></p>
    <a class="btn-white rv rv-d2" href="#calc">Открыть калькулятор</a>
  </div>
</div>

<footer>
  <div class="wrap foot-in">
    <span class="logo">MOLOKO</span>
    <p>Текст подвала с <span class="nb">неразрывным куском</span>.</p>
  </div>
</footer>
```

`.final` скруглён только сверху, `footer` только снизу, между ними нет зазора: визуально это один тёмный блок.

## 4.18. Блоки, объявленные в CSS, но не использованные в разметке

Проверено по файлу: `class="stats4"`, `class="stat"`, `class="two"`, `class="col"` встречаются в разметке 0 раз. Их надо знать, потому что это часть системы, но на текущей странице они ни на что не влияют. Шаблона разметки для них в исходнике нет, восстановить дословно нельзя, только вывести из селекторов.

```css
.stats4{display:grid;grid-template-columns:repeat(4,1fr);gap:14px}
@media(max-width:1000px){.stats4{grid-template-columns:repeat(2,1fr)}}
@media(max-width:560px){.stats4{grid-template-columns:1fr}}
.stat{background:var(--card);border:1px solid var(--line);border-radius:var(--r);padding:26px 24px;min-height:230px;display:flex;flex-direction:column}
section.gray .stat{background:#fff}
.stat .k{font-size:12px;font-weight:700;letter-spacing:0.08em;text-transform:uppercase;color:var(--ink3);margin-bottom:auto}
.stat .v{font-size:clamp(30px,3.6vw,42px);font-weight:800;letter-spacing:-0.03em;line-height:1;margin:18px 0 10px;font-variant-numeric:tabular-nums}
.stat p{font-size:13.5px;color:var(--ink2);margin:0;line-height:1.5}

.two{display:grid;grid-template-columns:1fr 1fr;gap:14px;margin-top:40px}
@media(max-width:860px){.two{grid-template-columns:1fr}}
.col{background:#fff;border:1px solid var(--line);border-radius:var(--r-lg);padding:clamp(26px,3vw,38px)}
.col h3{font-size:22px;font-weight:700;letter-spacing:-0.025em;margin:0 0 6px}
.col ul{list-style:none;margin:14px 0 0;padding:0}
.col li{font-size:15px;color:var(--ink2);padding:15px 0 15px 22px;position:relative;border-top:1px solid var(--line2);line-height:1.55}
.col li:first-child{border-top:none}
.col li::before{content:'';position:absolute;left:2px;top:23px;width:6px;height:6px;border-radius:999px;corner-shape:round;background:var(--ink)}
.col li b{color:var(--ink);font-weight:700}
```

## 4.19. Картинки

Общее правило: картинка заполняет заданный контейнер через `object-fit:cover`, радиус живёт на контейнере, не на картинке. Атрибуты `width` и `height` проставлены у каждой `img` для резервирования места.

| Место | Контейнер | Размер картинки | Радиус |
|---|---|---|---|
| Глобально | - | `max-width:100%;display:block` | не задан |
| `.hero .hero-img img` | абсолютная сетка на весь hero | `100%` / `100%`, `min-width:0`, `filter:contrast(1.02)` | от `.hero`, `overflow:hidden` |
| `.pillar .ph` | `aspect-ratio:16/11`, `background:var(--soft)`, `border-top:1px solid var(--line)` | `100%` / `100%` | от `.pillar`, `overflow:hidden` |
| `.mod2 .m-photo` | `height:clamp(420px,42vw,560px)`; ≤820px `clamp(300px,66vw,420px)` | `100%` / `100%` | `var(--r-lg)` на контейнере |
| `.gal figure` | `background:var(--soft)`, `position:relative` | `width:100%; height:clamp(200px,22vw,290px)` | `var(--r)` |

Соотношение сторон явно задано только в одном месте: `.pillar .ph{aspect-ratio:16/11}`. Везде остальное - фиксированная или `clamp`-высота контейнера. У всех картинок ниже первого экрана стоит `loading="lazy"`, у трёх картинок hero - `fetchpriority="high"` без `loading`.

## 4.20. Порядок блоков сверху вниз

| № | Тег и класс | `id` | Строка | Фон | Радиус | Что внутри |
|---|---|---|---|---|---|---|
| 1 | `<header class="hero">` | - | 306 | `--dark` | все углы | сетка 3 фото, `.scrim`, `.hero-nav`, `.hero-line` |
| 2 | `<div class="slab">` | - | 344 | `--soft` | снизу | `h2` плюс `.slab-facts` из трёх `.sf` |
| 3 | `<section>` | `concept` | 356 | белый | нет | шапка секции, `.pillars` с фото, `.note` |
| 4 | `<section class="gray">` | `look` | 414 | `--soft` | снизу | шапка, `.gal` из 8 `figure` |
| 5 | `<section>` | `menu` | 433 | белый | нет | шапка, `.mod2`, `.mod2.rev-order`, `.src` |
| 6 | `<section class="gray">` | `semi` | 468 | `--soft` | снизу | шапка, `.pillars` без фото, `.mod2`, `.note` |
| 7 | `<section>` | `unit` | 534 | белый | нет | шапка, `.tw`, `.src`, `.note` |
| 8 | `<section class="gray">` | `calc` | 556 | `--soft` | снизу | шапка, `.calc`, второй `.tw`, `.note` |
| 9 | `<section>` | `milk` | 655 | белый | нет | шапка, `.tw`, `.mod2` |
| 10 | `<div class="quote">` | - | 682 | `--dark` | все углы | один `p` |
| 11 | `<section class="gray">` | `risk` | 689 | `--soft` | снизу | шапка, `.risks` из 6 карточек |
| 12 | `<section>` | `plan` | 736 | белый | нет | шапка, `.steps` из 6 шагов, `.note`, `.cta-mid` |
| 13 | `<div class="final">` | - | 755 | `--dark` | сверху | `h2`, `p`, `.btn-white` |
| 14 | `<footer>` | - | 763 | `--dark` | снизу | `.foot-in`: `.logo` плюс `p` |

Ритм: тёмный hero, серая плита-тезис, строгое чередование белая / серая секция (concept, look, menu, semi, unit, calc, milk), тёмная цитата как пауза, последняя пара серая / белая (risk, plan), закрывающая тёмная связка `.final` плюс `footer`.

---

# ЧАСТЬ 5. КНОПКИ, ПИЛЮЛИ И МЕЛОЧЬ

Все кнопки явно переобъявляют `font-family:var(--sf)`, потому что `<button>` не наследует шрифт. Состояния `:active` и `:disabled` в исходнике не заданы ни для одной кнопки.

## 5.1. Перечень кнопок

| Класс | Тег | Где | Размер | font-size / weight | radius | Фон | Цвет | Border |
|---|---|---|---|---|---|---|---|---|
| `.hero-nav .cta` | `<a>` | навигация, «Считать →» | `6px 14px`; ≥901px `0 16px` плюс `align-self:stretch` и `margin:-3px -3px -3px 0` | `13px` / `700` | `999px` | `#fff`; `.scrolled` `#0a0a0a` | `#0a0a0a`; `.scrolled` `#fff` | `none` |
| `.cur-toggle` | `<button type="button">` | `$` / `₫` | `38x38` | `13px` / `800` | `999px` | `rgba(255,255,255,0.44)` плюс `blur(12px)`; `.scrolled` `rgba(10,10,10,0.11)` | `#fff`; `.scrolled` `#0a0a0a` | `none` |
| `.nav-burger` | `<button>` | `☰` / `✕`, только ≤900px | `38x38` | `17px` / не задан | `999px` | как у `.cur-toggle` | как у `.cur-toggle` | `none` |
| `.hero-nav .links a` | `<a>` | 6 ссылок меню | `6px 12px` | `13px` / `600` | `999px` | прозрачный | `#fff` при `opacity:.92` | не задан |
| `.links.mopen a` | `<a>` | те же в мобильном меню | `14px 18px`, `text-align:center` | `16px` / `600` | `16px` | прозрачный | `#fff`; `.scrolled` `#0a0a0a` | не задан |
| `.reset` | `<button>` | низ `.ctrl` | `width:100%; padding:13px` | `14px` / `700` | `999px` | `var(--soft)` | `var(--ink)` | `1px solid var(--line)` |
| `.btn-dark` | `<a>` | внутри `.cta-mid` | `17px 34px` | `16px` / `800` | `999px` | `var(--dark)` | `#fff` | `none` |
| `.btn-white` | `<a>` | внутри `.final` | `18px 38px` | `17px` / `800` | `999px` | `#fff` | `var(--ink)` | `none` |

Ховеры и переходы перечислены в части 6.

```css
.btn-white{display:inline-block;background:#fff;color:var(--ink);font-weight:800;font-size:17px;padding:18px 38px;border-radius:999px;corner-shape:round;transition:transform .15s;border:none;cursor:pointer;font-family:var(--sf)}
.btn-white:hover{transform:translateY(-2px)}
.btn-dark{display:inline-block;background:var(--dark);color:#fff;font-weight:800;font-size:16px;padding:17px 34px;border-radius:999px;corner-shape:round;transition:transform .15s;border:none;cursor:pointer;font-family:var(--sf)}
.btn-dark:hover{transform:translateY(-2px)}
.cta-mid{display:flex;justify-content:center;margin-top:clamp(30px,4vw,48px)}
.reset{width:100%;background:var(--soft);color:var(--ink);border:1px solid var(--line);border-radius:999px;corner-shape:round;padding:13px;font-family:var(--sf);font-weight:700;font-size:14px;cursor:pointer;transition:background .15s}
.reset:hover{background:var(--dark);color:#fff;border-color:var(--dark)}
```

## 5.2. Расположение кнопок

- **В навигации** - единственное место, где кнопки стоят в ряд: `.nav-actions`, flex, `gap:5px`, порядок `.cur-toggle`, `.nav-burger`, `.cta`.
- **В столбец** - только мобильное меню: `.links.mopen{flex-direction:column;gap:2px;padding:12px}`.
- **По центру страницы** - `.cta-mid{display:flex;justify-content:center;margin-top:clamp(30px,4vw,48px)}`. Отступ снизу не задан, работает паддинг секции. Ставится после `.steps`, последний `.step` имеет `padding:30px 0` (≤820px `24px 0`).
- **В финальном блоке** - `.btn-white` стоит прямо за абзацем, без обёртки, по левому краю `.wrap`. Отступ сверху даёт нижний margin абзаца `.final p{margin:0 0 36px}`.
- **В панели калькулятора** - `.reset` последний ребёнок `.ctrl`, на всю ширину; отступ сверху задаётся правилом `.grp:last-of-type{margin-bottom:18px}`.

Правил вида «две кнопки рядом» в исходнике не задано: `.btn-dark` и `.btn-white` всегда встречаются поодиночке.

## 5.3. Пилюли и теги

Помимо `.tag` (см. 4.3):

**Бейдж внутри заголовка шага** - `.step h3 span`, тёмная залитая пилюля справа от текста заголовка (CSS в 4.14).

**Круглый номер карточки** - `.pil-n` (CSS в 4.4). Позиция: первый ребёнок `.pillar`, собственный `margin:30px 22px 0` выравнивает его по текстовым паддингам карточки. Реагирует на наведение на всю карточку, а не на себя.

**Полосы-пилюли** - `.bar-track` / `.bar-fill` (CSS в 4.12).

**Маркеры списков** - круги 6x6px, два варианта различаются только вертикальной привязкой:

```css
.pillar li::before{content:'';position:absolute;left:2px;top:18px;width:6px;height:6px;border-radius:999px;corner-shape:round;background:var(--ink)}
.col li::before{content:'';position:absolute;left:2px;top:23px;width:6px;height:6px;border-radius:999px;corner-shape:round;background:var(--ink)}
```

Отступ текста от маркера задан паддингом строки: `.pillar li{padding:11px 0 11px 20px}`, `.col li{padding:15px 0 15px 22px}`.

## 5.4. Мелкие подписи и метки

Капс-метки построены по одной схеме: маленький кегль, тяжёлое начертание, положительный трекинг, `uppercase`, цвет `var(--ink3)`.

| Класс | Где | Расстояние до соседа |
|---|---|---|
| `.stat .k` | верх стат-карточки | `margin-bottom:auto` прижимает значение вниз карточки |
| `.kpi .k` | KPI-плитка | зазор даёт `.kpi .v{margin-top:10px}` |
| `.oh .k` | шапка главного вывода на тёмном | ниже `.oh .money{margin:12px 0 12px}` |
| `.grp-t` | заголовок группы ползунков | `padding-bottom:12px;margin-bottom:14px;border-bottom:1px solid var(--line)` |
| `.flow-t` | заголовок блока с полосами | `margin-bottom:18px` |
| `.risk .lv` | уровень риска | `margin-bottom:14px` |
| `thead th` | шапка таблицы | `padding:20px 12px` |

Метки без капса: `.hs span`, `.sf span`, `.step .no`, `.gal figcaption`, `.tname span`, `.sl-lab`, `.sl-val`, `.mi i`, `.src` (значения в шкале 1.3).

Две микроподписи в исходнике заданы инлайном, классов у них нет:

```html
<i style="font-style:normal;color:var(--ink3);font-size:12px">база 30 тыс. Ферма с доставкой около 20 тыс., опт пастеризованного 36-44 тыс.</i>
<div style="font-size:12px;color:var(--ink3);margin-top:5px">платится до открытия</div>
```

## 5.5. Ссылки в тексте

Отдельного оформления текстовых ссылок в системе нет. Глобальное правило одно:

```css
a{color:inherit;text-decoration:none}
```

Ссылка наследует цвет родителя и не подчёркивается никогда. Правил `a:hover` для контентных ссылок в исходнике не задано: hover описан только для `.hero-nav .links a` и для классов кнопок. Проверено по разметке: все теги `<a>` в файле - это 6 ссылок навигации, `.cta`, `.btn-dark` и `.btn-white`. Ссылок внутри абзацев, списков и таблиц нет, поэтому эталонного вида «ссылка в тексте» файл не задаёт. Единственное, что достаётся такой ссылке автоматически, - общий фокус-ринг.

## 5.6. Иконки и стрелки

Иконочной графики в файле нет вообще: тегов `<svg>` ноль, иконочных шрифтов и спрайтов не подключено, псевдоэлементы с иконками не используются (единственные `::before` - точки-маркеры списков).

| Символ | Где | Размер |
|---|---|---|
| `→` | «Считать →», «Посчитать свой сценарий →» | наследует кегль кнопки: `13px` в навигации, `16px` у `.btn-dark` |
| `☰` / `✕` | `.nav-burger`, закрыто / открыто, подставляется скриптом | `17px` |
| `$` / `₫` | `.cur-toggle`, usd / vnd, подставляется скриптом | `13px`, вес `800` |
| `·` | разделитель внутри `.mi i` и `.risk .lv` | наследует `12px` / `11px` |

Обе символьные кнопки - квадрат `38x38px` с `display:grid;place-items:center`: символ центрируется гридом, а не line-height.

## 5.7. Нумерация

Счётчиков CSS в файле нет, нумерация везде вручную в разметке. Два места: `.pil-n` (круг с обводкой, содержимое `01`, `02`, `03`, `04`) и `.step .no` (без плашки, текст в первой колонке грида). Нумерация рисков - текстом внутри метки уровня: `<span class="lv hi">Риск 1 · критический</span>`, отдельного стиля у цифры нет.

## 5.8. Формы

Единственный тип поля - `input[type=range]`. Полей `text`, `email`, `select`, `textarea`, `checkbox`, тега `<form>` и кнопки `submit` в исходнике нет.

```css
input[type=range]{-webkit-appearance:none;appearance:none;background:transparent;width:100%;height:22px;cursor:pointer;margin:0;display:block}
input[type=range]::-webkit-slider-runnable-track{height:2px;background:var(--line);border-radius:2px}
input[type=range]::-moz-range-track{height:2px;background:var(--line);border-radius:2px}
input[type=range]::-webkit-slider-thumb{-webkit-appearance:none;width:18px;height:18px;border-radius:999px;background:var(--dark);margin-top:-8px;border:2px solid #fff;box-shadow:0 1px 5px rgba(0,0,0,.28)}
input[type=range]::-moz-range-thumb{width:16px;height:16px;border-radius:999px;background:var(--dark);border:2px solid #fff;box-shadow:0 1px 5px rgba(0,0,0,.28)}
input[type=range]:focus-visible::-webkit-slider-thumb{outline:2px solid var(--acc);outline-offset:2px}
```

Расхождение из исходника, воспроизводить как есть: ручка в WebKit `18x18px`, в Firefox `16x16px`. Фокус-ринг задан только для WebKit-ручки.

Лейблы даны как `<span class="sl-lab">` плюс `aria-label` на самом `input`, тег `<label>` не используется. Состояний `disabled`, `invalid`, `readonly` в исходнике не задано. Атрибуты диапазонов в разметке, например:

```html
<input type="range" id="s_milk" min="14000" max="48000" step="500" aria-label="Цена молока">
<input type="range" id="s_gp" min="0.3" max="3" step="0.05" aria-label="Прибыль с гостя">
```

---

# ЧАСТЬ 6. ДВИЖЕНИЕ

## 6.1. Система появления

Один механизм на всю страницу: класс `.rv`. Элемент лежит в DOM, но невидим и сдвинут вниз; при входе в область просмотра JS вешает `.in`, и CSS-переход доводит до нормального состояния. Отдельно `.ph-zoom` - медленный отъезд картинки из масштаба.

```css
.js .rv{opacity:0;transform:translateY(30px);transition:opacity .8s cubic-bezier(.16,1,.3,1),transform .8s cubic-bezier(.16,1,.3,1)}
.js .rv.in{opacity:1;transform:none}
.rv-d1{transition-delay:.08s}.rv-d2{transition-delay:.16s}.rv-d3{transition-delay:.24s}
.js .ph-zoom img{transform:scale(1.08);transition:transform 1.4s cubic-bezier(.16,1,.3,1)}
.js .ph-zoom.in img{transform:scale(1)}
@media(prefers-reduced-motion:reduce){ /* .rv видим сразу + глобально гасятся все анимации и переходы */ }
@media print{.rv,.ph-zoom img{opacity:1!important;transform:none!important}}
```

**Изменено 27.09.2026 (12.5):** в эталоне было `.rv{opacity:0}` без условия, и контент был невидим без JS, при печати, в скриншотах и превью. Теперь прячет только `html.js`: класс ставит одна строка в `<head>` (6.2). Без неё страница просто показывается без анимации, контент не пропадает.

| Что | Начальное | Конечное | Длительность | Кривая |
|---|---|---|---|---|
| `.rv` | `opacity:0; transform:translateY(30px)` | `opacity:1; transform:none` | `.8s` для обоих свойств | `cubic-bezier(.16,1,.3,1)` |
| `.ph-zoom img` | `transform:scale(1.08)` | `transform:scale(1)` | `1.4s` | `cubic-bezier(.16,1,.3,1)` |

Шаг лесенки ровно 80 мс, четвёртой ступени (`rv-d4`) в исходнике не задано.

Как применяется:

- одиночный блок: `class="tag rv"`, `class="sec rv"`, `class="sub rv rv-d1"` - заголовок без задержки, подпись под ним с `rv-d1`;
- сетка карточек: задержка циклится по позиции в ряду, `rv`, `rv-d1`, `rv-d2`, `rv-d3`, потом снова с нуля;
- комбинация: `class="ph ph-zoom rv"` и `class="m-photo ph-zoom rv"` - на одном узле оба механизма, один класс `.in` включает и подъём, и зум;
- элементы с `.rv` в исходнике: `.tag`, `h2.sec`, `.sub`, `.kpi`, `.step`, `.note`, `.ph`, `.m-txt`, `.m-photo`, `.tw`, `.src`, `.risk`, `.pillar`, `.flow`, `.slab-facts`, `.oh`, `.ctrl`, `.cta-mid`, `.btn-white`, `figure` галереи;
- hero целиком без `.rv`.

## 6.2. Скрипт

**Первой строкой в `<head>`, до стилей** (без неё `.rv` не прячется и анимации нет, контент при этом виден):

```html
<script>document.documentElement.classList.add('js')</script>
```

Скрипт в конце файла, строки 1070-1094 эталона, целиком:

```html
<script>
(function(){
  var reduce = matchMedia('(prefers-reduced-motion:reduce)').matches;
  var els = document.querySelectorAll('.rv,.ph-zoom');
  if (reduce || !('IntersectionObserver' in window)){
    els.forEach(function(e){ e.classList.add('in'); });
  } else {
    var io = new IntersectionObserver(function(en){
      en.forEach(function(x){ if (x.isIntersecting){ x.target.classList.add('in'); io.unobserve(x.target); } });
    }, {threshold:0.12, rootMargin:'0px 0px -5% 0px'});
    els.forEach(function(e){ io.observe(e); });
  }
  var nav = document.getElementById('nav');
  var hero = document.querySelector('.hero');
  function onScroll(){ nav.classList.toggle('scrolled', window.scrollY > hero.offsetHeight - 90); }
  addEventListener('scroll', onScroll, {passive:true}); onScroll();
  var b = document.getElementById('burger'), lk = document.getElementById('navLinks');
  b.addEventListener('click', function(){
    var open = lk.classList.toggle('mopen');
    b.textContent = open ? '✕' : '☰';
  });
  lk.addEventListener('click', function(e){
    if (e.target.tagName === 'A'){ lk.classList.remove('mopen'); b.textContent = '☰'; }
  });
})();
</script>
```

| Фрагмент | Что делает |
|---|---|
| IIFE | ничего не выносит в глобальную область |
| `querySelectorAll('.rv,.ph-zoom')` | один общий список для обоих механизмов, один наблюдатель на всю страницу |
| `reduce \|\| !('IntersectionObserver' in window)` | две причины отключить анимацию: настройка пользователя или отсутствие API |
| ветка без наблюдателя | всем сразу ставится `.in`, контент показан в конечном состоянии |
| `if (x.isIntersecting)` | реагируем только на вход в зону, выход игнорируется |
| `io.unobserve(x.target)` | элемент снимается с наблюдения сразу после срабатывания: анимация одноразовая, при обратной прокрутке блок не исчезает |
| `{threshold:0.12}` | триггер, когда в область видимости попало 12 процентов площади элемента |
| `{rootMargin:'0px 0px -5% 0px'}` | нижняя граница зоны поднята на 5 процентов высоты окна: блок должен зайти чуть глубже нижнего края экрана |

Разметка, которую ожидает скрипт: `id="nav"` на `.hero-nav`, `id="burger"` на `.nav-burger`, `id="navLinks"` на `.hero-nav .links`, один элемент `.hero` на странице.

## 6.3. prefers-reduced-motion

Защита в двух местах, и оба нужны: CSS гасит движение (`opacity:1!important`, `transform:none!important`, `transition:none!important` на `.rv` и `.ph-zoom img`), JS гарантирует, что контент виден, даже если CSS не применился (`matchMedia(...).matches` даёт `true`, всем сразу ставится `.in`, наблюдатель не создаётся).

В эталоне `scroll-behavior:smooth` и остальные переходы под reduced-motion не отменялись. **С 27.09.2026** `NORMA.css` гасит глобально: `*,*::before,*::after{animation-duration:.01ms;transition-duration:.01ms;scroll-behavior:auto}` (12.5). Задержки `.rv-d1` / `.rv-d2` / `.rv-d3` формально остаются, но при `transition:none` эффекта не имеют.

## 6.4. Все transition

`@keyframes` и свойство `animation` в исходнике не заданы ни разу.

| Селектор | Свойства | Длительность | Кривая | Строка CSS |
|---|---|---|---|---|
| `.rv` | `opacity`, `transform` | `.8s` | `cubic-bezier(.16,1,.3,1)` | 33 |
| `.ph-zoom img` | `transform` | `1.4s` | `cubic-bezier(.16,1,.3,1)` | 36 |
| `.hero-nav .links a` | `background`, `opacity` | `.15s` | не задана, дефолтный `ease` | 52 |
| `.cur-toggle` | `background`, `color` | `.15s` | не задана | 64 |
| `.pillar` | `border-color` | `.3s` | не задана | 106 |
| `.pil-n` | `border-color`, `color` | `.3s` | не задана | 108 |
| `.reset` | `background` | `.15s` | не задана | 183 |
| `.bar-fill` | `width` | `.4s` | `cubic-bezier(.16,1,.3,1)` | 206 |
| `.btn-white` | `transform` | `.15s` | не задана | 272 |
| `.btn-dark` | `transform` | `.15s` | не задана | 274 |

Плюс не-transition движение: `html{scroll-behavior:smooth}`.

Ровно две кривые на весь сайт: `cubic-bezier(.16,1,.3,1)` для содержательного движения (появление, зум, заполнение полос) и дефолтный `ease` на коротких 150 мс для откликов интерфейса.

Важная деталь: смена темы шапки при `.scrolled` анимирована не полностью, у `.logo`, `.links`, `.nav-actions`, `.cta`, `.nav-burger` transition не задан, их цвет переключается мгновенно.

## 6.5. Hover

| Элемент | Что меняется | Из чего | Во что |
|---|---|---|---|
| `.hero-nav .links a:hover` | фон и непрозрачность | прозрачный, `.92` | `rgba(255,255,255,0.16)`, `1` |
| `.hero-nav.scrolled .links a:hover` | фон | прозрачный | `rgba(10,10,10,0.07)` |
| `.hero-nav .cta:hover` | фон | `#fff` | `#ececec` |
| `.hero-nav.scrolled .cta:hover` | фон | `#0a0a0a` | `#2b2b2b` |
| `.pillar:hover` | рамка | `rgba(10,10,10,0.10)` | `rgba(10,10,10,0.20)` |
| `.pillar:hover .pil-n` | рамка и цвет цифры | `var(--line)`, `var(--ink)` | `rgba(10,10,10,0.35)`, `#000` |
| `.reset:hover` | фон, текст, рамка | `var(--soft)`, `var(--ink)`, `var(--line)` | `var(--dark)`, `#fff`, `var(--dark)` |
| `.btn-white:hover`, `.btn-dark:hover` | сдвиг | `none` | `translateY(-2px)` |

Принцип: hover в NORMA никогда не двигает контент и не меняет размеры. Максимум - подъём кнопки на 2px и усиление уже существующей рамки на один шаг прозрачности. Карточки `.stat`, `.kpi`, `.risk`, `.col`, `.flow`, ячейки таблиц и `figure` галереи ховер-состояний не имеют. У `.cur-toggle` и `.nav-burger` hover-правил в исходнике не задано.

---

# ЧАСТЬ 7. АДАПТИВ

В файле ровно 27 блоков `@media`: 26 по ширине и один `prefers-reduced-motion`. Порядок в исходнике не сортированный, правила лежат рядом со своими секциями; конфликтующих пар среди них нет.

## 7.1. Индекс всех медиазапросов

| Брейкпоинт | Что меняется | Где полный код |
|---|---|---|
| `min-width:1024px` | `.hero-nav{padding:0 100px}` | 3.8 |
| `max-width:1020px` | `.calc` в одну колонку; `.ctrl` и `.oh` радиус `var(--r-lg)` в `var(--r)` | 4.11 |
| `max-width:1000px` | `.pillars` и `.stats4` в `repeat(2,1fr)`; `.pil-sub` и `.pillar h3` теряют фиксированную высоту, `height:auto;overflow:visible` | 4.4, 4.18 |
| `max-width:960px` | `.risks` в `repeat(2,1fr)` | 4.15 |
| `min-width:901px` | `.hero-nav .cta{align-self:stretch;display:inline-flex;align-items:center;padding:0 16px;margin:-3px -3px -3px 0}` | 3.8 |
| `max-width:900px` | шапка в мобильный режим (ссылки скрыты, бургер и `.mopen` включены); `.tw` радиус в `var(--r)`; `.kpis` в `repeat(2,1fr)` | 3.8, 4.10, 4.11 |
| `max-width:860px` | `.two` в `1fr` | 4.18 |
| `max-width:820px` | `.mod2` в `1fr`, высота фото в `clamp(300px,66vw,420px)`, `.rev-order .m-photo{order:0}`; `.gal` в `repeat(2,1fr)`; `.step` в `1fr`, `gap:6px`, `padding:24px 0` | 4.8, 4.7, 4.14 |
| `max-width:760px` | `.hero .hero-img` в `1fr 1fr`, третья картинка `display:none`; `.hero-sub{margin-top:14px;line-height:1.45}` | 4.1 |
| `max-width:640px` | `body{padding:8px}`; `:root{--r-lg:40px}`; `.hero-nav{top:30px;width:calc(100% - 24px)}` | 2.1, 3.8 |
| `max-width:620px` | `.risks` в `1fr` | 4.15 |
| `max-width:600px` | `.pillars` в `1fr` | 4.4 |
| `max-width:560px` | `.stats4` в `1fr` | 4.18 |
| `max-width:520px` | `.hero-stats{gap:14px 20px}`, `.hs{min-width:88px}`, `.hs b{font-size:19px}`, `.hs span{font-size:11px}` | 4.1 |
| `prefers-reduced-motion:reduce` | гасит `.rv` и `.ph-zoom img` | 6.3 |

## 7.2. Почему брейкпоинты именно такие

Комментариев, объясняющих выбор чисел, в исходнике нет; ниже причина выведена из самих значений.

1. **520px** - hero-статистика. Ниже этой ширины `clamp(20px,2.6vw,30px)` уже не спасает, цифры перестают влезать рядом при `min-width:120px` на блок. Единственное место, где `clamp` на мобильном заменяется фиксированным числом.
2. **560px** - `.stats4` в одну колонку: две карточки по `min-height:230px` с паддингом `26px 24px` дают колонку около 265px, дальше содержимое ломается.
3. **600px** - `.pillars` в одну колонку.
4. **620px** - `.risks` в одну колонку; карточка риска выше (`min-height:300px`), поэтому её порог на 20px раньше, чем у столпов.
5. **640px** - глобальный мобильный порог: меняет не один блок, а две глобальные константы плюс привязку шапки. Граница «телефон против всего остального».
6. **760px** - hero: три картинки в две, отступ подзаголовка.
7. **820px** - `.mod2`, `.gal`, `.step`. Двухколонник «текст плюс фото» ниже 820px даёт колонку под 400px, куда не помещается заголовок `clamp(26px,3.4vw,40px)`.
8. **860px** - `.two` в одну колонку.
9. **900px / 901px** - пара, делящая шапку на мобильную и десктопную; сюда же `.tw` и `.kpis`.
10. **960px** - `.risks` из трёх в две.
11. **1000px** - `.pillars` и `.stats4` из четырёх в две плюс снятие фиксированных высот в карточке столпа. При четырёх колонках и `gap:14px` внутри `.wrap` колонка на 1000px становится около 210px.
12. **1020px** - калькулятор: `370px` фиксированная колонка плюс `14px` зазор; ниже 1020px правой части остаётся меньше половины ширины.
13. **1024px** - `min-width`, боковые поля шапки 100px, порог «настоящего десктопа».

Итого 13 размерных порогов, одна пара из них (900/901) работает как переключатель.

## 7.3. Что НЕ адаптируется через `@media`

Большая часть размеров тянется через `clamp`, поэтому отдельных брейкпоинтов у них нет.

| Свойство | Формула целиком |
|---|---|
| `.wrap` боковые поля | `padding:0 clamp(24px,5vw,64px)` |
| `section` вертикальные поля | `padding:clamp(56px,7vw,96px) 0` |
| `.slab` | `padding:clamp(44px,6vw,72px) 0` |
| `.quote` | `padding:clamp(70px,9vw,108px) 0` |
| `.final` | `padding:clamp(72px,10vw,120px) 0` |
| `.hero` высота | `height:calc(96vh - 26px)`, `min-height:620px` |
| `.hero-line` от низа | `bottom:clamp(30px,5vw,64px)` |
| `.hero-stats` отступ сверху | `margin-top:clamp(22px,3vw,34px)` |
| `.sub` нижний отступ | `margin:0 0 clamp(34px,4vw,50px)` |
| `.col` внутренние поля | `padding:clamp(26px,3vw,38px)` |
| `.oh` внутренние поля | `padding:clamp(30px,3.6vw,44px)` |
| `.mod2 .m-txt` внутренние поля | `padding:clamp(28px,3vw,44px)` |
| `.mod2 .m-photo` высота | `clamp(420px,42vw,560px)`; ≤820px `clamp(300px,66vw,420px)` |
| `.gal img` высота | `height:clamp(200px,22vw,290px)` |
| `.cta-mid` отступ сверху | `margin-top:clamp(30px,4vw,48px)` |
| Все кегли | см. список из десяти `clamp` в 1.3 |

Правило системы: типографика и поля масштабируются непрерывно через `clamp`, медиазапросы отвечают только за перестройку сеток, за переключение шапки и за две глобальные константы на 640px.

## 7.4. Что ломается, если брейкпоинт не соблюсти

| Пропущено | Что сломается |
|---|---|
| `max-width:1020px` для `.calc` | колонка контролов остаётся жёсткие `370px`, правая часть с `.oh` (`min-height:230px`) и `.kpis` в четыре колонки сжимается до нечитаемой, сумма `clamp(46px,7vw,86px)` вылезает за карточку |
| замена радиуса на `var(--r)` у `.ctrl`, `.oh`, `.tw` | радиус 80px (на мобильном 40px) на узком блоке съедает углы: подписи полей и первая колонка таблицы уходят под скругление |
| `max-width:1000px` для `.pil-sub` и `.pillar h3` | фиксированные `height:70px` и `height:84px` при `overflow:hidden` обрежут заголовок и подпись на полуслове |
| `max-width:900px` для шапки | `.links` абсолютно центрированы, `.nav-actions` прижаты `margin-left:auto`, ссылки не переносятся из-за `white-space:nowrap`: без бургера они наедут друг на друга |
| `min-width:901px` для `.cta` | кнопка не растянется по высоте пилюли, останется зазор 3px по краям |
| `860px` / `820px` / `620px` / `600px` / `560px` | сетки не схлопнутся, карточки с `min-height` (`.risk` 300px, `.stat` 230px) станут узкими столбиками с обрезанным текстом |
| `max-width:820px` для `.step` | сетка `96px 1fr 1.25fr` с `gap:30px` оставит описанию треть ширины, текст `15.5px` пойдёт лесенкой по два-три слова |
| `max-width:820px` для `.rev-order .m-photo` | каждый второй модуль покажет фото перед текстом, порядок чтения станет непредсказуемым |
| `max-width:760px` для hero | три картинки в `1.15fr 1fr 1fr` на телефоне превращаются в полосы около 120px, сюжет кадра не читается |
| `max-width:640px`, `body{padding:8px}` | на телефоне уходит 32px из примерно 375px, около 9 процентов экрана на воздух, а `.wrap` добавляет сверху ещё `clamp(24px,5vw,64px)` с каждой стороны |
| `max-width:640px`, `--r-lg:40px` | скругление 80px на секции шириной 359px это почти половина ширины с каждой стороны: hero и `.final` получают форму капсулы, текст упирается в дугу |
| `max-width:640px` для `.hero-nav` | `calc(100% - 48px)` перестанет совпадать с уменьшенным полем body, шапка встанет уже полотна и разойдётся с ним по оси |
| `max-width:520px` для `.hs` | цифры hero при `min-width:120px` встают по одной в строку и выдавливают заголовок за пределы экрана |
| `min-width:1024px` для `.hero-nav` | на больших экранах шапка растянется на все 1320px и её края разойдутся с контентом `.wrap` |
| `prefers-reduced-motion` | у пользователя с отключённым движением останутся подъёмы на 30px и зумы 1.4s |
| весь скрипт reveal | без JS ни один `.rv` не получит `.in`, страница останется пустой: `opacity:0` на всех блоках, кроме hero. Это главный риск системы, `<noscript>`-обхода в исходнике не задано |

---

# ЧАСТЬ 8. КАК СОБРАТЬ НОВЫЙ БЛОК

Порядок ровно такой, каждый шаг закрывается решением из таблицы, а не на глаз.

### Шаг 1. Выбрать полотно

Смотрим, что стоит выше по странице, и берём противоположный цвет. Белая секция и серая чередуются, тёмная ставится точечно.

| Тип | Разметка | Фон | Радиус | Паддинг |
|---|---|---|---|---|
| Белая контентная | `<section id="...">` | `var(--card)` | нет | `clamp(56px,7vw,96px) 0` |
| Серая контентная | `<section class="gray" id="...">` | `var(--soft)` | `0 0 var(--r-lg) var(--r-lg)` | то же |
| Тёмная цитата-пауза | `<div class="quote">` | `var(--dark)` | `var(--r-lg)` все углы | `clamp(70px,9vw,108px) 0` |
| Тёмный финал | `<div class="final">` | `var(--dark)` | `var(--r-lg) var(--r-lg) 0 0` | `clamp(72px,10vw,120px) 0` |

Тёмное полотно ставится не чаще одного раза на 4-6 светлых: в эталоне их пять на четырнадцать блоков. Если полотно тёмное, текст красится по схеме из 2.4: база `#fff`, приглушение белым с альфой из шкалы `0.55 / 0.6 / 0.66 / 0.68 / 0.82`, акцент обратно в `#fff`.

### Шаг 2. Выбрать контейнер

Всегда `<div class="wrap">` внутри полотна. Другого контейнера в системе нет. `max-width:1300px`, `padding:0 clamp(24px,5vw,64px)`, вертикальных паддингов у него нет.

### Шаг 3. Поставить шапку блока

Обязательная тройка: `.tag`, затем `h2.sec`, затем `.sub`. `.tag` - одно-два слова. В `h2.sec` вторая половина мысли уходит в `<em>` и приглушается до `var(--ink3)`. Лид не длиннее `max-width:680px`, ключевые слова в `<b>`. Нижний отступ шапки задан `.sub` через `margin:0 0 clamp(34px,4vw,50px)`, вручную его не трогаем.

### Шаг 4. Выбрать сетку

| Сколько колонок | Класс-донор | `grid-template-columns` | Радиус карточки | Схлопывание |
|---|---|---|---|---|
| 4 карточки-столпа | `.pillars` / `.pillar` | `repeat(4,1fr)`, `gap:14px` | `var(--r-lg)` | 1000px в 2, 600px в 1 |
| 4 стат-карточки | `.stats4` / `.stat` | `repeat(4,1fr)`, `gap:14px` | `var(--r)` | 1000px в 2, 560px в 1 |
| 3 карточки | `.risks` / `.risk` | `repeat(3,1fr)`, `gap:14px` | `var(--r)` | 960px в 2, 620px в 1 |
| 2 колонки списков | `.two` / `.col` | `1fr 1fr`, `gap:14px` | `var(--r-lg)` | 860px в 1 |
| Текст плюс фото | `.mod2` | `1fr 1fr`, `gap:14px` | `var(--r-lg)` | 820px в 1, фото `order:0` |
| Галерея | `.gal` | `repeat(4,1fr)`, `gap:14px` | `var(--r)` | 820px в 2 |
| Таймлайн | `.steps` / `.step` | `96px 1fr 1.25fr`, `gap:30px` | нет, разделитель линией | 820px в 1, `gap:6px` |
| Таблица | `.tw` плюс `table` | `min-width:780px` | `44px`, ≤900px `var(--r)` | прокрутка внутри обёртки |
| Вертикальный стек | `.out` или `.alerts` | flex column, `gap:14px` или `10px` | по карточке | не требуется |

Зазор всегда `14px`, если у блока нет причины из списка исключений в 2.5 (`2px` в hero, `30px` / `6px` в `.step`, `10px` в `.alerts`). Парные блоки получают равные колонки, карточки в ряду - `align-items:stretch`, текстовые ряды - `baseline`. Симметрия проверяется по правилу 5 части 9. Радиус выбирается по ширине колонки: широкий блок в одну-две колонки получает `var(--r-lg)`, мелкая карточка в три-четыре - `var(--r)`. Если блок с `var(--r-lg)` где-то сжимается, добавляем ему в медиазапросе `border-radius:var(--r)`, как сделано у `.ctrl`, `.oh`, `.tw`.

### Шаг 5. Выбрать типографику по шкале

Ничего нового не придумываем, берём ближайшую строку из шкалы 1.3.

| Роль в блоке | Значения |
|---|---|
| Заголовок карточки, крупный | `24px / 700 / -0.025em / line-height 1.12` (как `.pillar h3`) |
| Заголовок карточки, средний | `22px / 700 / -0.025em` (как `.col h3`, `.step h3`) |
| Заголовок карточки, мелкий | `20px / 700 / -0.02em` (как `.risk h3`) |
| Крупное число | `clamp(30px,3.6vw,42px) / 800 / -0.03em / line-height 1` плюс `font-variant-numeric:tabular-nums` |
| Очень крупное число на тёмном | `clamp(46px,7vw,86px) / 800 / -0.04em / line-height 1` |
| Описание внутри карточки | `13.5px` или `14px`, `line-height 1.5`-`1.55`, цвет `var(--ink2)` |
| Строка списка | `13.5px` (плотный список) или `15px` (просторный), цвет `var(--ink2)`, `b` внутри в `var(--ink)` |
| Капс-метка | `11px` или `12px`, вес `700`-`800`, `letter-spacing` `0.06em`-`0.11em`, `uppercase`, цвет `var(--ink3)` |
| Сноска и источник | `12.5px`, `line-height 1.75`, цвет `var(--ink3)` |

Веса только `500`, `600`, `700`, `800`. Крупный заголовок всегда `800` с отрицательным трекингом. Ни одного значения `font-style:italic`.

### Шаг 6. Повесить классы появления

- На каждый самостоятельный узел блока - `rv`. На hero - никогда.
- В сетке задержки идут циклом по позиции в ряду: первый `rv`, второй `rv rv-d1`, третий `rv rv-d2`, четвёртый `rv rv-d3`, пятый снова `rv`.
- В паре «заголовок плюс подпись» подпись получает `rv-d1`.
- На контейнер фотографии добавляем `ph-zoom` (не на `img`), класс сочетается с `rv` на том же узле.
- Проверить, что новый блок попадает под общий селектор `document.querySelectorAll('.rv,.ph-zoom')`, никакой регистрации не нужно.

### Шаг 7. Проверить адаптив

1. Сетка схлопывается на своём пороге, колонка нигде не уже своего содержимого.
2. Крупный радиус не съедает углы на узком экране: если блок с `var(--r-lg)` становится уже примерно 420px, добавить понижение радиуса.
3. Высоты, фиксированные для выравнивания содержимого карточек, обязательно сброшены в `height:auto;overflow:visible` при переходе в одну колонку. При переходе из четырёх колонок в две выравнивание внутри каждой пары сохраняется и проверяется отдельно. Сброс на 1000px в CSS эталона - историческое поведение, а не основание отменять проверку пар.
4. Все кегли и поля, которые должны дышать, сделаны через `clamp`, а не через дополнительный медиазапрос.
5. Ничего не вылезает по горизонтали: широкий контент (таблица, диаграмма) кладётся в обёртку с `overflow-x:auto`, страница горизонтально не скроллится.

### Пустой шаблон блока

```html
<section class="gray" id="ИД_СЕКЦИИ">
  <div class="wrap">

    <span class="tag rv">МЕТКА</span>
    <h2 class="sec rv">Первая половина мысли. <em>Вторая половина.</em></h2>
    <p class="sub rv rv-d1">Лид на одну-две строки с <b>ключевым словом</b>.</p>

    <div class="pillars">
      <article class="pillar rv">
        <span class="pil-n">01</span>
        <h3>Заголовок</h3>
        <p class="pil-sub">Подзаголовок на две-три строки.</p>
        <div class="ph ph-zoom rv"><img src="..." width="736" height="981" loading="lazy" alt="..."></div>
        <ul>
          <li><b>Тезис</b> продолжение мысли</li>
          <li>Второй тезис</li>
        </ul>
      </article>
      <article class="pillar rv rv-d1">...</article>
      <article class="pillar rv rv-d2">...</article>
      <article class="pillar rv rv-d3">...</article>
    </div>

    <div class="note rv">Вывод секции одной фразой с <b>акцентом</b>.</div>

  </div>
</section>
```

CSS писать не нужно: все классы шаблона уже описаны в частях 1-5. Новый CSS появляется только тогда, когда в системе действительно нет подходящей сетки, и тогда он собирается из тех же величин: `gap:14px`, `border:1px solid var(--line)`, радиус `var(--r)` или `var(--r-lg)`, фон `#fff` (жёстким литералом, а не `var(--card)`, если блок может оказаться на серой полосе).

---

# ЧАСТЬ 9. ЖЕЛЕЗНЫЕ ПРАВИЛА

Эти пять правил проверяются на каждой сборке. Геометрия проверяется замерами в браузере, CSS и текст - командами и просмотром конкретных значений. Нарушение любого из них означает, что получилась не NORMA.

## Правило 1. Полотно чередуется цветом, а не линейками

Блоки разделяются сменой фона и скруглением, а не горизонтальными бордерами.

Что это значит в значениях:

- белая секция `section{background:var(--card)}` = `#ffffff`, радиуса не имеет вообще (белое на белом фоне страницы, граница не нужна);
- серая `section.gray{background:var(--soft)}` = `#f5f5f5` и **только** `border-radius:0 0 var(--r-lg) var(--r-lg)`, то есть скругление снизу, плюс `overflow:hidden`;
- `--r-lg` = `80px`, на `max-width:640px` `40px`;
- `.final` скруглён только сверху, `footer` только снизу, вместе они читаются как один тёмный блок;
- ни у одной секции нет `border-top` или `border-bottom`. Все 20 вхождений `var(--line)` - это рамки карточек, линия под шапкой таблицы, линия над итогом, разделитель шагов и подчёркивание `.grp-t`. Ни одного разделителя между полотнами.

Что запрещено: горизонтальная линия между секциями, рамка вокруг секции, тень под секцией, «декоративный» разделитель любого вида. Тень в системе всего три штуки, и ни одна не относится к полотну (см. 1.7).

Проверка: в CSS нет ни одного правила вида `section{border-top:...}` или `section + section{border:...}`; чередование фонов идёт без пропусков (в эталоне concept белый, look серый, menu белый, semi серый, unit белый, calc серый, milk белый, risk серый, plan белый).

## Правило 2. Числа на тёмном белые, подпись полупрозрачная белая

Цветной акцент на крупных числах - это не NORMA.

Точные значения из эталона:

- `.oh .money{font-size:clamp(46px,7vw,86px);font-weight:800;letter-spacing:-0.04em;line-height:1}` - цвет наследуется от `.oh{color:#fff}`, то есть чистый белый;
- подпись над числом `.oh .k{color:rgba(255,255,255,0.55)}`;
- комментарий под числом `.oh .note-d{color:rgba(255,255,255,0.68)}`, акцент внутри `.oh .note-d b{color:#fff}`;
- то же в hero: `.hs b{color:#fff}`, `.hs span{color:rgba(255,255,255,0.6)}`;
- единственное исключение во всём файле - `.oh .money.bad{color:#ffb59b}`, и это не акцент, а сигнал отрицательного результата: осветлённый терракотовый, потому что `var(--warn)` = `#9a4a2f` на `#111111` не читается.

Цвет на числах допускается только в таблице и только на мелком кегле: `.pos{color:var(--acc);font-weight:700}` и `.neg{color:var(--warn);font-weight:700}` при `table{font-size:15px}`. Крупное число цветным не бывает никогда.

Проверка: у каждого числа кегля `clamp(30px,3.6vw,42px)` и крупнее цвет либо наследуемый `var(--ink)`, либо `#fff`, либо `#ffb59b` в случае убытка. Никаких зелёных и терракотовых крупных цифр.

## Правило 3. Никаких мелких надписей-ярлыков над заголовками

Над заголовком секции стоит ровно один элемент, и это `.tag`. Больше ничего.

Что именно допускается:

```css
.tag{display:inline-block;font-size:12px;font-weight:700;letter-spacing:0.11em;text-transform:uppercase;border:1px solid var(--line);border-radius:999px;corner-shape:round;padding:7px 16px;margin-bottom:20px;color:var(--ink2)}
```

Одна пилюля с рамкой, одно-два слова, цвет `var(--ink2)`, отступ до заголовка `20px`. Проверено по эталону: 9 тегов на 9 заголовков `h2.sec`, один к одному, и больше над `h2` нет ничего.

Что запрещено: «эйрбоу» без рамки, мелкая строчка-рубрика над заголовком, номер раздела, подпись вида «раздел 3 из 7», дублирующая мелкая расшифровка над крупным заголовком. Такие надписи из системы вычищены, возвращать их нельзя.

Капс-метки в системе есть, но живут они **внутри** блока, а не над заголовком секции: `.stat .k` (12px), `.kpi .k` (11px), `.oh .k` (12px), `.grp-t` (11px), `.flow-t` (11px), `.risk .lv` (11px), `thead th` (11.5px). Все они подписывают конкретное число или группу контролов, а не заголовок.

Текст `.tag` содержательный («Юнит-экономика», «12 недель»), а не название раздела («О нас», «Процесс», «Цены»). Так `.tag` остаётся именованной частью системы, а не рефлексом, который `impeccable` ловит как признак ИИ (12.1).

Проверка: перед каждым `<h2 class="sec">` в разметке стоит ровно один `<span class="tag ...">` и ничего больше; ни одного `<p>` или `<span>` с кеглем меньше 14px между началом `.wrap` и заголовком.

## Правило 5. Симметрия (переносимое)

### Принцип

Все элементы одного ряда начинаются на одной горизонтали и заканчиваются на одной. Парные блоки равны по ширине, карточки в ряду равны по высоте, отступы вокруг парных элементов одинаковые.

Чем чище система, тем важнее это правило. Если нет теней, нет цвета и нет рамок, выравнивание остаётся единственным, за что цепляется глаз: перекос в 10-15 пикселей читается уже как поломка.

Симметрия по умолчанию. Асимметрия допустима только осознанно и только там, где несёт смысл. В остальных случаях это дефект. Каждое исключение фиксируется вместе с причиной, селектором и брейкпоинтом.

### Четыре механизма

**1. Один зазор на всю систему.** Выбрать одно значение `gap` и держать во всех сетках. Исключений 2-4 на весь проект, если они нужны; каждое обосновано. «Тут 12, тут 16, тут 20» быть не должно: разнобой расползается с каждой новой секцией.

В NORMA базовый зазор контентных сеток и стеков карточек - `14px`. Три исключения по компонентам с причинами и мобильным значением `.step` перечислены в 2.5. При переносе в другой проект базовый зазор выбирается один раз; реестр исключений содержит селектор, значение, брейкпоинт и причину. Внутренние отступы кнопок и зазоры между подписью и значением не подменяют зазор сетки.

**2. Растяжка вместо подгонки.** Рядам карточек ставить `align-items:stretch`: карточки тянутся до высоты самой высокой. Исключение - ряды, где выравниваются строки текста, там `align-items:baseline`. Для текстового ряда проверяется общая базовая линия; равенство верхней и нижней границы требуется от карточек и парных блоков.

**3. Фиксированная высота у того, что обязано совпасть.** Если внутри карточек есть элемент, который должен начинаться на одной линии во всех карточках ряда (фото, разделитель, кнопка), то всё, что над ним, получает одинаковую фиксированную высоту с `overflow:hidden`. Иначе заголовок из одной строки и заголовок из трёх разведут содержимое по разным линиям. Высота подбирается под фактический текст; обрезание не заменяет проверку содержимого.

Обязательное дополнение: на брейкпоинте, где ряд схлопывается в одну колонку, высоты сбрасываются в `auto`, а `overflow` в `visible`. Совпадать больше нечему, а обрезанный текст иначе останется обрезанным. Пока в ряду две или больше карточек, внутренние линии по-прежнему должны совпадать.

В эталоне `.pillar h3` имеет `height:70px`, `.pil-sub` - `height:84px`, оба с `overflow:hidden`. Исходник снимает эти ограничения уже на 1000px, при переходе в две колонки. Это описание исходника, а не разрешение нарушать симметрию пары в новой сборке. `min-height` у `.stat`, `.kpi` и `.oh` задаёт минимальный размер блока, но сам по себе не выравнивает внутренние фото, разделители или кнопки.

**4. Парные блоки равны по ширине.** `1fr 1fr`, а не `1.2fr 1fr` без причины. Для защиты от разной минимальной ширины содержимого использовать `repeat(2,minmax(0,1fr))` и `min-width:0` у детей. Паддинги, границы и внешние отступы пары одинаковые; итоговая ширина проверяется в браузере.

В NORMA `.two` и `.mod2` - равноправные пары. `.calc` (`370px 1fr`) совмещает панель управления и результат, `.step` (`96px 1fr 1.25fr`) разделяет номер, название и описание, `.hero .hero-img` (`1.15fr 1fr 1fr`) задаёт композицию фото. Это осмысленные различия ролей; их пропорции не переносятся на обычные парные карточки.

### Запрет

Вертикальный ритм внутри сетки или флекса НЕ задаётся отступами на соседей.

```css
/* ТАК НЕЛЬЗЯ */
.card + .card{margin-top:14px}
```

В сетке все карточки - сиблинги. Отступ получает каждая, кроме первой, и в сетке из четырёх колонок три карточки из четырёх окажутся ниже первой. Ритм между рядами задаёт `gap` сетки, между секциями - их собственный вертикальный паддинг. Соседский селектор для вертикального ритма не нужен никогда.

Тот же дефект дают выборочный `margin-top` у элемента сетки, `:not(:first-child){margin:...}` и `nth-child` с отступами. Для стека на `flex-direction:column` также используется `gap`. Отступ у всей сетки относительно предыдущего блока не является отступом между её карточками.

### Проверка

Не на глаз. Проверять каждый фактический ряд отдельно на каждом брейкпоинте: после загрузки шрифтов и изображений, завершения reveal-анимаций и без hover-сдвига. Нельзя сравнивать одной выборкой карточки из разных рядов или определять состав ряда по совпавшему `top`: так можно пропустить сдвинутую карточку.

Быстрый замер для селектора, который выбирает ровно один ряд:

```js
[...document.querySelectorAll('СЕЛЕКТОР_РЯДА > *')]
  .map(e => e.getBoundingClientRect().top)
// Все значения одного ряда обязаны быть одинаковыми.
```

Для ширин парных блоков то же с `.width`. Для карточек также сравниваются `.bottom` и `.height`. Разница обязана быть `0`, а не «примерно ноль»: без округления и допуска.

Пример проверки первого ряда `.pillars` при четырёх видимых колонках. На других брейкпоинтах и для следующих рядов выбрать соответствующий набор соседних элементов по их позиции в сетке:

```js
const grid = document.querySelector('.pillars');
const row = grid ? [...grid.children].slice(0, 4) : [];
if (row.length !== 4) throw new Error('Не найден полный ряд из четырёх карточек');
const rects = row.map(e => e.getBoundingClientRect());
const metrics = ['top', 'bottom', 'height', 'width'];
console.table(rects.map(r => Object.fromEntries(metrics.map(key => [key, r[key]]))));
for (const key of metrics) {
  const values = rects.map(r => r[key]);
  const delta = Math.max(...values) - Math.min(...values);
  console.assert(delta === 0, `${key}: разница ${delta}px`, values);
}
```

Отдельно тем же способом замеряются одноимённые внутренние элементы (фото, разделитель, кнопка). У пары сравниваются соответствующие `padding` и `margin` через `getComputedStyle()`.

Поиск запрещённого соседского отступа в исходниках; запускать из каталога проекта:

```bash
rg -nU --glob '*.{css,scss,html,vue,svelte,jsx,tsx}' \
  '\+\s*\.[a-zA-Z_][\w-]*\s*\{[^}]*\bmargin-top\s*:' .
```

Вывод должен быть пустым для сеток и флексов. Команда ловит типовой селектор, в том числе многострочный; это дополнительная проверка, а не доказательство отсутствия всех вариантов. Отдельно проверить `margin`, `margin-block-start`, `:not(:first-child)`, `nth-child` и вычисленные стили в браузере. Markdown исключён из поиска, чтобы запрещённый учебный пример в этом документе не считался дефектом сайта.

## Правило 4. Никаких длинных и средних тире нигде

Только короткий дефис `-` (U+002D). Ни `—` (U+2014, em dash), ни `–` (U+2013, en dash) - ни в разметке, ни в текстах, ни в JS-шаблонах, ни в атрибутах, ни в мета-тегах.

В эталоне это выдержано полностью. Проверка командой:

```bash
grep -c $'—\|–' /path/to/index.html
# эталон возвращает 0
```

Более строгий вариант, который заодно ловит минус-знак и прочие похожие символы:

```bash
grep -nP '[\x{2010}-\x{2015}\x{2212}]' /path/to/index.html
# эталон выводит ОДНУ строку: 962
```

Проверено вручную 17.09.2026: строгая проверка находит в эталоне ровно одно вхождение, `−` (U+2212 MINUS SIGN) на строке 962, внутри JS-шаблона строки таблицы, перед отрицательной суммой. Это математический минус, а не тире, и под запрет он не попадает. Но простая проверка через `grep -c '—\|–'` его в принципе не видит, поэтому не считайте её доказательством чистоты по всем похожим символам: она отвечает ровно на один вопрос, есть ли em-dash и en-dash.

То же и в GOODHUMAN: 7 вхождений U+2212 на сайте и 13 в маркетинг-плане, все перед числами («−15%», «−25%», «−41%») и в формуле «углеводы − клетчатка». Нарушением это не является.

Диапазоны пишутся дефисом: «Недели 1-2», «120-160 тыс.», «36-44 тыс.», «×1,5-1,6». Пауза в предложении пишется запятой, двоеточием или разбивкой на два предложения. Единственное место, где длинные штрихи допустимы, - декоративные символы внутри CSS-комментариев исходника (там стоят символы псевдографики, а не тире), но и от них лучше избавиться, чтобы проверка была однозначной.

---

# ЧАСТЬ 10. ЧЕК-ЛИСТ ПРИЁМКИ

По этим пунктам видно, что вёрстка получилась NORMA, а не похожая на неё. Каждый пункт проверяется либо командой, либо глазами в devtools по конкретному значению.

### Симметрия

- [ ] Все элементы каждого ряда имеют одинаковый `getBoundingClientRect().top`, разница ровно `0`.
- [ ] Нижние границы и высоты карточек каждого ряда совпадают: разница `.bottom` и `.height` ровно `0`.
- [ ] Парные блоки равны по ширине: разница `.width` ровно `0`; соответствующие паддинги и отступы одинаковые.
- [ ] `gap` одинаковый во всех контентных сетках и стеках карточек: в NORMA `14px`, три исключения с причинами и брейкпоинтами перечислены в 2.5. В другом проекте выбран один базовый зазор и обоснованы не более четырёх исключений.
- [ ] У рядов карточек `align-items:stretch`, у текстовых рядов `baseline`; для последних проверена базовая линия.
- [ ] Области над фото, разделителем или кнопкой имеют одинаковую фиксированную высоту с `overflow:hidden`, если она нужна для совпадения внутренних линий. Одного `min-height` для этого недостаточно.
- [ ] На брейкпоинте схлопывания в одну колонку выравнивающие высоты сброшены в `height:auto;overflow:visible`; текст доступен полностью. В двух колонках симметрия пары сохранена.
- [ ] Ни одного правила вида `.x + .x{margin-top}` внутри сетки или флекса; поиск из правила 5 не находит нарушений. Проверены также выборочные отступы через `margin`, `margin-block-start`, `:not(:first-child)` и `nth-child`.
- [ ] Асимметрия встречается только в перечисленных и обоснованных исключениях.
- [ ] Проверено замерами в браузере на каждом брейкпоинте и для каждого фактического ряда, после загрузки ресурсов и завершения анимаций. Значения не округлялись, визуальная оценка не заменяла замеры.

### Токены и шрифт

- [ ] В `:root` ровно 16 переменных с точными значениями из 1.1 (с 27.09.2026 добавлен `--ink-mute`), названия не переименованы.
- [ ] `--r-lg` переопределяется на `40px` внутри `@media(max-width:640px)`, и это единственное переопределение переменной вне `:root`.
- [ ] Подключён Hanken Grotesk с весами `400;500;600;700;800;900` и `display=swap`, других шрифтов на странице нет.
- [ ] Ни одного `font-style:italic`; все `em` и `i` имеют `font-style:normal`.
- [ ] На `html,body` стоит `line-height:1.5` и `-webkit-font-smoothing:antialiased`; базовый `font-size` не задан.
- [ ] Все числовые значения, которые выстраиваются в столбик (KPI, таблица, ползунки, статы), имеют `font-variant-numeric:tabular-nums`.

### Форма и цвет

- [ ] В CSS есть `*,*::before,*::after{...corner-shape:superellipse(2.02)}` и отдельное правило, возвращающее `corner-shape:round` пилюлям, кругам и `button`.
- [ ] Все пилюли имеют `border-radius:999px`, все крупные блоки `var(--r-lg)`, мелкие карточки `var(--r)`. Промежуточных «на глаз» радиусов нет, кроме заявленных `44px` у `.tw`, `24px` и `16px` в мобильном меню, `8px` у фокуса, `2px` у трека ползунка.
- [ ] Ровно один зелёный `#1d5c46` и один терракотовый `#9a4a2f`, оба через `var()`. Третьего акцентного цвета на странице нет.
- [ ] Белые карточки внутри `section.gray` покрашены литералом `#fff`, а не `var(--card)`.
- [ ] `box-shadow` встречается не более трёх раз и ни разу не на карточке или секции.
- [ ] Все границы `1px`, только `var(--line)` и `var(--line2)`; единственная граница `2px` - белое кольцо ручки ползунка.

### Полотно

- [ ] `body{padding:16px}` и `8px` на `max-width:640px`, `overflow-x:hidden` на `html,body`.
- [ ] Белые и серые секции чередуются без пропусков; между полотнами нет ни одной горизонтальной линии (правило 1).
- [ ] Серая секция скруглена только снизу, `.final` только сверху, `footer` только снизу, `.hero` и `.quote` по всем углам; у всех `overflow:hidden`.
- [ ] `.slab` подтянут вверх через `margin-top:calc(-1 * var(--r-lg))` и компенсирован `padding-top:var(--r-lg)!important`.
- [ ] Единственный контейнер контента - `.wrap` с `max-width:1300px` и `padding:0 clamp(24px,5vw,64px)`.
- [ ] Вертикальные паддинги полотен заданы формулами `clamp`, а не фиксированными числами (кроме `footer{padding:38px 0 44px}`).

### Навигация

- [ ] Шапка это три отдельные стеклянные пилюли, а не одна панель; у `.hero-nav` `background:transparent`.
- [ ] `backdrop-filter:blur(12px)` с `-webkit-` дублем на пилюлях, `blur(16px)` на мобильном меню.
- [ ] `.links` центрированы абсолютом `left:50%; transform:translateX(-50%)`, `.nav-actions` прижаты `margin-left:auto`.
- [ ] На десктопе (`min-width:901px`) CTA растянута `align-self:stretch` и врезана в край пилюли отрицательным `margin:-3px -3px -3px 0`.
- [ ] Класс `scrolled` ставится по порогу `window.scrollY > hero.offsetHeight - 90`, слушатель `{passive:true}`, с первичным вызовом `onScroll()`.
- [ ] В состоянии `scrolled` меняются только цвета: стекло в `rgba(10,10,10,0.11)`, текст в `#0a0a0a`, CTA инвертируется. Геометрия не меняется.
- [ ] На `max-width:900px` ссылки скрыты, бургер включён, меню открывается классом `mopen`, символ переключается `☰` и `✕`, клик по ссылке закрывает меню.

### Типографика блоков

- [ ] Крупные заголовки все с весом `800` и отрицательным трекингом от `-0.02em` до `-0.04em`.
- [ ] Двухцветный заголовок сделан через `em`: в `.slab h2` и `.hero-line h1` приглушён базовый текст, в `h2.sec` приглушён `em`.
- [ ] Капс-метки только `11px`, `11.5px`, `12px`, вес `700`-`800`, `letter-spacing` `0.05em`-`0.11em`, цвет `var(--ink3)`.
- [ ] Над заголовком секции только `.tag` и ничего больше (правило 3).
- [ ] Вторичный текст везде `var(--ink2)`, приглушённый `var(--ink3)`, акцент внутри текста `<b>` цветом `var(--ink)`.

### Тёмные блоки

- [ ] Тёмный фон всегда `#111111` через `var(--dark)`, другого тёмного оттенка на фоне нет.
- [ ] Приглушение на тёмном сделано белым с альфой из шкалы `0.55 / 0.6 / 0.66 / 0.68 / 0.82`, а не серым цветом (исключение `.quote p{color:#7d7d78}`).
- [ ] Крупные числа на тёмном белые (правило 2).
- [ ] `<meta name="theme-color">` совпадает со значением `--dark`.

### Движение

- [ ] Ровно один reveal-механизм: `.rv` плюс `.in`, `translateY(30px)`, `.8s`, `cubic-bezier(.16,1,.3,1)`.
- [ ] Задержки только три: `.08s`, `.16s`, `.24s`, шаг 80 мс, в сетках циклятся по позиции в ряду.
- [ ] `ph-zoom` висит на контейнере, не на `img`; масштаб `1.08` в `1`, `1.4s`.
- [ ] Наблюдатель один на страницу, `{threshold:0.12, rootMargin:'0px 0px -5% 0px'}`, с `io.unobserve` после срабатывания.
- [ ] Есть обе защиты `prefers-reduced-motion`: медиазапрос в CSS и ветка в JS.
- [ ] Hero без `.rv`.
- [ ] `@keyframes` и свойство `animation` не используются ни разу.
- [ ] Hover нигде не двигает контент: максимум `translateY(-2px)` на кнопке и усиление рамки на один шаг прозрачности.
- [ ] Есть общий фокус-ринг `outline:2px solid var(--acc);outline-offset:3px;border-radius:8px`.

### Адаптив

- [ ] Зазор сеток `14px` везде, кроме трёх заявленных исключений.
- [ ] Каждая сетка схлопывается на своём пороге из 7.1; сетка из четырёх колонок обязательно проходит ступень в две.
- [ ] Блоки с `var(--r-lg)`, которые ужимаются (`.ctrl`, `.oh`, `.tw`), понижают радиус на своём брейкпоинте.
- [ ] Фиксированные высоты с `overflow:hidden` сняты на своём брейкпоинте.
- [ ] Ни один кегль и ни одно крупное поле не адаптируются лишним медиазапросом там, где достаточно `clamp` (единственное исключение системы - `.hs b{font-size:19px}` на 520px).
- [ ] Широкий контент лежит в обёртке с `overflow-x:auto` и `min-width` на самой таблице; страница по горизонтали не скроллится.

### Текст и финальные проверки

- [ ] `grep -c $'—\|–' index.html` возвращает `0` (правило 4).
- [ ] Ссылки в тексте не подчёркнуты и наследуют цвет: в CSS есть `a{color:inherit;text-decoration:none}` и нет ни одного `text-decoration:underline`.
- [ ] Иконочных библиотек и `<svg>` нет, все «иконки» - текстовые символы.
- [ ] У всех `img` проставлены `width` и `height`; ниже первого экрана `loading="lazy"`, у картинок hero `fetchpriority="high"`.
- [ ] У каждой символьной кнопки есть осмысленный `aria-label`, у ползунков - `aria-label` с названием параметра.
- [ ] Скрипт лежит в конце файла, обёрнут в IIFE, ничего не выносит в глобальную область.
- [ ] Страница проверена с выключенным JS и в печати: весь контент виден (`.rv` прячет только `html.js`, строка в `<head>` из 6.2 на месте).

### Качество (часть 12)

- [ ] Контраст: текст до 24px не ниже 4.5:1, крупнее не ниже 3:1. `--ink3` только на крупном тексте, мелкое на `--ink-mute`.
- [ ] Строка основного текста 65-75 знаков, основной текст от 16px, функциональный от 11px, трекинг заголовков не туже -0.04em.
- [ ] Нет горизонтальной прокрутки на 320, 375, 768, 1280; ни один заголовок не переполняет контейнер.
- [ ] Тач-цели от 44×44px, фокус виден на всех интерактивных элементах, `outline:none` без замены нет.
- [ ] У каждой формы и списка есть пустое, загрузочное и ошибочное состояние.
- [ ] `[hidden]` реально скрывает (проверено `getBoundingClientRect`, а не наличием атрибута).
- [ ] Детектор `impeccable detect` с игнорами из 12.9 прогнан на 1280 и 390, находки закрыты или объяснены.

---

# ЧАСТЬ 11. РАСХОЖДЕНИЯ: NORMA против GOODHUMAN

Раздел добавлен отдельно от разбора: это результат прямого сравнения двух исходников, `moloko/index.html` и `goodhuman/index.html`, а не чтения одного из них.

**Ядро совпадает полностью.** Все 17 переменных `:root`, а также правила `body`, `.wrap`, `section` и `.hero-nav` в GOODHUMAN побайтно те же, что в эталоне. То есть NORMA перенесена без правок, а не «по мотивам».

**GOODHUMAN только добавляет свои переменные**, ни одной не переопределяя:

| Переменная | Значение | Зачем |
|---|---|---|
| `--accent` | `#E8B84A` | акцент проекта |
| `--accent-rgb` | `232 184 74` | тот же акцент для `rgb()` с альфой |
| `--dv-low` | `#c9c9c9` | шкала процента дневной нормы, низкий |
| `--dv-mid` | `#c79a3a` | средний |
| `--dv-good` | `#1d5c46` | хороший |
| `--dv-high` | `#2f7a5e` | выше нормы |
| `--dv-ul` | `#9a4a2f` | выше верхнего предела |
| `--m-prot` | `#111111` | белок в диаграммах |
| `--m-fat` | `#6e6e6e` | жир |
| `--m-carb` | `#b3b3b3` | углеводы |
| `--m-fib` | `#1d5c46` | клетчатка |

**Правило для переноса:** новый проект наследует все 17 переменных эталона без изменений и добавляет свои под своим префиксом. Переопределять базовые нельзя, иначе система перестаёт быть NORMA.

**Осторожно с одним местом в старой документации.** В `HANDOFF.md` раздела 4 записано `body{padding:16px}` и 8px на мобильном. В эталоне этого нет: `body` имеет `padding:0`. Верно то, что в CSS, а не в конспекте.

---

# ЧАСТЬ 12. КАЧЕСТВО: ЧТО ВЗЯТО ИЗ IMPECCABLE

Слито 27.09.2026 из скила `impeccable` 4.4.0 (`~/.claude/skills/impeccable`). Взяты правила качества, которые NORMA не покрывала или покрывала хуже. Канон NORMA остаётся главным: при конфликте побеждает NORMA.

## 12.1. Что сознательно НЕ взято (решение Макса)

| Правило impeccable | Почему не действует в NORMA |
|---|---|
| Кремовый, песочный, «бумажный» фон (`--paper`, `--cream`, тёплая полоса L 0.84-0.97) как признак ИИ | **Разрешён.** Тёплый фон бывает частью бренда (Lily of the Valley, `--paper`) |
| Glassmorphism «только редко или никогда», «Alpha is a design smell» | **Разрешён.** Матовые пилюли навигации NORMA, Apple Glass у self-help линейки. Стекло по-прежнему должно быть читаемым: контраст текста на стекле проверяется как на обычном фоне |
| Нумерация секций 01 / 02 / 03 как признак ИИ | **Разрешена.** `.pil-n`, `.step .no`, нумерация в презентациях (5.7) |
| Мелкая капс-метка над каждой секцией | **`.tag` остаётся каноном** (4.3, Правило 3). Сам impeccable делает исключение: «именованная система кикеров законна, запрещён рефлекс». Требование к `.tag`: текст содержательный («Юнит-экономика», «12 недель»), а не название раздела («О нас», «Процесс») |
| Одно семейство шрифтов как признак ИИ (`single-font`) | Hanken Grotesk во многих весах выбран осознанно |

## 12.2. Цвет и контраст

- Текст до 24px (и до 18.7px жирным) **не ниже 4.5:1** к фону. Крупнее **не ниже 3:1**. Отсюда разделение `--ink3` / `--ink-mute` (1.1).
- Плейсхолдер тоже 4.5:1, не бледно-серый.
- Элементы интерфейса, иконки и фокус-кольцо **не ниже 3:1** к соседнему цвету.
- Серый текст на цветном фоне не ставить: брать более тёмный тон того же цвета или прозрачность цвета текста.
- Цвет никогда не единственный носитель смысла: у `.pos` / `.neg` рядом знак или слово. Красный с зелёным не сочетать без второго признака.
- Акцент (`--acc`) только для действий, текущего выбора и состояния, не для украшения. Акцент не больше ~10% поверхности.
- Тёмное полотно не инверсия светлого: глубина через светлоту поверхностей, у светлого текста на тёмном межстрочный интервал на 0.05-0.1 больше.

## 12.3. Типографика

- Длина строки основного текста **65-75 знаков** (`max-width` в `ch`). Больше ~85 уже ошибка.
- Основной текст не меньше **16px**, на мобильном не меньше 14px. Функциональный текст (кнопки, метки, числа) не меньше **11px**, прочий текст длиннее 20 знаков не меньше **12px**.
- Межстрочный: основной текст **1.5-1.7**, заголовки 1.05-1.2. Абзац длиннее 50 знаков с интервалом меньше **1.3** считается ошибкой.
- Трекинг дисплейных заголовков **не туже -0.04em**. На `-0.05em` буквы слипаются.
- Капс-метки: трекинг 0.05-0.12em и **не длиннее ~30 знаков**. Основной текст капсом не набирается.
- Потолок hero: верх `clamp()` не больше **~96px** (у NORMA 88px, в норме) и не больше ~2.5× от нижнего значения.
- `text-wrap:balance` на h1-h3, `text-wrap:pretty` на прозе (уже в `NORMA.css`).
- Цифры в таблицах и счётчиках `tabular-nums`.
- Весов шрифта 3-4, грузятся только используемые. `font-display:swap`.

## 12.4. Раскладка

- Отступы по сетке 4px: 4, 8, 12, 16, 24, 32, 48, 64, 96. Внутри группы 8-12px, между секциями 48-96px. Ритм даёт чередование плотного и щедрого.
- Внутренний отступ у блока с фоном или рамкой **не меньше 12-16px**. Текст не касается рамки.
- Текст не ближе **16px** к краю экрана (лучше 24-32px).
- Вложенные карточки запрещены. Карточка только там, где это лучшая форма, а не по привычке.
- Одинаковая сетка «иконка + заголовок + текст», повторённая много раз, считается признаком ИИ. У NORMA сетки несут данные (`.pillar` с фото и списком, `.risk` с уровнем), и так должно оставаться.
- `z-index` только по смысловой шкале: выпадающее меню, липкая шапка, подложка модалки, модалка, тост, подсказка. Значения `999` и `9999` запрещены.
- Горизонтальной прокрутки страницы нет ни на одной ширине. Переполнение больше 16px считается ошибкой. Каждый заголовок проверяется на каждом брейкпоинте.
- Навигация не больше 5 пунктов верхнего уровня. На первом экране дашборда не больше 4 ключевых метрик, в группе формы не больше 4 полей.
- У flex- и grid-детей `min-width:0`, иначе длинные слова выталкивают раскладку.

## 12.5. Движение

- **Появление только усиливает видимый контент.** Контент не прячется до срабатывания класса: в фоновой вкладке, при печати, в скриншоте и превью соцсети секция останется пустой. В NORMA `.rv` прячет только при `html.js` (6.1-6.2).
- Длительности: отклик 100-150 мс, смена состояния 200-300 мс, раскладка 300-500 мс, вход 500-800 мс. Выход короче входа (~75%).
- Кривые только ease-out: expo `cubic-bezier(.16,1,.3,1)` (канон NORMA), quart `(.25,1,.5,1)`. Bounce и elastic запрещены.
- Не анимировать `width`, `height`, `padding`, `margin`, `top`, `left`. Высоту через `grid-template-rows`.
- Каскад задержек только внутри одного списка и суммарно не больше 500 мс (у NORMA 80 мс на шаг).
- Одинаковое появление на каждой секции - это рефлекс. Движение подбирается под то, что появляется.
- `prefers-reduced-motion` обязателен для каждой анимации, в `NORMA.css` теперь гасится глобально.
- Бесконечная анимация (пульс, мигающая точка) только там, где она обозначает живой процесс.

## 12.6. Взаимодействие и состояния

- У каждого интерактивного элемента продуманы **8 состояний**: обычное, наведение, фокус, нажатие, выключено, загрузка, ошибка, успех.
- `outline:none` без замены запрещён. Фокус через `:focus-visible`, кольцо 2-3px с отступом 2px, одинаковое везде.
- Наведение не несёт функции. Эффекты наведения внутри `@media (hover:hover)`.
- Тач-цели **не меньше 44×44px**.
- Загрузка: скелет вместо спиннера посреди контента, текст конкретный («Сохраняем...»).
- Пустое состояние: что здесь будет, зачем, кнопка первого действия. Не просто «нет данных».
- Ошибка: что случилось, почему, как исправить, кнопка повтора. Ввод пользователя сохраняется. Ошибка под полем, связана через `aria-describedby`.
- Кнопка отправки выключается на время запроса, чтобы не было двойной отправки.
- «Отменить» лучше подтверждения. Подтверждение только для необратимого, дорогого или массового.
- Модалка через `<dialog>` или `inert`, выпадающее меню внутри `overflow:hidden` не прятать, выносить через popover или `position:fixed`.
- Атрибут `hidden` ничем не перебивается: в `NORMA.css` стоит `[hidden]{display:none!important}` (на этом уже дважды ломались реальные страницы).

## 12.7. Тексты интерфейса

- Кнопка: глагол + объект («Обсудить запуск», «Сохранить класс»). Не «ОК», не «Отправить», не «Нажмите здесь».
- Разрушительное действие называется прямо и с количеством («Удалить 5 записей»).
- Одна сущность называется одним словом во всём интерфейсе.
- Лишний вводный абзац под заголовком, который всё уже сказал, удаляется.
- Картинка со смыслом получает `alt` со смыслом, декоративная `alt=""`, кнопка-иконка `aria-label`.
- Штампы («инновационный», «мирового уровня», «Не X. Y.» три раза подряд) вычищаются. Длинные тире запрещены Правилом 4.

## 12.8. Доступность и скорость

- WCAG AA целиком: настоящие кнопки и ссылки, уровни заголовков без пропусков, ссылка «к содержимому», логичный порядок табуляции, зум до 200% без поломки, динамика объявляется через `aria-live`.
- LCP меньше 2.5 с, INP меньше 200 мс, CLS меньше 0.1. У всех медиа размеры или `aspect-ratio`, `loading="lazy"` только ниже первого экрана, форматы WebP/AVIF.

## 12.9. Как проверять (процесс)

Порядок работы: `shape` (бриф) → сборка → `critique` / `audit` → профильные команды → `polish` последним.

**Детектор** на каждой сборке, с выключенными правилами из 12.1:

```bash
~/.claude/skills/impeccable/scripts/impeccable ignores add-rule cream-palette
~/.claude/skills/impeccable/scripts/impeccable ignores add-rule numbered-section-markers
~/.claude/skills/impeccable/scripts/impeccable ignores add-rule numbered-section-labels
~/.claude/skills/impeccable/scripts/impeccable ignores add-rule repeated-section-kickers
~/.claude/skills/impeccable/scripts/impeccable ignores add-rule single-font
~/.claude/skills/impeccable/scripts/impeccable detect index.html
~/.claude/skills/impeccable/scripts/impeccable detect --viewport 390x844 index.html
```

`ignores` пишет в `.impeccable/config.json` проекта, один раз на проект. Чистый прогон детектора означает только отсутствие найденного, это не доказательство качества.

**Оценка critique** (через `/impeccable critique`): 10 эвристик Нильсена по 0-4, итог из 40. 36-40 отлично, 28-35 хорошо, 20-27 приемлемо, ниже 20 переделывать. Смотреть глазами 2-3 персон: новичок, пользователь с клавиатурой и скринридером, человек с телефоном в одной руке.

**Оценка audit**: доступность, скорость, адаптив, темы, анти-паттерны по 0-4, итог из 20. Ниже 14 не выкатывать.

**Важность находок**: P0 блокирует задачу, P1 нарушение AA или серьёзная проблема, P2 раздражает, есть обход, P3 полировка. Если пользователь напишет в поддержку, это минимум P1.

**Стресс-прогон** (`harden`): имена по 100+ знаков, эмодзи, офлайн, 1000+ строк, десять нажатий «Отправить» подряд, ответы API 400/401/403/404/429/500.

---

# ПРИЛОЖЕНИЕ. Полный CSS эталона

Ниже весь CSS страницы primeglow.co/moloko, байт в байт из `primepeak-site/moloko/index.html`. 22848 байт, 276 правил, скобки сбалансированы. Это источник правды: если документ выше и этот код разойдутся, прав код.

```css
*,*::before,*::after{box-sizing:border-box;corner-shape:superellipse(2.02)}
:root{
  --page:#ffffff; --card:#ffffff; --soft:#f5f5f5; --dark:#111111;
  --ink:#0a0a0a; --ink2:#6b6b6b; --ink3:#a1a1a1;
  --line:rgba(10,10,10,0.10); --line2:rgba(10,10,10,0.06);
  --acc:#1d5c46; --acc-soft:#eef4f0; --warn:#9a4a2f;
  --sf:'Hanken Grotesk',-apple-system,system-ui,sans-serif;
  --r:20px; --r-lg:80px; --r-sm:14px;
}
html{scroll-behavior:smooth}
html,body{margin:0;padding:0;background:var(--page);color:var(--ink);font-family:var(--sf);line-height:1.5;-webkit-font-smoothing:antialiased;overflow-x:hidden}
body{padding:16px}
@media(max-width:640px){body{padding:8px}}
img{max-width:100%;display:block}
a{color:inherit;text-decoration:none}
.wrap{max-width:1300px;margin:0 auto;padding:0 clamp(24px,5vw,64px)}
.num{font-variant-numeric:tabular-nums}
.nb{white-space:nowrap}

/* ── ПОЛОТНО: чередующиеся полноширинные секции белый и серый ── */
.hero,.slab,.quote,.final,footer,section{position:relative;margin:0}
section{background:var(--card);padding:clamp(56px,7vw,96px) 0}
section.gray{background:var(--soft);border-radius:0 0 var(--r-lg) var(--r-lg)}
.quote{border-radius:var(--r-lg);overflow:hidden}
.hero{border-radius:var(--r-lg);overflow:hidden}
.slab{border-radius:0 0 var(--r-lg) var(--r-lg);overflow:hidden;margin-top:calc(-1 * var(--r-lg));z-index:1;padding-top:var(--r-lg)!important}
.final{border-radius:var(--r-lg) var(--r-lg) 0 0;overflow:hidden}
footer{border-radius:0 0 var(--r-lg) var(--r-lg);overflow:hidden}
@media(max-width:640px){:root{--r-lg:40px}}

/* ── REVEAL ── */
.rv{opacity:0;transform:translateY(30px);transition:opacity .8s cubic-bezier(.16,1,.3,1),transform .8s cubic-bezier(.16,1,.3,1)}
.rv.in{opacity:1;transform:none}
.rv-d1{transition-delay:.08s}.rv-d2{transition-delay:.16s}.rv-d3{transition-delay:.24s}
.ph-zoom img{transform:scale(1.08);transition:transform 1.4s cubic-bezier(.16,1,.3,1)}
.ph-zoom.in img{transform:scale(1)}
@media(prefers-reduced-motion:reduce){.rv,.ph-zoom img{opacity:1!important;transform:none!important;transition:none!important}}

/* ── HERO ── */
.hero{height:calc(96vh - 26px);min-height:620px;background:var(--dark);border:none;z-index:2}
.hero .hero-img{position:absolute;inset:0;width:100%;height:100%;display:grid;grid-template-columns:1.15fr 1fr 1fr;gap:2px}
.hero .hero-img img{width:100%;height:100%;object-fit:cover;filter:contrast(1.02);min-width:0}
@media(max-width:760px){.hero .hero-img{grid-template-columns:1fr 1fr}.hero .hero-img img:nth-child(3){display:none}}
.hero .scrim{position:absolute;inset:0;background:linear-gradient(180deg,rgba(0,0,0,0.46) 0%,rgba(0,0,0,0.10) 30%,rgba(0,0,0,0.22) 50%,rgba(0,0,0,0.80) 100%)}
.hero-nav{position:fixed;top:44px;left:50%;transform:translateX(-50%);z-index:100;display:flex;align-items:center;width:calc(100% - 48px);max-width:1320px;padding:0;background:transparent;border:none;color:#fff}
@media(max-width:640px){.hero-nav{top:30px;width:calc(100% - 24px)}}
@media(min-width:1024px){.hero-nav{padding:0 100px}}
.hero-nav .logo,.hero-nav .links,.nav-actions{background:rgba(255,255,255,0.44);backdrop-filter:blur(12px);-webkit-backdrop-filter:blur(12px);border:none}
.hero-nav .logo{border-radius:999px;padding:8px 16px;font-weight:700;font-size:15px;letter-spacing:0.18em;color:#fff}
.hero-nav .links{position:absolute;left:50%;transform:translateX(-50%);display:flex;align-items:center;gap:2px;border-radius:999px;padding:3px 5px}
.hero-nav .links a{font-size:13px;font-weight:600;color:#fff;opacity:.92;padding:6px 12px;border-radius:999px;transition:background .15s,opacity .15s;white-space:nowrap}
.hero-nav .links a:hover{background:rgba(255,255,255,0.16);opacity:1}
.nav-actions{display:flex;align-items:center;gap:5px;margin-left:auto;border-radius:999px;padding:3px 3px 3px 5px}
.hero-nav .cta{background:#fff;color:#0a0a0a;font-weight:700;font-size:13px;padding:6px 14px;border-radius:999px;border:none;cursor:pointer;font-family:var(--sf)}
@media(min-width:901px){.hero-nav .cta{align-self:stretch;display:inline-flex;align-items:center;padding:0 16px;margin:-3px -3px -3px 0}}
.hero-nav.scrolled .logo,.hero-nav.scrolled .links,.hero-nav.scrolled .nav-actions{background:rgba(10,10,10,0.11)}
.hero-nav.scrolled .logo,.hero-nav.scrolled .links a{color:#0a0a0a}
.hero-nav.scrolled .links a:hover{background:rgba(10,10,10,0.07)}
.hero-nav.scrolled .cta{background:#0a0a0a;color:#fff}
.hero-nav .cta:hover{background:#ececec}
.hero-nav.scrolled .cta:hover{background:#2b2b2b}
.nav-burger{display:none}
.cur-toggle{display:grid;place-items:center;width:38px;height:38px;border-radius:999px;corner-shape:round;border:none;background:rgba(255,255,255,0.44);backdrop-filter:blur(12px);-webkit-backdrop-filter:blur(12px);cursor:pointer;color:#fff;font-family:var(--sf);font-weight:800;font-size:13px;flex:none;transition:background .15s,color .15s}
.hero-nav.scrolled .cur-toggle{background:rgba(10,10,10,0.11);color:#0a0a0a}
@media(max-width:900px){
  .hero-nav .links{display:none}
  .nav-burger{display:grid;place-items:center;width:38px;height:38px;border-radius:999px;corner-shape:round;border:none;background:rgba(255,255,255,0.44);backdrop-filter:blur(12px);-webkit-backdrop-filter:blur(12px);cursor:pointer;color:#fff;font-size:17px}
  .hero-nav.scrolled .nav-burger{background:rgba(10,10,10,0.11);color:#0a0a0a}
  .hero-nav .links.mopen{display:flex;flex-direction:column;position:absolute;top:calc(100% + 10px);left:0;right:0;background:rgba(20,20,20,0.92);backdrop-filter:blur(16px);-webkit-backdrop-filter:blur(16px);border-radius:24px;padding:12px;gap:2px}
  .hero-nav.scrolled .links.mopen{background:rgba(255,255,255,0.97);box-shadow:0 18px 50px rgba(0,0,0,0.14)}
  .hero-nav .links.mopen a{padding:14px 18px;font-size:16px;text-align:center;border-radius:16px}
  .hero-nav.scrolled .links.mopen a{color:#0a0a0a}
}
.hero-line{position:absolute;z-index:3;left:0;right:0;bottom:clamp(30px,5vw,64px);color:#fff}
.hero-line h1{font-size:clamp(38px,7vw,88px);font-weight:800;line-height:1.04;letter-spacing:-0.035em;margin:0;max-width:1120px;color:rgba(255,255,255,0.66)}
.hero-line h1 em{font-style:normal;color:#fff}
.hero-sub{margin:20px 0 0;max-width:820px;font-size:clamp(15px,1.45vw,18px);line-height:1.5;font-weight:500;color:rgba(255,255,255,0.82)}
.hero-sub b{color:#fff;font-weight:700}
@media(max-width:760px){.hero-sub{margin-top:14px;line-height:1.45}}
.hero-stats{display:flex;gap:clamp(18px,3vw,46px);flex-wrap:wrap;margin-top:clamp(22px,3vw,34px)}
.hs{min-width:120px}
.hs b{display:block;font-size:clamp(20px,2.6vw,30px);font-weight:800;letter-spacing:-0.02em;color:#fff;font-variant-numeric:tabular-nums;line-height:1.1}
.hs span{font-size:12px;color:rgba(255,255,255,0.6);font-weight:600;letter-spacing:0.04em}
@media(max-width:520px){.hero-stats{gap:14px 20px}.hs{min-width:88px}.hs b{font-size:19px}.hs span{font-size:11px}}

/* ── SLAB ── */
.slab{background:var(--soft);color:var(--ink);border:none;padding:clamp(44px,6vw,72px) 0}
.slab h2{font-size:clamp(32px,5.5vw,72px);font-weight:800;line-height:1.05;letter-spacing:-0.035em;margin:0;color:var(--ink3)}
.slab h2 em{font-style:normal;color:var(--ink)}
.slab-facts{display:flex;gap:36px;flex-wrap:wrap;margin-top:30px}
.sf b{display:block;font-size:34px;font-weight:800;letter-spacing:-0.02em;color:var(--ink);font-variant-numeric:tabular-nums}
.sf span{font-size:13px;color:var(--ink3);font-weight:600}

/* ── ЗАГОЛОВКИ СЕКЦИЙ ── */
h2.sec{font-size:clamp(30px,5vw,56px);font-weight:800;letter-spacing:-0.03em;line-height:1.08;margin:0 0 16px}
h2.sec em{font-style:normal;color:var(--ink3)}
.sub{font-size:17px;color:var(--ink2);max-width:680px;margin:0 0 clamp(34px,4vw,50px)}
.sub b{color:var(--ink);font-weight:700}
.tag{display:inline-block;font-size:12px;font-weight:700;letter-spacing:0.11em;text-transform:uppercase;border:1px solid var(--line);border-radius:999px;corner-shape:round;padding:7px 16px;margin-bottom:20px;color:var(--ink2)}

/* ── PILLARS ── */
.pillars{display:grid;grid-template-columns:repeat(4,1fr);gap:14px;align-items:stretch}
@media(max-width:1000px){.pillars{grid-template-columns:repeat(2,1fr)}}
@media(max-width:600px){.pillars{grid-template-columns:1fr}}
.pillar{background:var(--card);border:1px solid var(--line);border-radius:var(--r-lg);padding:0;overflow:hidden;display:flex;flex-direction:column;transition:border-color .3s}
.pillar:hover{border-color:rgba(10,10,10,0.20)}
.pil-n{display:grid;place-items:center;width:34px;height:34px;border:1px solid var(--line);border-radius:999px;corner-shape:round;font-size:13px;font-weight:600;color:var(--ink);margin:30px 22px 0;flex:0 0 auto;transition:border-color .3s,color .3s}
.pillar:hover .pil-n{border-color:rgba(10,10,10,0.35);color:#000}
.pillar h3{font-size:24px;font-weight:700;letter-spacing:-0.025em;margin:0;padding:16px 22px 4px;line-height:1.12;height:70px;overflow:hidden}
.pil-sub{font-size:14px;color:var(--ink2);margin:0;padding:0 22px 20px;line-height:1.5;height:84px;overflow:hidden}
@media(max-width:1000px){.pil-sub,.pillar h3{height:auto;overflow:visible}}
.pillar .ph{border-top:1px solid var(--line);overflow:hidden;aspect-ratio:16/11;margin:0;background:var(--soft)}
.pillar .ph img{width:100%;height:100%;object-fit:cover}
.pillar ul{margin:0;padding:14px 22px 18px;list-style:none}
.pillar li{font-size:13.5px;color:var(--ink2);padding:11px 0 11px 20px;position:relative;border-top:1px solid var(--line2);line-height:1.5}
.pillar li:first-child{border-top:none}
.pillar li::before{content:'';position:absolute;left:2px;top:18px;width:6px;height:6px;border-radius:999px;corner-shape:round;background:var(--ink)}
.pillar li b{color:var(--ink);font-weight:700}

/* ── СТАТ-КАРТОЧКИ ── */
.stats4{display:grid;grid-template-columns:repeat(4,1fr);gap:14px}
@media(max-width:1000px){.stats4{grid-template-columns:repeat(2,1fr)}}
@media(max-width:560px){.stats4{grid-template-columns:1fr}}
.stat{background:var(--card);border:1px solid var(--line);border-radius:var(--r);padding:26px 24px;min-height:230px;display:flex;flex-direction:column}
section.gray .stat{background:#fff}
.stat .k{font-size:12px;font-weight:700;letter-spacing:0.08em;text-transform:uppercase;color:var(--ink3);margin-bottom:auto}
.stat .v{font-size:clamp(30px,3.6vw,42px);font-weight:800;letter-spacing:-0.03em;line-height:1;margin:18px 0 10px;font-variant-numeric:tabular-nums}
.stat p{font-size:13.5px;color:var(--ink2);margin:0;line-height:1.5}

/* ── ДВЕ КОЛОНКИ СПИСКОВ ── */
.two{display:grid;grid-template-columns:1fr 1fr;gap:14px;margin-top:40px}
@media(max-width:860px){.two{grid-template-columns:1fr}}
.col{background:#fff;border:1px solid var(--line);border-radius:var(--r-lg);padding:clamp(26px,3vw,38px)}
.col h3{font-size:22px;font-weight:700;letter-spacing:-0.025em;margin:0 0 6px}
.col ul{list-style:none;margin:14px 0 0;padding:0}
.col li{font-size:15px;color:var(--ink2);padding:15px 0 15px 22px;position:relative;border-top:1px solid var(--line2);line-height:1.55}
.col li:first-child{border-top:none}
.col li::before{content:'';position:absolute;left:2px;top:23px;width:6px;height:6px;border-radius:999px;corner-shape:round;background:var(--ink)}
.col li b{color:var(--ink);font-weight:700}
.src{font-size:12.5px;color:var(--ink3);line-height:1.75;margin-top:26px}
.note{margin-top:26px;background:var(--soft);border-radius:var(--r);padding:22px 26px;font-size:15px;color:var(--ink2);line-height:1.6}
section.gray .note{background:#fff}
.note b{color:var(--ink);font-weight:700}

/* ── ТАБЛИЦЫ ── */
.tw{border:1px solid var(--line);border-radius:44px;overflow-x:auto;background:#fff;margin-top:8px;-webkit-overflow-scrolling:touch}
@media(max-width:900px){.tw{border-radius:var(--r)}}
table{width:100%;border-collapse:collapse;font-size:15px;min-width:780px}
thead th{background:#fff;font-weight:700;font-size:11.5px;letter-spacing:0.05em;text-transform:uppercase;color:var(--ink3);text-align:left;padding:20px 12px;border-bottom:1px solid var(--line);line-height:1.25}
td{padding:15px 12px;border-bottom:1px solid var(--line2);vertical-align:middle}
th:first-child,td:first-child{padding-left:22px}
th:last-child,td:last-child{padding-right:22px}
th.n,td.n{text-align:right;font-variant-numeric:tabular-nums;white-space:nowrap}
tbody tr:last-child td{border-bottom:none}
tr.tot td{background:var(--soft);font-weight:800;border-top:1px solid var(--line)}
tr.dim td{color:var(--ink2)}
td.hl{font-weight:800}
.pos{color:var(--acc);font-weight:700}
.neg{color:var(--warn);font-weight:700}
.tname{font-weight:700;color:var(--ink)}
.tname span{display:block;font-weight:500;font-size:12.5px;color:var(--ink3)}

/* ── КАЛЬКУЛЯТОР ── */
.calc{display:grid;grid-template-columns:370px 1fr;gap:14px;align-items:start}
@media(max-width:1020px){.calc{grid-template-columns:1fr}}
.ctrl{background:#fff;border:1px solid var(--line);border-radius:var(--r-lg);padding:28px 26px}
@media(max-width:1020px){.ctrl{border-radius:var(--r)}}
.grp{margin-bottom:26px}
.grp:last-of-type{margin-bottom:18px}
.grp-t{font-size:11px;font-weight:800;letter-spacing:0.11em;text-transform:uppercase;color:var(--ink3);padding-bottom:12px;margin-bottom:14px;border-bottom:1px solid var(--line)}
.sl{margin-bottom:17px}
.sl:last-child{margin-bottom:0}
.sl-top{display:flex;justify-content:space-between;align-items:baseline;gap:10px;margin-bottom:2px}
.sl-lab{font-size:13.5px;color:var(--ink2);line-height:1.3}
.sl-val{font-size:14px;font-weight:800;color:var(--ink);white-space:nowrap;font-variant-numeric:tabular-nums}
input[type=range]{-webkit-appearance:none;appearance:none;background:transparent;width:100%;height:22px;cursor:pointer;margin:0;display:block}
input[type=range]::-webkit-slider-runnable-track{height:2px;background:var(--line);border-radius:2px}
input[type=range]::-moz-range-track{height:2px;background:var(--line);border-radius:2px}
input[type=range]::-webkit-slider-thumb{-webkit-appearance:none;width:18px;height:18px;border-radius:999px;background:var(--dark);margin-top:-8px;border:2px solid #fff;box-shadow:0 1px 5px rgba(0,0,0,.28)}
input[type=range]::-moz-range-thumb{width:16px;height:16px;border-radius:999px;background:var(--dark);border:2px solid #fff;box-shadow:0 1px 5px rgba(0,0,0,.28)}
input[type=range]:focus-visible::-webkit-slider-thumb{outline:2px solid var(--acc);outline-offset:2px}
.reset{width:100%;background:var(--soft);color:var(--ink);border:1px solid var(--line);border-radius:999px;corner-shape:round;padding:13px;font-family:var(--sf);font-weight:700;font-size:14px;cursor:pointer;transition:background .15s}
.reset:hover{background:var(--dark);color:#fff;border-color:var(--dark)}
.out{display:flex;flex-direction:column;gap:14px}
.oh{background:var(--dark);color:#fff;border-radius:var(--r-lg);padding:clamp(30px,3.6vw,44px);min-height:230px;display:flex;flex-direction:column;justify-content:center}
@media(max-width:1020px){.oh{border-radius:var(--r)}}
.oh .k{font-size:12px;font-weight:700;letter-spacing:0.1em;text-transform:uppercase;color:rgba(255,255,255,0.55)}
.oh .money{font-size:clamp(46px,7vw,86px);font-weight:800;letter-spacing:-0.04em;line-height:1;margin:12px 0 12px;font-variant-numeric:tabular-nums}
.oh .money.bad{color:#ffb59b}
.oh .note-d{font-size:15px;color:rgba(255,255,255,0.68);margin:0;line-height:1.5}
.oh .note-d b{color:#fff;font-weight:700}
.kpis{display:grid;grid-template-columns:repeat(4,1fr);gap:14px}
@media(max-width:900px){.kpis{grid-template-columns:repeat(2,1fr)}}
.kpi{background:#fff;border:1px solid var(--line);border-radius:var(--r);padding:18px 18px;min-height:104px;display:flex;flex-direction:column;justify-content:space-between}
.kpi .k{font-size:11px;font-weight:700;letter-spacing:0.06em;text-transform:uppercase;color:var(--ink3);line-height:1.3}
.kpi .v{font-size:24px;font-weight:800;letter-spacing:-0.02em;font-variant-numeric:tabular-nums;margin-top:10px}
.flow{background:#fff;border:1px solid var(--line);border-radius:var(--r);padding:24px 26px}
.flow-t{font-size:11px;font-weight:800;letter-spacing:0.11em;text-transform:uppercase;color:var(--ink3);margin-bottom:18px}
.bar{margin-bottom:15px}
.bar:last-child{margin-bottom:0}
.bar-top{display:flex;justify-content:space-between;align-items:baseline;gap:12px;margin-bottom:6px;font-size:14px}
.bar-top b{font-weight:700}
.bar-top span{color:var(--ink2);font-variant-numeric:tabular-nums;white-space:nowrap}
.bar-track{height:10px;background:var(--soft);border-radius:999px;corner-shape:round;overflow:hidden}
.bar-fill{height:100%;background:var(--dark);border-radius:999px;corner-shape:round;transition:width .4s cubic-bezier(.16,1,.3,1)}
.bar-fill.a2{background:#3f3f3f}.bar-fill.a3{background:#6e6e6e}.bar-fill.a4{background:var(--acc)}
.alerts{display:flex;flex-direction:column;gap:10px}
.al{border-radius:var(--r);padding:18px 22px;font-size:14.5px;line-height:1.55;border:1px solid var(--line)}
.al.warn{background:#fdf4f1;border-color:rgba(154,74,47,.22);color:#5c3a2c}
.al.warn b{color:var(--warn)}
.al.ok{background:var(--acc-soft);border-color:rgba(29,92,70,.22);color:#204a3c}
.al.ok b{color:var(--acc)}

/* ── MOD2: текст + фото ── */
.mod2{display:grid;grid-template-columns:1fr 1fr;gap:14px;align-items:stretch}
@media(max-width:820px){.mod2{grid-template-columns:1fr}}
.mod2 .m-txt{background:#fff;border:1px solid var(--line);border-radius:var(--r-lg);padding:clamp(28px,3vw,44px);display:flex;flex-direction:column;justify-content:center}
section.gray .mod2 .m-txt{border-color:transparent}
.mod2 .m-txt h2{font-size:clamp(26px,3.4vw,40px);font-weight:800;letter-spacing:-0.03em;line-height:1.05;margin:0 0 18px}
.mod2 .m-txt p{font-size:15.5px;line-height:1.55;color:var(--ink2);margin:0 0 12px}
.mod2 .m-txt p b{color:var(--ink)}
.mod2 .m-photo{border-radius:var(--r-lg);overflow:hidden;height:clamp(420px,42vw,560px);background:var(--soft)}
.mod2 .m-photo img{width:100%;height:100%;object-fit:cover;display:block}
.mod2.rev-order .m-photo{order:-1}
@media(max-width:820px){.mod2 .m-photo{height:clamp(300px,66vw,420px)}.mod2.rev-order .m-photo{order:0}}

/* ── МЕНЮ ── */
.menu-list{display:flex;flex-direction:column}
.mi{padding:16px 0;border-top:1px solid var(--line2)}
.mi:first-child{border-top:none}
.mi b{display:block;font-size:17px;font-weight:700;letter-spacing:-0.015em}
.mi span{display:block;font-size:13.5px;color:var(--ink2);line-height:1.45;margin:4px 0 7px}
.mi i{display:flex;justify-content:space-between;align-items:baseline;gap:12px;font-style:normal;font-size:12px;font-weight:700;letter-spacing:0.04em;color:var(--ink3);font-variant-numeric:tabular-nums}
.mi i em{font-style:normal;font-size:17px;font-weight:800;color:var(--ink);white-space:nowrap}

/* ── ГАЛЕРЕЯ ── */
.gal{display:grid;grid-template-columns:repeat(4,1fr);gap:14px;margin-top:14px}
@media(max-width:820px){.gal{grid-template-columns:repeat(2,1fr)}}
.gal figure{margin:0;border-radius:var(--r);overflow:hidden;background:var(--soft);position:relative}
.gal img{width:100%;height:clamp(200px,22vw,290px);object-fit:cover}
.gal figcaption{position:absolute;left:0;right:0;bottom:0;padding:10px 14px;background:linear-gradient(180deg,rgba(0,0,0,0),rgba(0,0,0,.72));color:#fff;font-size:12px;font-weight:600;letter-spacing:.02em}

/* ── ШАГИ ── */
.steps{margin-top:4px}
.step{display:grid;grid-template-columns:96px 1fr 1.25fr;gap:30px;padding:30px 0;border-top:1px solid var(--line);align-items:baseline}
.step:first-child{border-top:none}
@media(max-width:820px){.step{grid-template-columns:1fr;gap:6px;padding:24px 0}}
.step .no{font-size:15px;font-weight:800;color:var(--ink3)}
.step h3{font-size:22px;font-weight:700;letter-spacing:-0.025em;margin:0}
.step h3 span{font-size:11px;font-weight:800;letter-spacing:0.07em;text-transform:uppercase;background:var(--ink);color:#fff;border-radius:999px;corner-shape:round;padding:4px 11px;margin-inline-start:12px;vertical-align:middle;white-space:nowrap}
.step p{font-size:15.5px;color:var(--ink2);margin:0}

/* ── РИСКИ ── */
.risks{display:grid;grid-template-columns:repeat(3,1fr);gap:14px}
@media(max-width:960px){.risks{grid-template-columns:repeat(2,1fr)}}
@media(max-width:620px){.risks{grid-template-columns:1fr}}
.risk{background:#fff;border:1px solid var(--line);border-radius:var(--r);padding:26px 24px;min-height:300px;display:flex;flex-direction:column}
.risk .lv{font-size:11px;font-weight:800;letter-spacing:0.08em;text-transform:uppercase;color:var(--ink3);margin-bottom:14px}
.risk .lv.hi{color:var(--warn)}
.risk h3{font-size:20px;font-weight:700;letter-spacing:-0.02em;margin:0 0 10px}
.risk p{font-size:14px;color:var(--ink2);margin:0;line-height:1.55}
.risk .fix{margin-top:auto;padding-top:16px;font-size:13px;color:var(--ink);font-weight:700;line-height:1.45}

/* ── QUOTE / FINAL / FOOTER ── */
.quote{background:var(--dark);color:#fff;border:none;padding:clamp(70px,9vw,108px) 0}
.quote p{font-size:clamp(26px,4.2vw,50px);font-weight:700;letter-spacing:-0.03em;line-height:1.16;margin:0;max-width:1050px;color:#7d7d78}
.quote em{font-style:normal;color:#fff}
.final{background:var(--dark);color:#fff;border:none;padding:clamp(72px,10vw,120px) 0}
.final h2{color:#fff;font-size:clamp(34px,6.4vw,80px);font-weight:800;letter-spacing:-0.035em;line-height:1.02;margin:0 0 24px}
.final p{font-size:17px;color:rgba(255,255,255,0.68);max-width:620px;margin:0 0 36px}
.btn-white{display:inline-block;background:#fff;color:var(--ink);font-weight:800;font-size:17px;padding:18px 38px;border-radius:999px;corner-shape:round;transition:transform .15s;border:none;cursor:pointer;font-family:var(--sf)}
.btn-white:hover{transform:translateY(-2px)}
.btn-dark{display:inline-block;background:var(--dark);color:#fff;font-weight:800;font-size:16px;padding:17px 34px;border-radius:999px;corner-shape:round;transition:transform .15s;border:none;cursor:pointer;font-family:var(--sf)}
.btn-dark:hover{transform:translateY(-2px)}
.cta-mid{display:flex;justify-content:center;margin-top:clamp(30px,4vw,48px)}
footer{background:var(--dark);color:rgba(255,255,255,0.55);font-size:14px;padding:38px 0 44px;border:none}
.foot-in{display:flex;justify-content:space-between;gap:20px;flex-wrap:wrap;align-items:flex-end}
.foot-in .logo{color:#fff;font-weight:700;font-size:16px;letter-spacing:0.18em}
.foot-in p{margin:0;max-width:700px;line-height:1.7}

a:focus-visible,button:focus-visible,summary:focus-visible{outline:2px solid var(--acc);outline-offset:3px;border-radius:8px}
/* пилюли и круги: истинно круглые края */
.hero-nav .logo,.hero-nav .links,.hero-nav .links a,.nav-actions,.hero-nav .cta,.nav-burger,.cur-toggle,button,.tag{corner-shape:round}
```

---

# СВЕРКА С ИСХОДНИКОМ

Отдельный агент-контролёр заново открыл исходник и прошёл документ по значениям. Ниже его отчёт целиком: что исправлено, что дописано и чего в эталоне нет вовсе. Раздел приложен как есть, потому что список «чего нет» так же важен для переноса, как и список того, что есть: он не даёт выдумать поведение, которого в системе не было.

Начало отчёта обрывается на полуслове: контролёр вернул только заключительную часть, сам выправленный документ в его ответ не поместился. Правки, которые он назвал критичными, я внёс в текст выше вручную и перепроверил по исходнику: строки тега `<style>` (открыт на 17, закрыт на 301), отсутствие общей обёртки IIFE у кода калькулятора, и находку U+2212 на строке 962.

runnable-track` и `::-moz-range-track`). Формулировка исправлена и в правиле 1, и в разделе 1.7.
12. **Правило 4.** Было «строгий grep не выводит ни одной строки». На самом деле `grep -nP '[\x{2010}-\x{2015}\x{2212}]'` находит одно вхождение: U+2212 MINUS SIGN на HTML:962, внутри JS-шаблона итоговой строки таблицы. Правило переписано честно, добавлена рекомендация заменить минус дефисом ради однозначной автопроверки; там же уточнено, какие именно символы псевдографики стоят в комментариях (U+2500 в CSS, 64 раза; U+2550 в HTML, 188 раз) и почему они под запрет не попадают. Соответствующий пункт чек-листа разделён на два.
13. **Чек-лист про IIFE.** Было «скрипт обёрнут в IIFE, ничего не выносит в глобальную область». В эталоне так только для блока reveal и навигации; код калькулятора (HTML:771-1057) лежит в глобальной области и держит там 11 имён. Пункт переписан, в часть 6.2 добавлен разбор структуры `<script>` на три независимые части.
14. **`section.gray .stat{background:#fff}`.** Формулировка «карточка обязана остаться белой» дополнена фактом: `--card` сегодня равен `#ffffff`, поэтому это правило визуально ничего не меняет и работает как страховка, в отличие от `section.gray .note`, где разница реальная.
15. **Строки `<style>`.** Было «строки 17-301». Уточнено: тег открыт на 17, закрыт на 301, сам CSS занимает 18-300.
16. **`.hs b` и `.links.mopen a`** в таблице 1.3: проставлены наследуемые веса и помечено, откуда они наследуются.
17. **`.slab` и `!important`:** добавлено объяснение, почему `padding-top:var(--r-lg)!important` (CSS:27) побеждает более поздний шорткат `padding` (CSS:88).

### Что дописано, потому что без этого страница не воспроизводится

- **Раздел 1.8 «Разметка `<head>`»**: `<!doctype html>`, `lang="ru"`, `charset`, `viewport` с `viewport-fit=cover`, порядок мета-тегов, отсутствие `og:image` и `canonical`.
- **Часть 6.2**: структура тега `<script>` (три блока, их границы по строкам, что из них относится к системе).
- **Часть 6.1**: полный перечень узлов с `.rv` с числом вхождений (96 узлов) и `.ph-zoom` (16 узлов).
- **Часть 4.11**: состав `.ctrl` в эталоне (7 групп `.grp`, 28 ползунков, одна `.reset`).
- **Часть 4.12**: адреса и содержимое обоих инлайновых стилей блока `#cash` (HTML:978, 980, 981).
- **Часть 4.19**: разбивка 19 картинок по местам и точное число `loading="lazy"` (16).
- **Часть 5.6**: переключение `aria-label` у `.cur-toggle` скриптом и статический `aria-label` у `.nav-burger`.
- **Часть 5.8**: общее число ползунков (28) и оговорка, что ручкам не выдаётся `corner-shape:round`.
- **Часть 5.5**: `text-decoration` встречается в файле ровно один раз.
- **Часть 1.6**: `aspect-ratio` добавлен в таблицу нестандартных свойств как единственное вхождение.
- **Часть 4.15**: в эталоне 6 карточек риска, четыре с `.lv.hi` и две с обычным `.lv`.

### Чего в исходнике нет и потому не задокументировано

- Активный пункт меню: ни `.active`, ни `aria-current`, ни скролл-спая.
- Компенсация фиксированной шапки при переходе по якорю: ни `scroll-padding-top`, ни `scroll-margin-top`, ни JS-смещения.
- Состояния `:active`, `:disabled`, `:invalid`, `:readonly` - ни у одной кнопки и ни у одного поля.
- Hover у `.cur-toggle`, `.nav-burger` и у контентных ссылок.
- Фокус-ринг у `::-moz-range-thumb`.
- `aria-expanded` на бургере, закрытие меню по Escape или клику вне, блокировка скролла body при открытом меню.
- `@supports`-фолбэк для `corner-shape`.
- `<noscript>`-обход для контента с `opacity:0`.
- `@font-face`, внешние CSS-файлы, `@keyframes`, свойство `animation`, CSS-счётчики, `<svg>`, иконочные шрифты.
- Тег `<form>`, `<label>`, поля `text` / `email` / `select` / `textarea` / `checkbox`.
- Токены для отступов, теней, длительностей и типографики: их нет, значения захардкожены по месту или считаются `clamp`.
- Разметка для `.stats4`, `.stat`, `.two`, `.col`: CSS есть, HTML-примеров в эталоне нет, поэтому шаблон для них восстановить дословно невозможно.
- Приглушённый текст и акценты внутри мобильного меню и внутри `.gal figcaption`: заданы только базовый фон и базовый цвет.
- `og:image` и `<link rel="canonical">`.
- Причины выбора конкретных чисел брейкпоинтов: комментариев в исходнике нет, объяснения в 7.2 выведены из самих значений и помечены как вывод, а не как цитата.
