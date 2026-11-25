# 3dmaster-core

> **3dmaster 共享 3D / CAD 渲染与解析核心库（基于 Qt6 + OpenCASCADE 8.0.0）**  
> **Shared 3D / CAD rendering & parsing core library for the 3dmaster project (Qt6 + OpenCASCADE 8.0.0)**

[🇨🇳 中文](#-中文说明) | [🇺🇸 English](#-english)

---

<a id="-中文说明"></a>

## 🇨🇳 中文说明

### 仓库定位
`3dmaster-core` 是以下两个桌面预览插件的**唯一核心代码真相源（Single Source of Truth）**：
- 🔍 **[3dmaster-seer](https://github.com/luxp1990/3dmaster-seer)** — 面向 Windows Seer 的 3D / CAD 预览插件
- 🚀 **[3dmaster-quicklook](https://github.com/luxp1990/3dmaster-quicklook)** — 面向 Windows QuickLook 的 3D / CAD 预览插件

### 目录结构
```text
3dmaster-core/
├── CMakeLists.txt                  # 定义静态库目标 3dmaster_core
├── README.md
├── include/
│   ├── i18n.h                      # 中英双语运行时切换（支持可选宿主 QSettings 读取）
│   ├── model_data.h                # 网格、原型实例与包围盒数据结构
│   ├── model_loader_worker.h       # 异步模型解析器（STEP/IGES/glTF/3MF/STL/OBJ/PLY/OFF/PRT）
│   ├── master_widget.h             # Qt6 OpenGL 视口控件
│   └── master_sidebar.h            # 装配体结构树与剖切/显示控制侧边栏
└── src/
    ├── model_loader_worker.cpp
    ├── master_widget.cpp
    └── master_sidebar.cpp
```

### 宿主项目集成方式
两个插件仓库通过 Git Submodule 将本仓库固定在各自仓库根目录的 `core/` 路径下，并检出到对应的发布 Tag（Release Tag）：

```bash
git submodule add https://github.com/luxp1990/3dmaster-core.git core
git -C core checkout vX.Y.Z
```

在宿主 `CMakeLists.txt` 中通过 `add_subdirectory(.../core ...)` 引入并链接 `3dmaster_core`（支持传入 `OCCT_DIR` / `OCCT_INCLUDE_DIR` 或开启 `3DMASTER_CORE_DISABLE_OCCT`）。

### 版本号规则
- **锁定同步版本（Lockstep Versioning）**：`3dmaster-core` 的发布 Tag 版本号与 `3dmaster-seer`、`3dmaster-quicklook` 的插件版本号严格保持一致（例如核心发布 `v2.1.0` 对应两个插件的 `v2.1.0`）。

---

<a id="-english"></a>

## 🇺🇸 English

### Purpose
`3dmaster-core` is the **Single Source of Truth** for the shared C++ / Qt6 / OpenCASCADE 8.0.0 rendering and model-loading core consumed by:
- 🔍 **[3dmaster-seer](https://github.com/luxp1990/3dmaster-seer)** — 3D / CAD preview plugin for Windows Seer
- 🚀 **[3dmaster-quicklook](https://github.com/luxp1990/3dmaster-quicklook)** — 3D / CAD preview plugin for Windows QuickLook

### Repository Layout
```text
3dmaster-core/
├── CMakeLists.txt                  # Defines static library target 3dmaster_core
├── README.md
├── include/
│   ├── i18n.h                      # Bilingual (zh/en) runtime i18n with optional host QSettings
│   ├── model_data.h                # Mesh, prototype-instance, and bounds data structures
│   ├── model_loader_worker.h       # Async model parsers (STEP/IGES/glTF/3MF/STL/OBJ/PLY/OFF/PRT)
│   ├── master_widget.h             # Qt6 OpenGL viewport widget
│   └── master_sidebar.h            # Assembly tree & section/display control sidebar
└── src/
    ├── model_loader_worker.cpp
    ├── master_widget.cpp
    └── master_sidebar.cpp
```

### Consuming as a Submodule
Each plugin repository pins `3dmaster-core` as a Git submodule at repo-root `core/` checked out to an exact release tag:

```bash
git submodule add https://github.com/luxp1990/3dmaster-core.git core
git -C core checkout vX.Y.Z
```

The shell `CMakeLists.txt` adds `core/` via `add_subdirectory(...)` and links `3dmaster_core` (reusing the shell's `Qt6` and `OCCT_DIR` / `OCCT_INCLUDE_DIR` configuration, or `3DMASTER_CORE_DISABLE_OCCT=ON` for mesh-only test builds).

### Versioning Rule
- **Lockstep Versioning**: `3dmaster-core` version tags always match the lockstep release version of `3dmaster-seer` and `3dmaster-quicklook` (e.g., `v2.1.0` in `3dmaster-core` corresponds to `v2.1.0` in both plugins).
