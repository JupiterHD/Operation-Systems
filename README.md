# Operation-Systems
Лабораторные работы по предмету "Операционные системы".

## Лабораторная работа 1
...

## Лабораторная работа 2
**Установка Arch Linux в виртуальной машине**

Выполнил установку Arch Linux. Процесс установки записан на видео:

[![Установка Arch Linux](https://img.shields.io/badge/Видео-Установка_Arch_Linux-red)](https://youtu.be/VFUwRH6rhLQ)

## Лабораторная работа 3

### Задание 1: Резервное копирование изображений

**Цель:** Создать скрипт для копирования всех изображений из указанной папки в резервную папку.

**Функционал:**
1. Проверка корректности введенных параметров
2. Автоматическое создание резервной папки
3. Поиск изображений
4. Копирование

**Код скрипта:**
```bash
#!/bin/bash

# Проверка аргументов
if [ $# -eq 0 ]; then
    source_dir="."
elif [ $# -eq 1 ]; then
    source_dir="$1"
else
    echo "Использование: $0 [source_dir]"
    exit 1
fi

# Проверка существования исходной директории
if [ ! -d "$source_dir" ]; then
    echo "Директория '$source_dir' не существует"
    exit 1
fi

# Создание резервной папки с timestamp
backup_dir="backup_images_$(date +'%Y-%m-%d_%H-%M-%S')"

if ! mkdir -p "$backup_dir"; then
    echo "Невозможно создать папку '$backup_dir'!"
    exit 1
fi

count=0

# Включение обработки отсутствующих файлов
shopt -s nullglob

# Копирование изображений
for img in "$source_dir"/*.{jpg,jpeg,png,gif,bmp,webp}; do
    echo "Копируем: $(basename "$img")"
    if cp "$img" "$backup_dir/"; then
        ((count++))
    else
        echo "Ошибка при копировании: $(basename "$img")" >&2
    fi
done

# Отключение nullglob
shopt -u nullglob

echo "Скопировано файлов: $count"

exit 0
```
