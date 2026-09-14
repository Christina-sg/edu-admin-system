# 教务管理系统（edu-admin-system）

一个 Java Web 教务管理系统，面向学校教务场景，计划覆盖 **学生 / 教师 / 课程 / 班级 / 选课 / 成绩** 等模块。

> **项目状态：起步阶段。** 项目骨架已就绪，技术选型与数据库尚未最终确定（见下方「技术选型」「数据库」两节）。选型确定后开始提交业务代码。

---

## 一、技术栈

**已确定**

- **Java 21 (LTS)** —— 编译目标 `release 21`
- **Maven 3.9+** —— 构建与依赖管理
- **UTF-8** —— 源码与资源统一编码

**待确定**

- **Web 框架**：路线 A（Spring Boot 3 + Thymeleaf）/ 路线 B（SSM）/ 路线 C（Servlet + JSP），三选一
- **数据库**：MySQL / PostgreSQL / H2（开发期可先用 H2 免安装）

---

## 二、目录结构

```
edu-admin-system/
├── pom.xml                    # Maven 配置（当前框架中立，选型确定后加依赖）
├── .gitignore                 # Java / Maven / IDE 忽略规则
├── README.md                  # 本文件
├── docs/                      # 设计文档、需求整理、数据库设计
├── scripts/                   # 辅助脚本（建库、初始化数据、一键启动等）
└── src/
    ├── main/
    │   ├── java/              # Java 源码，包名统一用 com.eduadmin.*
    │   ├── resources/
    │   │   ├── static/        # 静态资源：css / js / img
    │   │   └── templates/     # 服务端模板（Thymeleaf 等，若走模板渲染）
    │   └── webapp/
    │       └── WEB-INF/       # 传统 war 结构（仅 Servlet / JSP 路线需要）
    └── test/java/             # 单元测试
```

> 空的目录用 `.gitkeep` 占位 —— **git 本身不跟踪空目录**，没有占位文件的话提交后这些目录不会出现在仓库里。新增目录时请照做。

---

## 三、本地开发环境

需要具备：

- **JDK 21**（任意发行版：Temurin / Oracle / OpenJDK 均可）
- **Maven 3.9+**
- **Git**

检查是否就绪：

```bash
java -version    # 期望：openjdk version "21.x"
mvn -version     # 期望：Apache Maven 3.9.x + Java 21
```

如果自备的 JDK / Maven 不在 PATH 里，在自己的 shell 配置中导出即可（路径按各自机器实际调整）：

```bash
export JAVA_HOME="/path/to/jdk-21"
export PATH="$JAVA_HOME/bin:/path/to/apache-maven-3.9.x/bin:$PATH"
```

**网络提示**：首次执行 `mvn` 需要从中央仓库下载插件与依赖。国内网络较慢时，可在 `~/.m2/settings.xml` 里配置镜像（如阿里云）或走本地代理 —— 这是各人本地环境，不要提交进仓库。

---

## 四、常用命令

```bash
mvn clean compile     # 编译
mvn clean package     # 打包
mvn dependency:tree   # 查看依赖树（排查版本冲突）
mvn test              # 运行单元测试
```

确定 Web 框架后，启动方式会补充在此（例如路线 A 的 `mvn spring-boot:run`）。

---

## 五、技术选型（待确认）

「Java + HTML」可以走三条常见路线，**代码结构与依赖差别较大**，因此选型确定前不写业务代码：

**路线 A — Spring Boot 3 + Thymeleaf / 静态 HTML（推荐）**
现代主流方案，生态完整，内置 Tomcat，`mvn spring-boot:run` 或 `java -jar` 直接启动。后续接数据库、权限、REST 接口都顺。目录使用 `resources/templates/` 或 `resources/static/`。

**路线 B — SSM（Spring + SpringMVC + MyBatis）**
传统教学栈，许多课程与教材仍在使用；配置比 Spring Boot 繁琐。

**路线 C — Servlet + JSP**
原生 Java Web，打成 war 包部署到 Tomcat；适合要求"从底层理解"的场景。目录使用 `src/main/webapp/WEB-INF/`。

> 选型确定后，在 `pom.xml` 的 `<dependencies>` 中补充对应依赖。

---

## 六、数据库（待确认）

教务场景通常需要这些表：**学生、教师、课程、班级、选课、成绩**（可能还有院系、专业、学期）。

- 候选：**MySQL**（最常用）/ **PostgreSQL**（功能强）/ **H2**（开发期免安装，内存或文件模式）
- 确定后需要：加驱动依赖 + 配置连接串 + **建表脚本**（放 `scripts/`，设计说明放 `docs/`）

> 连接串与口令等敏感信息**不要写进提交的配置文件**：用环境变量，或放在已被 `.gitignore` 忽略的 `application-local.properties` / `.env`。

---

## 七、协作约定

**分支**

- `main` 保持可运行状态，直接改 `main` 前请确认不会破坏构建
- 功能开发建议开分支：`feat/<模块名>`，完成后通过 Pull Request 合并

**提交信息**

- 格式：`<type>: <简述>`，冒号后留一个空格
- `type` 取值：`feat`（新功能）/ `fix`（修缺陷）/ `docs`（文档）/ `chore`（杂务）/ `refactor`（重构）/ `test`（测试）
- 例：`feat: 新增学生列表查询接口`

**提交前自查**

- 提交前先拉取最新代码，避免历史分叉
- **不要提交**：编译产物（`target/`）、IDE 配置、本地数据库文件、日志、以及任何密码 / 密钥 / token
- 真实学生数据（姓名、学号、成绩）一律不入库、不入仓库；测试请用假数据

---

## 八、开发计划

- [x] 建立项目骨架与本地开发环境
- [ ] 确认技术选型（路线 A / B / C）与数据库
- [ ] 数据库设计 + 建表脚本
- [ ] 登录与权限（学生 / 教师 / 管理员角色）
- [ ] 学生、教师、课程、班级管理模块
- [ ] 选课与成绩模块
- [ ] 部署与验收文档

---

## 九、参与开发

仓库为**公开仓库**，可直接查看与克隆。

需要提交代码的协作者，请联系仓库管理员在 **Settings → Collaborators** 中添加写权限；收到邀请后**需要在 GitHub 上接受邀请**，否则无法推送。私有改动也可以先 Fork 后提 Pull Request。
