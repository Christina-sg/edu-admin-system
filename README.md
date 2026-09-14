# 教务管理系统（edu-admin-system）

Java + HTML 实现的教务管理系统。**当前处于起步阶段**：环境已就绪、目录骨架已建，
技术选型待确认（见下文）。

---

## 一、开发环境（已安装，2026-09-14）

全部为**用户级安装**（在 `~/.local/opt/` 下），不需要 sudo，也没有改动系统目录。

| 组件 | 版本 | 路径 |
|---|---|---|
| JDK | Temurin **21.0.12.1 LTS** | `~/.local/opt/jdk` → `jdk-21.0.12.1+1` |
| Maven | **3.9.16** | `~/.local/opt/maven` → `apache-maven-3.9.16` |

`~/.bashrc` 末尾已追加以下内容（**已备份**到 `~/.bashrc.bak-20260914-112931`）：

```bash
export JAVA_HOME="$HOME/.local/opt/jdk"
export MAVEN_HOME="$HOME/.local/opt/maven"
case ":$PATH:" in
    *":$JAVA_HOME/bin:"*) ;;
    *) export PATH="$JAVA_HOME/bin:$MAVEN_HOME/bin:$PATH" ;;
esac
```

> `jdk` / `maven` 是**软链接**，指向具体版本目录。以后升级 JDK 只需重指软链接，PATH 不用动。

**验证环境**（新开终端或 `source ~/.bashrc` 后）：

```bash
java -version    # 应显示 openjdk 21.0.12.1 LTS
mvn -version     # 应显示 Apache Maven 3.9.16 + Java 21
```

如果在当前终端里 `java` 还找不到，先执行一次：

```bash
export JAVA_HOME="$HOME/.local/opt/jdk"
export PATH="$JAVA_HOME/bin:$HOME/.local/opt/maven/bin:$PATH"
```

---

## 二、目录结构

```
edu-admin-system/
├── pom.xml                    # Maven 配置（当前框架中立，待确认后加依赖）
├── .gitignore                 # Java/Maven/IDE 忽略规则
├── README.md                  # 本文件
├── docs/                      # 设计文档、需求整理、数据库设计
├── scripts/                   # 辅助脚本（建库、初始化数据、一键启动等）
└── src/
    ├── main/
    │   ├── java/              # Java 源码（包名建议 com.eduadmin.*）
    │   ├── resources/
    │   │   ├── static/        # 静态资源：css / js / img
    │   │   └── templates/     # 服务端模板（Thymeleaf 等，若走模板渲染）
    │   └── webapp/
    │       └── WEB-INF/       # 传统 war 结构（仅 Servlet/JSP 路线需要）
    └── test/java/             # 单元测试
```

---

## 三、技术选型（待确认 ⚠️）

「Java + HTML」可以走三条常见路线，**代码结构差别很大，所以先不写业务代码**。
确定后需要在 `pom.xml` 里加对应依赖：

### 路线 A — Spring Boot 3 + Thymeleaf / 静态 HTML（推荐）
现代主流，生态最全，内置 Tomcat（`java -jar` 或 `mvn spring-boot:run` 直接跑）。
- 教学与求职都常见；后续加数据库、权限、REST 接口都顺
- 目录用 `resources/templates/` 或 `resources/static/`

### 路线 B — SSM（Spring + SpringMVC + MyBatis）
传统教学栈，很多课程/教材仍用这套。配置比 Spring Boot 繁琐（XML 或注解配置）。
- 如果课程指定 SSM，选这条

### 路线 C — Servlet + JSP（最基础）
原生 Java Web，打 war 包部署到 Tomcat。适合课程要求"从底层理解"的场景。
- 目录用 `src/main/webapp/WEB-INF/`

**👉 请确认走哪条路线**（如果不确定，告诉我课程/作业的要求，我按那个来）。

---

## 四、数据库（同样待确认）

教务系统通常需要：学生、教师、课程、班级、选课、成绩等表。
可选：MySQL（最常用）/ PostgreSQL / H2（开发期免安装，内存数据库）。

确定数据库后需要：加驱动依赖 + 配置连接串 + 建表脚本（放 `scripts/` 或 `docs/`）。

---

## 五、常用命令

```bash
# 编译
mvn clean compile

# 打包
mvn clean package

# 查看依赖树（排查依赖冲突）
mvn dependency:tree
```

> 首次执行 `mvn` 会从中央仓库下载插件与依赖（走代理的话见 `~/.bashrc` 的 `detect_proxy`）。
> 如果下载慢或失败，先确认代理已开启：`curl -sS -o /dev/null -w "%{http_code}\n" https://repo.maven.apache.org`

---

## 六、待办（master 回来处理）

- [ ] **确认技术选型**（路线 A / B / C）
- [ ] **确认数据库**（MySQL / PostgreSQL / H2）
- [ ] **配置 GitHub 连接**（`git init`、远程仓库、认证）—— 由 master 亲自执行
- [ ] 之后：写业务代码、建库、写文档

---

## 七、IDE 说明

VS Code 已安装插件（WSL 侧）：
- **Java**：`vscjava.vscode-java-pack`（含 Language Support、Debugger、Test Runner、Maven）
- **Spring Boot**：`vmware.vscode-spring-boot`（若走路线 A 会很有用）
- **HTML/CSS**：`ecmel.vscode-html-css`、`formulahendry.auto-rename-tag`、`ritwickdey.LiveServer`（HTML 实时预览）

用 VS Code 打开本项目时，按提示 **Install on WSL** 或直接用 Remote-WSL 连接（本机是 WSL Ubuntu 24.04）。
