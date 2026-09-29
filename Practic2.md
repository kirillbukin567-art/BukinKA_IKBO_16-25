# Практическое занятие №2

Выполнил: (Букин Кирилл Алексеевич)

Группа: (ИКБО-16-255)

---

## Задача 1

```
sudo apt install -y python3-pip
pip3 install matplotlib --break-system-packages
python3 -m pip show matplotlib
```

Результат:

```
Name: matplotlib
Version: 3.11.2
Summary: Python plotting package
Home-page: https://matplotlib.org
Author: John D. Hunter, Michael Droettboom
Author-email: Unknown <matplotlib-users@python.org>
License: License agreement for matplotlib versions 1.3.0 and later
 (полный текст лицензии — matplotlib включает лицензии сторонних
  шрифтов и библиотек: FreeType, HarfBuzz, QHull и другие)
Location: /home/kolik/.local/lib/python3.14/site-packages
Requires: contourpy, cycler, fonttools, kiwisolver, numpy, packaging, pillow, pyparsing, python-dateutil
Required-by:
```

Основные поля: `Name`/`Version` — имя и версия пакета; `Summary` — краткое описание; `License` — лицензия (у matplotlib включает лицензии встроенных шрифтов); `Location` — куда пакет установлен на диске; `Requires` — прямые зависимости; `Required-by` — что зависит от этого пакета (пусто).

Получение пакета без менеджера пакетов, прямо из репозитория (PyPI):

```
pip3 download matplotlib --no-deps -d .
unzip -l matplotlib-*.whl | head -20
```

Результат:

```
Saved ./matplotlib-3.11.2-cp314-cp314-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl
Successfully downloaded matplotlib

Archive:  matplotlib-3.11.2-cp314-cp314-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl
  Length      Date    Time    Name
---------  ---------- -----   ----
      110  2026-09-11 18:31   pylab.py
        0  2026-09-11 18:31   matplotlib/
    56239  2026-09-11 18:31   matplotlib/__init__.py
     ...
```

`pip download` скачивает файл пакета (`.whl` — обычный ZIP-архив), не устанавливая его. Имя файла кодирует версию (`3.11.2`), версию Python (`cp314` — CPython 3.14) и платформу (`manylinux_..._x86_64`).

Альтернатива — исходники прямо с GitHub:

```
git clone https://github.com/matplotlib/matplotlib.git --depth 1
```

---

## Задача 2

```
sudo apt install -y nodejs npm
node --version
npm --version
npm view express
```

Результат:

```
v22.22.1
9.2.0

express@5.2.1 | MIT | deps: 28 | versions: 289
Fast, unopinionated, minimalist web framework
https://expressjs.com/

keywords: express, framework, sinatra, web, http, rest, restful, router, app, api

dist
.tarball: https://registry.npmjs.org/express/-/express-5.2.1.tgz
.shasum: 8f21d15b6d327f92b4794ecf8cb08a72f956ac04
.integrity: sha512-hIS4idWWai69NezIdRt2xFVofaF4j+6INOpJlVOLDO8zXGpUVEVzIYk12UUi2JzjEzWL3IOAxcTubgz9Po0yXw==
.unpackedSize: 75.4 kB

dependencies:
accepts: ^2.0.0      etag: ^1.8.1         proxy-addr: ^2.0.7
body-parser: ^2.2.1  finalhandler: ^2.1.0 qs: ^6.14.0
content-type: ^1.0.5 fresh: ^2.0.0        range-parser: ^1.2.1
cookie: ^0.7.1       http-errors: ^2.0.0  router: ^2.2.0
debug: ^4.4.0        mime-types: ^3.0.0   send: ^1.1.0
depd: ^2.0.0         on-finished: ^2.4.1  statuses: ^2.0.1
encodeurl: ^2.0.0    once: ^1.4.0         type-is: ^2.0.1
escape-html: ^1.0.3  parseurl: ^1.3.3     vary: ^1.1.2
(...and 4 more.)

maintainers:
- wesleytodd <wes@wesleytodd.com>
- jonchurch <npm@jonchurch.com>
- ctcpip <c@labsector.com>
- ulisesgascon <ulisesgascondev@gmail.com>
- sheplu <jean.burellier@gmail.com>

dist-tags:
latest-4: 4.22.3  latest: 5.2.1

published 10 months ago by jonchurch <npm@jonchurch.com>
```

Основные поля: первая строка — имя, версия, лицензия, число прямых зависимостей и общее число опубликованных версий; `dist.tarball` — прямая ссылка на архив пакета; `dependencies` — прямые зависимости с диапазонами semver; `maintainers` — кто может публиковать новые версии; `dist-tags` — какая версия считается «текущей».

Полный список зависимостей (без сокращения):

```
npm view express dependencies
```

Получение пакета без npm, прямо из репозитория (npm registry):

```
npm pack express
tar -tzf express-*.tgz | head -20
curl -o express-direct.tgz https://registry.npmjs.org/express/-/express-5.2.1.tgz
```

Альтернатива — исходники прямо с GitHub:

```
git clone https://github.com/expressjs/express.git --depth 1
```

---

## Задача 3

```
sudo apt install -y graphviz
dot -V
```

Файл matplotlib.dot:

```
digraph matplotlib_deps {
    rankdir=LR;
    node [shape=box, style=filled, fillcolor=lightyellow];

    "matplotlib" [fillcolor=lightblue];

    "matplotlib" -> "contourpy";
    "matplotlib" -> "cycler";
    "matplotlib" -> "fonttools";
    "matplotlib" -> "kiwisolver";
    "matplotlib" -> "numpy";
    "matplotlib" -> "packaging";
    "matplotlib" -> "pillow";
    "matplotlib" -> "pyparsing";
    "matplotlib" -> "python-dateutil";
}
```

```
dot -Tpng matplotlib.dot -o matplotlib.png
```

Файл express.dot (полный список из 28 зависимостей, полученный командой `npm view express dependencies`):

```
digraph express_deps {
    rankdir=LR;
    node [shape=box, style=filled, fillcolor=lightyellow];

    "express" [fillcolor=lightgreen];

    "express" -> "accepts";
    "express" -> "body-parser";
    "express" -> "content-disposition";
    "express" -> "content-type";
    "express" -> "cookie";
    "express" -> "cookie-signature";
    "express" -> "debug";
    "express" -> "depd";
    "express" -> "encodeurl";
    "express" -> "escape-html";
    "express" -> "etag";
    "express" -> "finalhandler";
    "express" -> "fresh";
    "express" -> "http-errors";
    "express" -> "merge-descriptors";
    "express" -> "mime-types";
    "express" -> "on-finished";
    "express" -> "once";
    "express" -> "parseurl";
    "express" -> "proxy-addr";
    "express" -> "qs";
    "express" -> "range-parser";
    "express" -> "router";
    "express" -> "send";
    "express" -> "serve-static";
    "express" -> "statuses";
    "express" -> "type-is";
    "express" -> "vary";
}
```

```
dot -Tpng express.dot -o express.png
```

Результат — изображения matplotlib.png и express.png (приложены отдельными файлами).

---

## Задача 4

Установка: MiniZinc IDE, https://www.minizinc.org/software.html, версия для Windows (bundled, со встроенным решателем Gecode).

Файл happy_tickets.mzn:

```
include "globals.mzn";

int: n = 6;
array[1..n] of var 0..9: d;

constraint all_different(d);

var 0..24: sum1 = d[1] + d[2] + d[3];
var 0..24: sum2 = d[4] + d[5] + d[6];

constraint sum1 = sum2;

solve minimize sum1;

output [
  "Билет: ", show(d[1]), show(d[2]), show(d[3]), show(d[4]), show(d[5]), show(d[6]), "\n",
  "Сумма (первые 3 = последние 3) = ", show(sum1), "\n"
];
```

Запуск: MiniZinc IDE, кнопка Run (F6).

Результат:

```
Билет: 432810
Сумма (первые 3 = последние 3) = 9
----------
Билет: 431620
Сумма (первые 3 = последние 3) = 8
----------
==========
```

Минимальная сумма — 8, билет 431620 (4+3+1 = 6+2+0 = 8), все 6 цифр различны. Двойное подчёркивание `==========` означает, что решение доказано оптимальным.

---

## Задача 5

Рисунок: пакеты root, menu (версии 1.0.0–1.5.0), dropdown (версии 1.8.0, 2.0.0–2.3.0), icons (версии 1.0.0, 2.0.0).

Связи по рисунку: root зависит от menu (любая версия) и от icons (конкретно 1.0.0); menu версий 1.1.0–1.5.0 зависит от dropdown версий 2.0.0–2.3.0, а menu версии 1.0.0 — от dropdown версии 1.8.0; dropdown версий 2.0.0–2.3.0 зависит от icons версии 2.0.0.

Файл task5_deps.mzn:

```
include "globals.mzn";

% menu:     1=1.0.0, 2=1.1.0, 3=1.2.0, 4=1.3.0, 5=1.4.0, 6=1.5.0
% dropdown: 1=1.8.0, 2=2.0.0, 3=2.1.0, 4=2.2.0, 5=2.3.0
% icons:    1=1.0.0, 2=2.0.0

var 1..6: menu;
var 1..5: dropdown;
var 1..2: icons;

constraint icons = 1;

constraint (menu >= 2) -> (dropdown >= 2);
constraint (menu = 1)  -> (dropdown = 1);

constraint (dropdown >= 2) -> (icons = 2);

solve satisfy;

output [
  "menu     = ", show(menu), "\n",
  "dropdown = ", show(dropdown), "\n",
  "icons    = ", show(icons), "\n"
];
```

Результат:

```
menu     = 1
dropdown = 1
icons    = 1
```

То есть menu=1.0.0, dropdown=1.8.0, icons=1.0.0 — единственное возможное сочетание. Любая версия menu, кроме 1.0.0, тянет dropdown линейки 2.x, которая, в свою очередь, требует icons=2.0.0 — а это противоречит прямому требованию root: icons=1.0.0. Конфликт снимается только при menu=1.0.0, потому что она тянет dropdown=1.8.0, а эта версия dropdown зависимости от icons вообще не имеет.

---

## Задача 6

Условие:

```
root 1.0.0 зависит от foo ^1.0.0 и target ^2.0.0.
foo 1.1.0 зависит от left ^1.0.0 и right ^1.0.0.
foo 1.0.0 не имеет зависимостей.
left 1.0.0 зависит от shared >=1.0.0.
right 1.0.0 зависит от shared <2.0.0.
shared 2.0.0 не имеет зависимостей.
shared 1.0.0 зависит от target ^1.0.0.
target 2.0.0 и 1.0.0 не имеют зависимостей.
```

Файл task6_deps.mzn:

```
include "globals.mzn";

% foo:    1=1.0.0, 2=1.1.0
% left:   1=1.0.0
% right:  1=1.0.0
% shared: 1=1.0.0, 2=2.0.0
% target: 1=1.0.0, 2=2.0.0

var 1..2: foo;
var 0..1: left;
var 0..1: right;
var 0..2: shared;
var 1..2: target;

constraint target = 2;

constraint (foo = 2) -> (left != 0);
constraint (foo = 2) -> (right != 0);
constraint (right != 0) -> (shared = 1);
constraint (shared = 1) -> (target = 1);

constraint (left != 0)   <-> (foo = 2);
constraint (right != 0)  <-> (foo = 2);
constraint (shared != 0) <-> (left != 0 \/ right != 0);

solve satisfy;

output [
  "foo    = ", show(foo), "\n",
  "left   = ", show(left), "\n",
  "right  = ", show(right), "\n",
  "shared = ", show(shared), "\n",
  "target = ", show(target), "\n"
];
```

Результат:

```
foo    = 1
left   = 0
right  = 0
shared = 0
target = 2
```

То есть foo=1.0.0, target=2.0.0, а left/right/shared не устанавливаются. Жадный выбор новой версии foo=1.1.0 заводит в тупик при любой версии shared (либо конфликт с right, либо конфликт требований к target), поэтому единственное решение — откатиться к foo=1.0.0, у которой зависимостей нет вовсе.

---

## Задача 7

Общий подход: вместо ручного написания ограничений (задачи 5–6) метаданные пакетов описываются структурой данных, а ограничения для MiniZinc строятся по ней автоматически — так же, как это делает настоящий менеджер пакетов.

Файл task7_generate.py:

```
# Автоматическая генерация MiniZinc-модели по метаданным пакетов.
# Задача 7: вместо ручного написания ограничений (как в задачах 5-6)
# строим их программно по структуре данных с описанием пакетов и зависимостей.

# --- 1. Метаданные пакетов в виде словаря (данные задачи 6) ---
packages = {
    "root":   {"1.0.0": {"foo": "^1.0.0", "target": "^2.0.0"}},
    "foo":    {"1.0.0": {},
               "1.1.0": {"left": "^1.0.0", "right": "^1.0.0"}},
    "left":   {"1.0.0": {"shared": ">=1.0.0"}},
    "right":  {"1.0.0": {"shared": "<2.0.0"}},
    "shared": {"1.0.0": {"target": "^1.0.0"},
               "2.0.0": {}},
    "target": {"1.0.0": {}, "2.0.0": {}},
}

# --- 2. Разбор версий и диапазонов semver ---
def parse_version(v):
    return tuple(int(x) for x in v.split("."))

def satisfies(version, rng):
    v = parse_version(version)
    rng = rng.strip()
    if rng.startswith("^"):
        base = parse_version(rng[1:])
        upper = (base[0] + 1, 0, 0)
        return base <= v < upper
    if rng.startswith(">="):
        return v >= parse_version(rng[2:])
    if rng.startswith("<"):
        return v < parse_version(rng[1:])
    return version == rng

# --- 3. Индексация версий: 0 = "не установлен", 1..N = версии по возрастанию ---
sorted_versions = {}
version_index = {}
for pkg, vers in packages.items():
    vs = sorted(vers.keys(), key=parse_version)
    sorted_versions[pkg] = vs
    version_index[pkg] = {v: i + 1 for i, v in enumerate(vs)}

# --- 4. Сбор всех рёбер зависимостей (пакет, версия, от_чего_зависит, диапазон) ---
edges = []
for pkg, vers in packages.items():
    for v, deps in vers.items():
        for dep_pkg, rng in deps.items():
            edges.append((pkg, v, dep_pkg, rng))

# --- 5. Кто от кого зависит (в обратную сторону) ---
by_dep = {}
for (pkg, v, dep_pkg, rng) in edges:
    by_dep.setdefault(dep_pkg, []).append((pkg, v))

# --- 6. Генерация текста MiniZinc-модели ---
lines = ['include "globals.mzn";', '']

for pkg, vs in sorted_versions.items():
    n = len(vs)
    lines.append(f"var 0..{n}: {pkg};  % 0=не установлен; " +
                  ", ".join(f"{i+1}={v}" for i, v in enumerate(vs)))
lines.append("")

for (pkg, v, dep_pkg, rng) in edges:
    parent_idx = version_index[pkg][v]
    ok_indices = [version_index[dep_pkg][dv]
                  for dv in sorted_versions[dep_pkg] if satisfies(dv, rng)]
    disj = " \\/ ".join(f"{dep_pkg} = {i}" for i in ok_indices)
    lines.append(f"constraint ({pkg} = {parent_idx}) -> ({disj});"
                  f"  % {pkg} {v} требует {dep_pkg} {rng}")
lines.append("")

for pkg in packages:
    requirers = by_dep.get(pkg, [])
    if not requirers:
        continue
    disj = " \\/ ".join(f"{p} = {version_index[p][v]}" for p, v in requirers)
    lines.append(f"constraint ({pkg} != 0) -> ({disj});  % {pkg} ставим, только если он кому-то нужен")
lines.append("")

lines.append("constraint root != 0;")
lines.append("")
lines.append("solve satisfy;")
lines.append("")
lines.append('output [' + ', '.join(f'"{pkg}=", show({pkg}), "\\n"' for pkg in packages) + '];')

model_text = "\n".join(lines)
print(model_text)

with open("task7_generated.mzn", "w") as f:
    f.write(model_text)
```

```
python3 task7_generate.py
```

Автоматически сгенерированный файл task7_generated.mzn:

```
include "globals.mzn";

var 0..1: root;  % 0=не установлен; 1=1.0.0
var 0..2: foo;  % 0=не установлен; 1=1.0.0, 2=1.1.0
var 0..1: left;  % 0=не установлен; 1=1.0.0
var 0..1: right;  % 0=не установлен; 1=1.0.0
var 0..2: shared;  % 0=не установлен; 1=1.0.0, 2=2.0.0
var 0..2: target;  % 0=не установлен; 1=1.0.0, 2=2.0.0

constraint (root = 1) -> (foo = 1 \/ foo = 2);  % root 1.0.0 требует foo ^1.0.0
constraint (root = 1) -> (target = 2);  % root 1.0.0 требует target ^2.0.0
constraint (foo = 2) -> (left = 1);  % foo 1.1.0 требует left ^1.0.0
constraint (foo = 2) -> (right = 1);  % foo 1.1.0 требует right ^1.0.0
constraint (left = 1) -> (shared = 1 \/ shared = 2);  % left 1.0.0 требует shared >=1.0.0
constraint (right = 1) -> (shared = 1);  % right 1.0.0 требует shared <2.0.0
constraint (shared = 1) -> (target = 1);  % shared 1.0.0 требует target ^1.0.0

constraint (foo != 0) -> (root = 1);  % foo ставим, только если он кому-то нужен
constraint (left != 0) -> (foo = 2);  % left ставим, только если он кому-то нужен
constraint (right != 0) -> (foo = 2);  % right ставим, только если он кому-то нужен
constraint (shared != 0) -> (left = 1 \/ right = 1);  % shared ставим, только если он кому-то нужен
constraint (target != 0) -> (root = 1 \/ shared = 1);  % target ставим, только если он кому-то нужен

constraint root != 0;

solve satisfy;

output ["root=", show(root), "\n", "foo=", show(foo), "\n", "left=", show(left), "\n", "right=", show(right), "\n", "shared=", show(shared), "\n", "target=", show(target), "\n"];
```

Результат запуска task7_generated.mzn в MiniZinc IDE:

```
root=1
foo=1
left=0
right=0
shared=0
target=2
```

Результат полностью совпадает с задачей 6, полученной вручную — это подтверждает, что автоматический подход работает правильно на реальных метаданных, не переписывая ограничения заново под каждый новый набор пакетов.
