
---
# Yocto 專案概述

- Yocto 專案是一個開源協作框架  
- 從零開始構建針對專用硬體的嵌入式 Linux 系統  

<!--
演示重點：
介紹 Yocto 的定位與價值，強調它並非簡化版桌面 Linux，而是可為特定硬體量身定制的完整解決方案。
-->

---
# 核心元件

1. **OpenEmbedded (OE)**：提供配方 (recipes) 與類 (classes)  
2. **BitBake**：任務調度與構建引擎  
3. **Poky**：官方參考發行版範例  

<!--
演示重點：
說明 OE 作為元資料框架，BitBake 的調度角色，以及 Poky 如何作為可運行的範例系統。
-->
---
# 分層模型 (Layer Model)

- **meta-poky**：Poky 核心元資料  
- **meta-openembedded**：社群維護配方集合  
- **BSP Layers**（如 meta-qcom）：硬體支援套件  
- **Custom Layers**：專案專用修改  

<!--
演示重點：
強調分層的好處：模組化、易於維護與升級；建議自有修改集中在獨立層中。
-->
---
# 配方 (.bb 檔) 結構

- **SRC_URI**：來源位置  
- **依賴關係**  
- **do_compile / do_install**  

```bitbake
DESCRIPTION = "My App"
LICENSE     = "MIT"
SRC_URI     = "file://my-app.tar.gz"

do_compile() {
    oe_runmake
}

do_install() {
    install -d ${D}${bindir}
    install -m 0755 ${S}/my_app ${D}${bindir}/
}
````

<!--
演示重點：
逐步拆解配方內容：來源抓取、編譯步驟、安裝路徑；示範最簡單的 `.bb` 範例。
-->

---

# 配置檔 (.conf)

* **conf/local.conf**：使用者層級設定（目標機器、並行執行數）
* **conf/bblayers.conf**：列出所有啟用的層，並決定順序

<!--
演示重點：
說明 local.conf 如何調整效能與目標，bblayers.conf 如何管理元資料來源與優先順序。
-->

---

# 環境準備

* **主機需求**：建議 16–32 GB RAM、SSD、90–300 GB 可用空間
* **必要套件**（Ubuntu 範例）：

```bash
sudo apt install gawk wget git diffstat unzip texinfo \
     gcc build-essential chrpath socat cpio python3 \
     python3-pexpect xz-utils debianutils iputils-ping \
     python3-jinja2 libegl1-mesa libsdl1.2-dev \
     mesa-common-dev zstd liblz4-tool file locales -y
```


<!--
演示重點：
強調硬體規格對構建效率的影響，以及安裝必要套件的方法。
-->

---

# 核心 BitBake 指令

* `bitbake <image-name>`：構建整套映像 (e.g., core-image-minimal)
* `bitbake <recipe-name>`：構建單一配方 (e.g., gdb)
* `bitbake -c <task> <recipe>`：執行特定任務 (e.g., compile)
* `bitbake -c cleanall <recipe>`：徹底清理

<!--
演示重點：
示範常用命令與參數，並介紹 `bitbake -e` 查詢環境變數的實用技巧。
-->

---

# 自訂與擴充

* **建立新層**：`bitbake-layers create-layer meta-myproject`
* **覆寫配方**：使用 `.bbappend`
* **進階修改**：`devtool modify <recipe>`

<!--
演示重點：
展示如何不改動原配方就加入補丁 (.bbappend)，以及 devtool 的工作流程。
-->

---

# 建置除錯技巧

* **資源不足**：調整 `BB_NUMBER_THREADS`、`PARALLEL_MAKE`
* **抓取錯誤**：`bitbake -c fetch <recipe>`
* **檢查日誌**：`${WORKDIR}/temp/log.do_<task>`

<!--
演示重點：
提供常見錯誤排查方法，教導如何快速定位並解決構建中斷問題。
-->

---

# Qualcomm IQ9 支援

* **meta-qcom BSP**：硬體支援套件
* **repo 工具**：管理多個 Git 倉庫

```bash
repo init -u https://github.com/quic-yocto/qcom-manifest \
         -b qcom-linux-scarthgap -m <manifest.xml>
repo sync
source oe-init-build-env
```

<!--
演示重點：
說明 Qualcomm BSP 的初始化流程，並強調 manifest 檔的重要性與同步步驟。
-->

---

# 實例：Hello IQ9 應用

1. 在 `meta-myproject/recipes-apps/hello-iq9/files/hello_iq9.c` 撰寫程式
2. 建立 `hello-iq9_1.0.bb` 配方
3. 新增至自訂映像配方 (`IMAGE_INSTALL += "hello-iq9"`)
4. `bitbake my-iq9-custom-image`

<!--
演示重點：
透過簡易「Hello World」示例，展現從程式撰寫到映像建置的全流程。
-->

---

# 重點回顧 & Q\&A

* **核心架構**：OE、BitBake、Poky
* **分層與配方**：模組化管理
* **建置與除錯**：高效流程
* **客製化範例**：Hello IQ9

<!--
演示重點：
快速回顧全場重點，並開放聽眾提問。
-->

```

> **使用說明**：  
> - 每個 `---` 分隔一張投影片。  
> - 在 HTML 注釋 (`<!-- ... -->`) 中撰寫講者提示。  
> - 可搭配 Reveal.js、Marp 等工具直接渲染。

