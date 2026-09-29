# Практическое занятие №1

Выполнил: (Букин Кирилл Алексеевич)

Группа: (ИКБО-16-25)

---

## Задача 1

```
grep -o '^[^:]*' /etc/passwd | sort
```

Результат:

```
_apt
_chrony
backup
bin
daemon
dhcpcd
games
irc
kolik
landscape
list
lp
mail
man
messagebus
news
nobody
polkitd
proxy
root
sync
sys
syslog
systemd-network
systemd-resolve
uucp
www-data
```

## Задача 2

```
grep -v '^#' /etc/protocols | awk 'NF {print $2, $1}' | sort -rn | head -5
```

Результат:

```
262 mptcp
143 ethernet
142 rohc
141 wesp
140 shim6
```

## Задача 3

Файл banner:

```
#!/usr/bin/env bash
# banner - выводит текст в рамке; ширина рамки зависит от длины текста

if [ $# -lt 1 ]; then
    echo "Использование: $0 \"текст\"" >&2
    exit 1
fi

text="$*"
len=${#text}
border="+$(printf '%*s' $((len + 2)) '' | tr ' ' '-')+"

echo "$border"
echo "| $text |"
echo "$border"
```

Запуск:

```
./banner "Hello from RTU MIREA!"
./banner "Hi"
```

Результат:

```
+-----------------------+
| Hello from RTU MIREA! |
+-----------------------+
+----+
| Hi |
+----+
```

## Задача 4

Файл identifiers.sh:

```
#!/usr/bin/env bash
# identifiers.sh - выводит все уникальные идентификаторы (правила C/C++/Java) из файла

if [ $# -ne 1 ] || [ ! -f "$1" ]; then
    echo "Использование: $0 файл" >&2
    exit 1
fi

grep -oE '\b[A-Za-z_][A-Za-z0-9_]*\b' "$1" | sort -u | tr '\n' ' '
echo
```

Файл hello.c:

```
#include <stdio.h>

int main(void)
{
    printf("hello, world\n");
    return 0;
}
```

Запуск:

```
./identifiers.sh hello.c
```

Результат:

```
h hello include int main n printf return stdio void world
```

## Задача 5

Файл reg:

```
#!/usr/bin/env bash
# reg - регистрирует пользовательскую команду: задаёт права и копирует в /usr/local/bin

DEST_DIR="${DEST_DIR:-/usr/local/bin}"

if [ $# -ne 1 ]; then
    echo "Использование: $0 файл_команды" >&2
    exit 1
fi

file="$1"

if [ ! -f "$file" ]; then
    echo "Ошибка: файл '$file' не найден" >&2
    exit 1
fi

chmod 755 "$file" || exit 1
cp "$file" "$DEST_DIR/" || exit 1
echo "Команда '$(basename "$file")' зарегистрирована в $DEST_DIR"
```

Запуск:

```
sudo ./reg banner
ls -l /usr/local/bin/banner
cd / && banner "Works everywhere"
```

Результат:

```
Команда 'banner' зарегистрирована в /usr/local/bin
-rwxr-xr-x 1 root root 366 Sep 21 17:35 /usr/local/bin/banner
+------------------+
| Works everywhere |
+------------------+
```

## Тестовые данные для задач 6–10

```
mkdir -p test/sub
printf '// comment\nint main(){}\n' > test/a.c
printf 'int main(){}\n'            > test/b.c
printf '/* comment */\nvar x;\n'   > test/c.js
printf 'var x;\n'                  > test/d.js
printf '# comment\nprint(1)\n'     > test/e.py
printf 'print(1)\n'                > test/sub/f.py
echo same  > test/x1.txt; echo same  > test/sub/x2.txt; echo same > test/x3.log
echo other > test/y1.txt; echo other > test/sub/y2.txt
: > test/empty1.txt; : > test/empty2.txt
printf 'int main()\n{\n    if (1)\n        return 0;\n}\n' > test/in.txt
```

## Задача 6

Файл check_comment.sh:

```
#!/usr/bin/env bash
# check_comment.sh - проверяет наличие комментария в первой строке файлов .c, .js, .py

dir="${1:-.}"

if [ ! -d "$dir" ]; then
    echo "Использование: $0 [каталог]" >&2
    exit 1
fi

while IFS= read -r -d '' f; do
    first_line=$(head -n 1 "$f")
    case "$f" in
        *.c|*.js) pattern='^[[:space:]]*(//|/\*)' ;;
        *.py)     pattern='^[[:space:]]*#' ;;
    esac
    if [[ $first_line =~ $pattern ]]; then
        echo "ЕСТЬ: $f"
    else
        echo "НЕТ:  $f"
    fi
done < <(find "$dir" -type f \( -name '*.c' -o -name '*.js' -o -name '*.py' \) -print0)
```

Запуск:

```
./check_comment.sh test
```

Результат:

```
НЕТ:  test/sub/f.py
ЕСТЬ: test/e.py
НЕТ:  test/d.js
ЕСТЬ: test/c.js
НЕТ:  test/b.c
ЕСТЬ: test/a.c
```

## Задача 7

Файл find_dups.sh:

```
#!/usr/bin/env bash
# find_dups.sh - находит файлы-дубликаты (по содержимому) в каталоге и подкаталогах

dir="${1:-.}"

if [ ! -d "$dir" ]; then
    echo "Использование: $0 [каталог]" >&2
    exit 1
fi

find "$dir" -type f -print0 \
    | xargs -0 -r sha256sum \
    | sort \
    | uniq -w64 --all-repeated=separate
```

Запуск:

```
./find_dups.sh test
```

Результат:

```
7e4fa2eb8c7ac089739d5defc4489fad68a100d92082ca35c6b40a4524821f87  test/sub/y2.txt
7e4fa2eb8c7ac089739d5defc4489fad68a100d92082ca35c6b40a4524821f87  test/y1.txt

a6328afc76e9db71da297ebff4b0d3e7a7eb3b01d917c05a6573fef121b6ecb6  test/sub/x2.txt
a6328afc76e9db71da297ebff4b0d3e7a7eb3b01d917c05a6573fef121b6ecb6  test/x1.txt
a6328afc76e9db71da297ebff4b0d3e7a7eb3b01d917c05a6573fef121b6ecb6  test/x3.log

e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855  test/empty1.txt
e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855  test/empty2.txt
```

## Задача 8

Файл archive_ext.sh:

```
#!/usr/bin/env bash
# archive_ext.sh - архивирует в tar все файлы каталога с заданным расширением

if [ $# -lt 1 ] || [ $# -gt 2 ]; then
    echo "Использование: $0 расширение [каталог]" >&2
    exit 1
fi

ext="${1#.}"
dir="${2:-.}"
archive="files_${ext}.tar"

if [ ! -d "$dir" ]; then
    echo "Ошибка: каталог '$dir' не найден" >&2
    exit 1
fi

mapfile -d '' files < <(find "$dir" -maxdepth 1 -type f -name "*.${ext}" -printf '%f\0')

if [ ${#files[@]} -eq 0 ]; then
    echo "Файлов с расширением .$ext в '$dir' не найдено"
    exit 0
fi

tar -cf "$archive" -C "$dir" -- "${files[@]}" && echo "Создан архив $archive (файлов: ${#files[@]})"
```

Запуск:

```
./archive_ext.sh c test
tar -tf files_c.tar
./archive_ext.sh rs test
```

Результат:

```
Создан архив files_c.tar (файлов: 2)
b.c
a.c
Файлов с расширением .rs в 'test' не найдено
```

## Задача 9

Файл spaces2tabs.sh:

```
#!/usr/bin/env bash
# spaces2tabs.sh - заменяет последовательности из 4 пробелов на символ табуляции

if [ $# -ne 2 ]; then
    echo "Использование: $0 входной_файл выходной_файл" >&2
    exit 1
fi

if [ ! -f "$1" ]; then
    echo "Ошибка: файл '$1' не найден" >&2
    exit 1
fi

sed 's/ \{4\}/\t/g' "$1" > "$2"
```

Запуск:

```
./spaces2tabs.sh test/in.txt test/out.txt
cat -A test/out.txt
```

Результат:

```
int main()$
{$
^Iif (1)$
^I^Ireturn 0;$
}$
```

## Задача 10tk

Файл empty_files.sh:

```
#!/usr/bin/env bash
# empty_files.sh - выводит имена пустых текстовых (.txt) файлов в указанной директории

if [ $# -ne 1 ] || [ ! -d "$1" ]; then
    echo "Использование: $0 каталог" >&2
    exit 1
fi

find "$1" -maxdepth 1 -type f -name '*.txt' -empty -printf '%f\n'
```

Запуск:

```
./empty_files.sh test
```

Результат:

```
empty1.txt
empty2.txt
```
