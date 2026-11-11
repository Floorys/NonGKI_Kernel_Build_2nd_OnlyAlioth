| Device | Location | Codename | Kernel/Author/Name | Branches | Pack Method | KernelSU | SUSFS | Hook | KPM | Re:Kernel | BBG | NoMount | Droidspaces | Status |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Xiaomi Poco F3 (LineageOS) | All | alioth | [4.19/LineageOS/sm8250](https://github.com/LineageOS/android_kernel_xiaomi_sm8250) | `lineage-24.0`, `lineage-23.2`, `lineage-22.2`, `lineage-21` | AnyKernel3 | BakaSU / Next / xxKSU / rsuntk / SukiSU | ✅ (opt., BakaSU) | Inline / Syscall | ❌ | ✅ (opt.) | ✅ (opt.) | ✅ (opt.) | ✅ (opt.) | Stable |
| Xiaomi Poco F3 (E404) | All | alioth | [4.19/kvsnr113/E404](https://github.com/kvsnr113/xiaomi_sm8250_kernel_e404) | `main`, `main-bpf`, `a17`, `staging`, `personal`, `personal-test` | AnyKernel3 | BakaSU / Next / xxKSU / rsuntk / SukiSU | ✅ (opt., BakaSU) | Inline / Syscall | ❌ | ✅ (opt.) | ✅ (opt.) | ✅ (opt.) | ✅ (opt.) | Untested |

**English**:
- This fork exclusively supports **Xiaomi Poco F3 (`alioth`)**. All other upstream device workflows were removed.
- **LineageOS (`sm8250`)**: supports branches `lineage-24.0`, `lineage-23.2`, `lineage-22.2`, and `lineage-21`, compiled with AOSP Clang (`lineage-20.0`).
- **E404 (`sm8250`)**: supports branches `main`, `main-bpf`, `a17`, `staging`, `personal`, and `personal-test`, compiled with LLVM/Clang `22.1.3` (`LD=ld.lld LLVM=1 LLVM_IAS=1`).
- Every workflow is available in both **EN** and **RU** variants and exposes interactive `workflow_dispatch` toggles:
  - KernelSU fork (`bakasu`, `next`, `xxksu`, `rsuntk`, `sukisu`) and BakaSU `Multi-Manager`
  - `SUSFS` (inline hooks; requires `kernelsu_fork=bakasu`), `NoMount`, `Baseband Guard (BBG)` (+ optional boot partition write block), `Re:Kernel`, `Droidspaces` (LXC/Docker), and `Disable LTO/CFI`.
- Combined `Build and Release Kernel` (`build-release.yml` / `build-release-ru.yml`) can build `lineageos`, `e404`, or `both` in one run and publish directly to GitHub Releases.

**Русский**:
- Этот форк поддерживает только **Xiaomi Poco F3 (`alioth`)** — все остальные устройства из оригинального репозитория удалены.
- **LineageOS (`sm8250`)**: ветки `lineage-24.0`, `lineage-23.2`, `lineage-22.2`, `lineage-21`, сборка через AOSP Clang (`lineage-20.0`).
- **E404 (`sm8250`)**: ветки `main`, `main-bpf`, `a17`, `staging`, `personal`, `personal-test`, сборка через LLVM/Clang `22.1.3` (`LD=ld.lld LLVM=1 LLVM_IAS=1`).
- Все workflow доступны в двух версиях — **EN** и **RU**, с интерактивными галочками при запуске:
  - Выбор KernelSU (`bakasu`, `next`, `xxksu`, `rsuntk`, `sukisu`) и `Multi-Manager` (для BakaSU)
  - `SUSFS` (работает только с `bakasu`), `NoMount`, `Baseband Guard (BBG)` (+ опциональная блокировка записи в `boot`), `Re:Kernel`, `Droidspaces` (LXC/Docker) и отключение `LTO/CFI`.
- Общий релизный workflow (`build-release.yml` / `build-release-ru.yml`) позволяет собрать `lineageos`, `e404` или оба ядра (`both`) и сразу выложить в GitHub Releases.
