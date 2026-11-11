<h2 align="center">Non-GKI Kernel Build — Xiaomi Poco F3 (alioth)</h2>

<p align="center">
  <a href="README.md">English</a> | Русский | <a href="README_cn.md">中文说明</a> | <a href="Supported_list.md">Список поддержки</a> | <a href="https://github.com/JackA1ltman/NonGKI_Kernel_Build_2nd/wiki">Wiki (оригинал)</a>
</p>
<p align="center">
  <img alt="GitHub Actions Workflow Status" src="https://img.shields.io/github/actions/workflow/status/Floorys/NonGKI_Kernel_Build_2nd/build-release.yml?branch=mainline&style=for-the-badge">
  <img alt="GitHub Downloads (all assets, latest release)" src="https://img.shields.io/github/downloads/Floorys/NonGKI_Kernel_Build_2nd/latest/total?style=for-the-badge">
  <img alt="GitHub License" src="https://img.shields.io/github/license/Floorys/NonGKI_Kernel_Build_2nd?style=for-the-badge">
</p>

> [!NOTE]
> Форк [JackA1ltman/NonGKI_Kernel_Build_2nd](https://github.com/JackA1ltman/NonGKI_Kernel_Build_2nd) (версия 2.0), полностью переделанный под **Xiaomi Poco F3 (`alioth`, 4.19)**.  
> Лишние устройства удалены — добавлены интерактивные переключатели функций в GitHub Actions и переводы workflow на **английский** и **русский** языки.

---

### О проекте

Репозиторий предназначен для автоматической сборки Non-GKI ядра `4.19` для **Xiaomi Poco F3 (`alioth`)** через **GitHub Actions** с упаковкой в `AnyKernel3` и (опционально) автоматической публикацией в **GitHub Releases**.

> [!IMPORTANT]
> Проект распространяется по лицензии [GPLv3](LICENSE).
>
> Разрешается:
> - Делать форк и использовать для личной сборки ядер
> - Ставить Star и участвовать в развитии проекта
> - Делиться собранными образами в открытом виде и бесплатно
>
> Запрещается:
> - Использовать проект для оказания платных услуг
> - Вести коммерческую деятельность без согласия авторов

---

### Поддерживаемые ядра (Poco F3 / `alioth`)

Подробная таблица параметров — в [Supported_list.md](Supported_list.md).

1. **LineageOS (`sm8250`)** — [`LineageOS/android_kernel_xiaomi_sm8250`](https://github.com/LineageOS/android_kernel_xiaomi_sm8250)
   - Ветки: `lineage-24.0`, `lineage-23.2`, `lineage-22.2`, `lineage-21`
   - Компилятор: AOSP Clang (`lineage-20.0`), `LD=ld.lld LLVM=1 LLVM_IAS=1`
   - Фикс-патч SUSFS: [`Patches/Patch/poco_f3_susfs_fixed.patch`](Patches/Patch/poco_f3_susfs_fixed.patch)
2. **E404 (`sm8250`)** — [`kvsnr113/xiaomi_sm8250_kernel_e404`](https://github.com/kvsnr113/xiaomi_sm8250_kernel_e404)
   - Ветки: `main`, `main-bpf`, `a17`, `staging`, `personal`, `personal-test`
   - Компилятор: LLVM/Clang `22.1.3`, `LD=ld.lld LLVM=1 LLVM_IAS=1`
   - Фикс-патч SUSFS: [`Patches/Patch/poco_f3_e404_susfs_fixed.patch`](Patches/Patch/poco_f3_e404_susfs_fixed.patch)

---

### Доступные Workflow (EN и RU)

В разделе **Actions** доступны отдельные сценарии на английском и русском языках:

| Назначение | EN | RU |
|---|---|---|
| Сборка и релиз (LineageOS / E404 / оба) | [build-release.yml](.github/workflows/build-release.yml) | [build-release-ru.yml](.github/workflows/build-release-ru.yml) |
| Сборка только Poco F3 (LineageOS) | [build-poco-f3-lineageos.yml](.github/workflows/build-poco-f3-lineageos.yml) | [build-poco-f3-lineageos-ru.yml](.github/workflows/build-poco-f3-lineageos-ru.yml) |
| Сборка только Poco F3 (E404) | [build-poco-f3-e404.yml](.github/workflows/build-poco-f3-e404.yml) | [build-poco-f3-e404-ru.yml](.github/workflows/build-poco-f3-e404-ru.yml) |

- Еженедельная автосборка (`schedule`) включена только в [build-release.yml](.github/workflows/build-release.yml), чтобы не запускать дублирующие сборки.

---

### Интерактивные переключатели при запуске

При нажатии **Run workflow** можно настроить сборку галочками и выпадающими списками:

- **Цель / ветка**: выбор `lineageos` / `e404` / `both` (в релизном workflow) и нужной ветки ядра.
- **Менеджер KernelSU (`kernelsu_fork`)**: `bakasu` (по умолчанию), `next`, `xxksu`, `rsuntk`, `sukisu`.
- **Multi-Manager (`multi_manager`)**: поддержка нескольких менеджеров для **BakaSU** (`BakaSU + Official KSU + KowSU`).
- **SUSFS (`susfs_enable`)**: встроенные патчи скрытия рута/модулей (`v2.4.0`).  
  > Важно: SUSFS в этом пайплайне работает **только с `kernelsu_fork=bakasu`** (при несовместимой комбинации запуск остановится на проверке параметров до начала компиляции).
- **NoMount (`nomount_enable`)**: подмена путей без монтирования.
- **Baseband Guard (`bbg_enable` / `bbg_boot`)**: защита разделов (и опциональная блокировка записи в `boot`-раздел).
- **Re:Kernel (`rekernel_enable`)**: поддержка Re:Kernel для управления заморозкой процессов.
- **Droidspaces (`droidspaces_enable`)**: конфиг и фиксы для контейнеров LXC / Docker.
- **Disable LTO / CFI (`disable_lto_cfi`)**: отключение LTO и CFI для более быстрой сборки или отладки.

---

### Как запустить сборку

1. Перейдите во вкладку **Actions** вашего репозитория.
2. Выберите нужный workflow на русском (`... (RU)`) или английском (`... (EN)`).
3. Нажмите **Run workflow**, выберите ветку ядра, форк KernelSU и отметьте нужные модули.
4. После завершения:
   - при использовании `build-poco-f3-*` архив `AnyKernel3` появится в **Artifacts**;
   - при использовании `build-release*` с включённым `release-kernel` архивы будут прикреплены к новому релизу во вкладке **Releases**.

---

### Благодарности

- Оригинальный проект: [JackA1ltman/NonGKI_Kernel_Build_2nd](https://github.com/JackA1ltman/NonGKI_Kernel_Build_2nd)
- Контрибьюторы серии [Version 1.x](https://github.com/JackA1ltman/NonGKI_Kernel_Build): [@adontoo](https://github.com/adontoo), [@PeterTea5822](https://github.com/PeterTea5822), [@pkczc](https://github.com/pkczc), [@yu13140](https://github.com/yu13140)
- [KernelSU_Action](https://github.com/xiaoleGun/KernelSU_Action) - @xiaoleGun

### Авторские права и используемые проекты

- [KernelSU](https://github.com/tiann/KernelSU) - @tiann
  - [rsuntk](https://github.com/rsuntk/KernelSU) - @rsuntk ([rsuntk-SUSFS](https://github.com/cyberc3dr/KernelSU) - @cyberc3dr)
  - [xxKSU](https://github.com/backslashxx/KernelSU) - @backslashxx
  - [SukiSU-Ultra](https://github.com/SukiSU-Ultra/SukiSU-Ultra) - @ShirkNeko
    - [BakaSU](https://github.com/Baka-SU/BakaSU) - @BakaSU Development ([ReSukiSU_CI](https://github.com/cctv18/ReSukiSU_CI) - @cctv18)
  - [KernelSU-Next](https://github.com/KernelSU-Next/KernelSU-Next) - @rifsxd
- [SUSFS](https://gitlab.com/simonpunk/susfs4ksu) - @simonpunk
- [Re:Kernel](https://github.com/Sakion-Team/Re-Kernel) - @Sakion-Team
- [Baseband Guard](https://github.com/vc-teahouse/Baseband-guard) - @秋刀鱼
- [Droidspaces](https://github.com/ravindu644/Droidspaces-OSS) - @ravindu644
- [NoMount](https://github.com/maxsteeel/nomount) - @maxsteeel
- [AnyKernel3](https://github.com/osm0sis/AnyKernel3) - @osm0sis
- Авторы ядер: [LineageOS](https://github.com/LineageOS/android_kernel_xiaomi_sm8250), [@kvsnr113](https://github.com/kvsnr113/xiaomi_sm8250_kernel_e404)

