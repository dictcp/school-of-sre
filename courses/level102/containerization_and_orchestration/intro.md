# 容器與協調

## 介紹

容器、Docker 和 Kubernetes 是每個與軟體相關領域的人都在談論的「酷炫」字眼。讓我們深入了解這些技術的核心，理解整體概念到底是什麼！

在本課程中，我們將探討容器的內部運作與應用：容器的實作方式，如何將應用程式容器化，以及如何在大規模部署容器化應用程式時不失眠。我們還會動手嘗試一些實作練習。

### 先備知識
- 基本 Linux 知識，有助於理解容器內部結構
- 基本的 shell 指令知識（對於容器化應用程式很有用）
- 執行基本網頁應用程式的知識。你可以透過我們的[Python 與 Web 模組](https://linkedin.github.io/school-of-sre/level101/python_web/intro/)來熟悉這部分內容。

## 本課程期望

本模組分為三個子模組。第一個子模組介紹容器的內部構造及其使用原因。

第二個子模組介紹 Docker，一個流行的容器引擎，並包含將基本網頁應用容器化的實作練習。

最後一個模組討論容器協調 Kubernetes，並透過實驗展示它如何讓 SRE 的工作更輕鬆。

## 本課程不涵蓋的內容

我們不會涵蓋進階的 Docker 與 Kubernetes 概念，但會提供相關連結與參考資料，方便你根據興趣自行進修。

## 課程內容

本課程涵蓋下列主題：

- [容器介紹](https://linkedin.github.io/school-of-sre/level102/containerization_and_orchestration/intro_to_containers/)
    - [什麼是容器](https://linkedin.github.io/school-of-sre/level102/containerization_and_orchestration/intro_to_containers/#what-are-containers)
    - [為什麼要使用容器](https://linkedin.github.io/school-of-sre/level102/containerization_and_orchestration/intro_to_containers/#why-containers)
    - [虛擬機器與容器的差異](https://linkedin.github.io/school-of-sre/level102/containerization_and_orchestration/intro_to_containers/#difference-between-virtual-machines-and-containers)
    - [容器如何實作](https://linkedin.github.io/school-of-sre/level102/containerization_and_orchestration/intro_to_containers/#how-are-containers-implemented)
    - [Namespace](https://linkedin.github.io/school-of-sre/level102/containerization_and_orchestration/intro_to_containers/#namespaces)
    - [Cgroups](https://linkedin.github.io/school-of-sre/level102/containerization_and_orchestration/intro_to_containers/#cgroups)
    - [容器引擎](https://linkedin.github.io/school-of-sre/level102/containerization_and_orchestration/intro_to_containers/#container-engine)
- [使用 Docker 容器化](https://linkedin.github.io/school-of-sre/level102/containerization_and_orchestration/containerization_with_docker/)
    - [介紹](https://linkedin.github.io/school-of-sre/level102/containerization_and_orchestration/containerization_with_docker/#introduction)
    - [Docker 基本術語](https://linkedin.github.io/school-of-sre/level102/containerization_and_orchestration/containerization_with_docker/#docker-terminology)
    - [Docker 引擎組件](https://linkedin.github.io/school-of-sre/level102/containerization_and_orchestration/containerization_with_docker/#components-of-docker-engine)
    - [動手實作](https://linkedin.github.io/school-of-sre/level102/containerization_and_orchestration/containerization_with_docker/#lab)
    - [進階 Docker 介紹](https://linkedin.github.io/school-of-sre/level102/containerization_and_orchestration/containerization_with_docker/#advanced-features-of-docker)
- [使用 Kubernetes 進行容器協調](https://linkedin.github.io/school-of-sre/level102/containerization_and_orchestration/orchestration_with_kubernetes/)
    - [介紹](https://linkedin.github.io/school-of-sre/level102/containerization_and_orchestration/orchestration_with_kubernetes/#introduction)
    - [使用 Kubernetes 的動機](https://linkedin.github.io/school-of-sre/level102/containerization_and_orchestration/orchestration_with_kubernetes/#motivation-to-use-kubernetes)
    - [Kubernetes 架構](https://linkedin.github.io/school-of-sre/level102/containerization_and_orchestration/orchestration_with_kubernetes/#architecture-of-kubernetes)
    - [動手實作](https://linkedin.github.io/school-of-sre/level102/containerization_and_orchestration/orchestration_with_kubernetes/#lab)
    - [進階 Kubernetes 概念介紹](https://linkedin.github.io/school-of-sre/level102/containerization_and_orchestration/orchestration_with_kubernetes/#advanced-topics)
- [結論](https://linkedin.github.io/school-of-sre/level102/containerization_and_orchestration/conclusion/)
