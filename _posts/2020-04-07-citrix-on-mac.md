---
title: "Раскладка клавиатуры в Citrix на macOS"
description: Вдруг кому-то пригодится, актуальная инструкция на 2020 год, как побороть раскладку клавиатуры в Citrix на macOS.
category: blog
tag: "Citrix"
lang: ru
---
Вдруг кому-то пригодится, актуальная инструкция на 2020 год, как побороть раскладку клавиатуры в `Citrix` на `macOS`.

И так, пошаговая инструкция:
1. Закрываете `Citrix`
2. Открываете папку `~/Library/Application Support/Citrix Receiver`, можно в finder через `go to...`
3. Там есть файл `Config`, открываете его
4. Находите строку `KeyboardLayout=(User Profile)` и меняете на `KeyboardLayout=US-International`
5. Запускаете `Citrix` и раскладка наконец-то работает как надо
