# Дизайн сайту-портфоліо — Mykhailo Doronin

Джерело дизайну для персонального сайту-портфоліо, написане мовою **[`.dac`](https://github.com/DorMiwww/Vireo)** —
власною Design-as-Code мовою проєкту [Vireo](https://github.com/DorMiwww/Vireo). Дизайн — це не картинка й не Figma-файл,
а код: компоненти, властивості й токени, з яких компілятор Vireo генерує JSON, HTML або Figma-документ.

Контент і факти про мене взяті з реального профілю [github.com/DorMiwww](https://github.com/DorMiwww)
(біо, стек, репозиторії) — нічого вигаданого.

---

## Структура проєкту

```
design/
├── README.md
├── .gitignore
├── preview.html          # згенерований HTML-прев'ю, з усіма медіа вбудованими base64 (є в .gitignore)
├── .github/
│   └── workflows/
│       └── deploy.yml     # CI: vireo check → render --embed-assets → GitHub Pages
└── dac/
    ├── portfolio.dac      # головний файл: NavBar → Hero → Activity → About → Skills → Projects → Contact → Footer
    ├── tokens.dac          # палітра кольорів і 8pt spacing-скейл (var/fun), довідково
    ├── components/
    │   ├── buttons.dac     # Primary / Ghost / Outline кнопки
    │   ├── badges.dac      # Skill-пігулки, Status-індикатор, Tag-лейбл
    │   └── nav.dac         # пункт навігаційного меню
    └── assets/
        ├── wheel.svg               # реальний штурвал з твого GitHub-профілю (Hero-візуал)
        ├── vireo-preview.png       # реальний скрін dashboard.dac, зменшений sips-ом (Featured project)
        └── icons-*.svg (x5)        # skillicons.dev, завантажені один раз локально (Skills-картки)
```

Це майже точно та структура, яку сам `vireo init` пропонує за замовчуванням (`tokens.dac` + `components/*.dac` +
головний екран), тільки під один сторінковий сайт замість дизайн-системи з кількома екранами.

---

## Як подивитись результат

Встановлений CLI (`vireo --version` має показати `0.2.0` або новіше — медіа-властивості з'явились саме в 0.2.0):

```bash
curl -fsSL https://raw.githubusercontent.com/DorMiwww/Vireo/main/install.sh | bash
```

Перевірити, що дизайн валідний (парситься, всі `ref:`/`import` резолвляться):

```bash
vireo check dac/portfolio.dac
```

Згенерувати HTML-прев'ю (той самий `preview.html`, що вже лежить у цій папці):

```bash
vireo render dac/portfolio.dac -o html --theme dark --title "Mykhailo Doronin — DevOps Engineer" --embed-assets -o preview.html
```

`--embed-assets` пакує `wheel.svg` і `vireo-preview.png` як base64 прямо у файл — `preview.html` лишається
одним самодостатнім файлом, який можна відкрити або передати будь-де без окремої папки `assets/`.

Відкрити `preview.html` у будь-якому браузері.

Також можна вивести AST у JSON або згенерувати Figma-документ (через [Vireo Figma-плагін](https://github.com/DorMiwww/Vireo/blob/main/DOCS/figma-plugin.md)):

```bash
vireo render dac/portfolio.dac -o json --pretty
vireo render dac/portfolio.dac -o figma
```

---

## Що на сторінці

- **NavBar** — лого-мітка "M", ім'я + роль, і меню *About / Skills / Projects / Contact*.
- **Hero** — двоколонковий: зліва статус-пігулка, ім'я, роль, особиста фраза
  *"I steer clusters instead of ships — same wheel, different kind of helm"* (взята з твого GitHub-профілю — той самий
  каламбур зі штурманським штурвалом і логотипом Kubernetes), опис і дві кнопки; справа — **реальний `wheel.svg`**
  з твого профілю (той самий штурвал, вже в твоїх же брендових кольорах `#326CE5`/`#1B4F9C` — нічого перефарбовувати
  не треба було).
- **Activity** *(нова секція)* — твій справжній GitHub contribution graph, зіграний змійкою
  (`github-contribution-grid-snake-dark.svg` з гілки `output` репозиторію `DorMiwww/DorMiwww`, яку генерує твій
  власний `.github/workflows/snake.yml`). Це живі дані, не картинка-заглушка.
- **About** — чим займаєшся щодня (Docker/Kubernetes, Terraform, GitOps/ArgoCD, CI/CD, observability) + 4 картки-факти
  (локація, фокус, backend-корені, рік на GitHub).
- **Skills** — 5 карток по категоріях (Containers & Orchestration, Cloud & Infrastructure, CI/CD, Observability,
  Languages & Data), кожна тепер із рядком іконок (з `skillicons.dev`, той самий сервіс, що вже в твоєму README —
  але завантажений один раз і збережений локально, `dac/assets/icons-*.svg`, а не хотлінк наживо) над текстовими
  пігулками — менш "текстова стіна", більше впізнаваних логотипів.
- **Projects** — велика картка **Vireo** як флагманський проєкт зі **справжнім скріншотом** реального рендеру
  `dashboard.dac` (той самий, що в `DOCS/images/preview-dashboard.png` цього ж репозиторію Vireo) + 3 менші картки
  (Infra Hackathon, Lighter, Kotlin Backend Template), кожна з посиланням на GitHub.
- **Contact** — email, GitHub, LinkedIn, dev.to, Stack Overflow.
- **Footer** — копірайт (слово "Vireo" — клікабельне посилання на репозиторій) + локація.

### Медіа — усе перевірене, нічого вигаданого

Жодного фейкового фото чи вигаданого стокового зображення. Кожен медіа-елемент — або твій реальний актив
(`wheel.svg`, скопійований з `DorMiwww/DorMiwww/assets/`), або реальний скріншот з цього ж репозиторію Vireo
(`vireo-preview.png`), або сервіс, який уже використовує твій власний профіль (`skillicons.dev`, GitHub
contribution snake). Перед вставкою кожен зовнішній URL перевірявся напряму (`curl`, HTTP 200 і правильна
кількість іконок у кожному SVG) — без цього легко вставити "мертве" посилання, яке виглядає правильно в коді,
але показує порожнє місце в браузері.

**Чому іконки `skillicons.dev` завантажені локально, а не хотлінкнуті:** на живому сайті частина іконок
у стрічках зникала (окремі `<img>`-запити до `skillicons.dev` час від часу не встигали/не проходили) — типова
крихкість стороннього сервісу під навантаженням реальних відвідувачів, яку неможливо було відтворити разовим
`curl`-запитом під час розробки. Тому кожну з 5 іконок-стрічок один раз завантажено (`dac/assets/icons-*.svg`,
з підтвердженою правильною кількістю іконок у кожному файлі) і тепер вони вбудовуються в `index.html` через
`--embed-assets`, як і `wheel.svg`/`vireo-preview.png` — жодної залежності від стороннього сервісу під час
показу сайту відвідувачу. GitHub-змійку в Activity свідомо лишив хотлінком — вона має показувати актуальні дані,
а не застиглий знімок.

### Про навігацію

Пункти меню (*About / Skills / Projects / Contact*) намальовані як справжній навбар — візуально, з правильними
стилями й hover-станами, так само як пункти меню в референсних `dashboard.dac`-прикладах Vireo. Це навігація
*в дизайні*, а не готовий production-сайт: поточний HTML-рендерер Vireo не проставляє `id` на компоненти й завжди
відкриває `href` у новій вкладці (`target="_blank"`), тож `href="#about"` не проскролив би сторінку коректно.
Тому внутрішні пункти меню без `href` — а зовнішні посилання (GitHub, LinkedIn, dev.to, Stack Overflow, email)
навпаки всі клікабельні й відкриваються правильно, це вже перевірено в `preview.html`.

Коли дійде до реальної верстки (React/Next.js/будь-що) — той самий `.dac` буде однозначним technical spec для
розробки: усі кольори, відступи, розміри й тексти вже зафіксовані в коді, а не в чиїйсь голові.

---

## Палітра (`dac/tokens.dac`)

Кольори взяті з banner-градієнта твого власного GitHub-профілю (`0F2027 → 326CE5 → 1B4F9C`), а не вигадані з нуля:

| Токен | Значення | Призначення |
|---|---|---|
| `brand` | `#326CE5` | Kubernetes-синій — акценти, кнопки, роль |
| `brandDark` | `#1B4F9C` | темніший акцент |
| `pageBg` | `#0B1220` | фон сторінки (глибокий navy) |
| `surface` / `surfaceAlt` | `#101A2E` / `#0D1626` | картки й альтернативні секції |
| `textPrimary` / `textMuted` / `textFaint` | `#E7ECF5` / `#8A94A6` / `#5B6472` | ієрархія тексту |
| `success` | `#34D399` | статус-індикатор |

Spacing — 8pt-грід через функцію `spacing(n) = 8 * n` (демо в `tokens.dac`, показує підтримку `fun` у `.dac`).

---

## Деплой на GitHub Pages

`.github/workflows/deploy.yml` автоматично білдить і публікує сайт при кожному push у `main` (гілка з файлами
під `dac/**`), або вручну через вкладку Actions → Run workflow.

**Що робить workflow:**
1. Ставить JDK 21 (`actions/setup-java`) — Vireo чистий JVM-інструмент.
2. Встановлює `vireo` офіційним `install.sh` (`curl -fsSL .../install.sh | bash`). Готового GitHub Release під
   Vireo ще нема (Phase 6 в роадмапі не завершена), тож інсталятор сам збере CLI з джерела через Gradle —
   довше (~1-3 хв), але працює однаково надійно; коли релізи з'являться, це прискориться само собою.
3. `vireo check dac/portfolio.dac` — збірка падає, якщо дизайн зламаний (лінт перед деплоєм).
4. `vireo render dac/portfolio.dac -o html --embed-assets -o dist/index.html` — той самий self-contained файл,
   що й локальний `preview.html`, тільки під назвою `index.html` (потрібно GitHub Pages).
5. Публікує `dist/` через `actions/upload-pages-artifact` + `actions/deploy-pages` (сучасний, Actions-based
   спосіб — не легасі-гілка `gh-pages`).

**Що треба зробити один раз вручну на GitHub (я не можу зробити це за тебе):**
1. Створити новий репозиторій **з назвою точно `DorMiwww.github.io`** — для User Page GitHub вимагає саме таку
   назву (не "portfolio", не "website" — інша назва дасть Project Page на іншій адресі).
2. Запушити вміст цієї теки (`design/`) як корінь того репозиторію (тобто `README.md`, `.gitignore`, `dac/`,
   `.github/` мають лежати прямо в корені репо, не у вкладеній підпапці).
3. У репозиторії: Settings → Pages → Source → **GitHub Actions** (не "Deploy from a branch" — інакше цей
   workflow ніхто не використає).
4. Після першого успішного прогону сайт з'явиться на `https://dormiwww.github.io/`.

Git у цій теці зараз немає (свідомо — дивись історію розмови), тож `git init` / створення репозиторію на GitHub —
на твій розсуд і твоїми руками.

---

## Примітка щодо мови

Контент самого сайту (`dac/*.dac`) написаний англійською — так само, як і твій публічний GitHub-профіль
(README, bio), для консистентності з існуючим брендом. Цей README — українською, бо це внутрішня документація
репозиторію, не публічна сторінка.
