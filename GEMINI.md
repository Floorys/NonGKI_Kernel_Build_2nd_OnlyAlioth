# Non-GKI Kernel Build — Xiaomi Poco F3 (alioth)

## Общие сведения о проекте
Этот репозиторий — специализированный форк `JackA1ltman/NonGKI_Kernel_Build_2nd` (v2.0), полностью оптимизированный и переработанный исключительно под **Xiaomi Poco F3 (`alioth`)**, ядро **Linux 4.19**.

> [!IMPORTANT]
> **Критическое правило:** Все сторонние устройства из апстрима удалены. НИКОГДА не восстанавливать удалённые устройства и не синхронизировать обратно чужие воркфлоу устройств при обновлениях.

---

## Поддерживаемые ядра и компиляторы

1. **LineageOS (`sm8250`)**:
   - Репозиторий: `LineageOS/android_kernel_xiaomi_sm8250`
   - Ветки: `lineage-24.0` (по умолчанию), `lineage-23.2`, `lineage-22.2`, `lineage-21`
   - Компилятор: AOSP Clang (`lineage-20.0`), `LD=ld.lld LLVM=1 LLVM_IAS=1`
   - Упаковщик: стандартный `AnyKernel3`
   - Фикс SUSFS: `Patches/Patch/poco_f3_susfs_fixed.patch`

2. **E404 (`sm8250`)**:
   - Репозиторий: `kvsnr113/xiaomi_sm8250_kernel_e404`
   - Ветки: `main`, `main-bpf`, `a17`, `staging` + все 31 ветка ядра E404
   - Компилятор: LLVM/Clang `22.1.3`, `LD=ld.lld LLVM=1 LLVM_IAS=1`
   - Упаковщик: интерактивный `kvsnr113/AnyKernel3` (выбор ROM, DTB, батареи кнопками громкости)
   - Фикс SUSFS: `Patches/Patch/poco_f3_e404_susfs_fixed.patch`

---

## Архитектура воркфлоу (CI/CD)
В репозитории строго поддерживаются **ровно 2 сценария**:
- `.github/workflows/build-release.yml` — основной воркфлоу на английском языке (+ еженедельный `schedule`).
- `.github/workflows/build-release-ru.yml` — русскоязычный интерфейс (вызывает `build-release.yml` с `lang: "ru"`).

Релизы публикуются через нативный GitHub CLI (`gh release create "${{ env.NEW_TAG }}" ./downloaded-artifacts/*`), устаревший экшен `actions/create-release@v1` удалён.

---

## Правила совместимости и инварианты

1. **KernelSU и SUSFS:**
   - Форки: `bakasu` (по умолчанию), `next`, `xxksu`, `rsuntk`, `sukisu`.
   - **SUSFS совместим ТОЛЬКО с `bakasu`!** При выборе других форков SUSFS должен быть выключен, иначе сборка валится с явным сообщением об ошибке.
   - `Multi-Manager` (BakaSU + Official KSU + KowSU) активен только для `bakasu`.

2. **Особенности E404 (вырезанный аудит и линковка):**
   - В `alioth_defconfig` у E404 выключен `# CONFIG_AUDIT is not set`.
   - В `.github/workflows/patch-no-kprobe/action.yml` ОБЯЗАТЕЛЬНО должна сохраняться fallback-заглушка:
     ```c
     #ifndef CONFIG_AUDIT
     void security_dump_masked_av(struct policydb *policydb, struct context *scontext, struct context *tcontext, u16 tclass, u32 permissions) {}
     #endif
     ```
     Без неё линкер `ld.lld` падает с ошибкой `undefined symbol: security_dump_masked_av`.

3. **KPM (Kernel Patch Module):**
   - Требует `CONFIG_DEBUG_KERNEL=y`, `CONFIG_KALLSYMS=y` и `CONFIG_KALLSYMS_ALL=y`.
   - При `kpm_enable: true` эти параметры автоматически принудительно прописываются в дефконфиг (включая E404, где автор отключил их ради экономии памяти).
   - Кэш `ccache` разделен по `DEVICE_NAME` и `ROM_TEXT`, чтобы Clang 20 и Clang 22 не засоряли кэш друг друга.

4. **Интегрированные модули:**
   - **NoMount**: ветка `master` (`curl -sSLf https://raw.githubusercontent.com/maxsteeel/nomount/refs/heads/master/kernel/setup.sh | bash -s`).
   - **Baseband Guard (BBG)**: `curl -sSLf https://github.com/vc-teahouse/Baseband-guard/raw/main/setup.sh | bash` (+ опциональная блокировка записи в `boot`).
   - **Re:Kernel**: проверка наличия `cgroup_task_frozen` перед применением, патч без `|| true`.
   - **Droidspaces**: установка `coccinelle` (`spatch`) по требованию через apt, сборка из исходников вырезана для экономии времени.
   - **fgetpriority**: экшен `.github/workflows/patch-fgetpriority`.
   - **VPNHide Next**: экшен `.github/workflows/patch-vpnhide` (`git clone --depth=1` и очистка `rm -rf vpnhide`).
   - **RTL8812AU (88XXau)**: экшен `.github/workflows/patch-rtl8812au` (`aircrack-ng/rtl8812au` ветка `v5.6.4.2`, встраивается `CONFIG_88XXAU=y` для TP-Link Archer T4UH v2 `2357:010E` и др., адаптировано под ARM64 + Clang/LLD + `O=out`).
