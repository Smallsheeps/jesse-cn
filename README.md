# Jesse CN

Jesse CN 是基于 [jesse-ai/jesse](https://github.com/jesse-ai/jesse) 的中文改版。

本仓库保留 Jesse 原有的核心框架能力，包括策略回测、参数优化、行情数据导入、Web Dashboard API、技术指标库、Monte Carlo 分析、机器学习辅助流程等。在此基础上，本项目对内置前端静态页面做了中文本地化，方便中文用户直接使用。

## 与原项目的关系

- 原项目仓库: [https://github.com/jesse-ai/jesse](https://github.com/jesse-ai/jesse)
- 原项目官网: [https://jesse.trade](https://jesse.trade)
- 原项目文档: [https://docs.jesse.trade](https://docs.jesse.trade)
- 本项目是中文改版，不是 Jesse 官方仓库。
- 原项目版权和许可证归原作者及贡献者所有，本项目继续遵循原项目的 MIT License。

## 功能概览

- Python 策略开发框架
- 加密货币策略回测
- 参数优化和批量测试
- 多交易所历史 K 线导入
- 内置技术指标库
- Web Dashboard 后端和静态前端
- PostgreSQL 数据存储
- Redis 实时消息和任务状态
- Docker 镜像构建支持

## 环境要求

源码安装推荐环境:

- Python 3.10 或更高版本，推荐 Python 3.11
- Git
- C/C++ 编译环境
- PostgreSQL
- Redis

Docker 使用推荐环境:

- Docker
- Docker Compose

## 从源码安装

Windows PowerShell:

```powershell
git clone https://github.com/Smallsheeps/jesse-cn.git
cd jesse-cn

py -3.11 -m venv .venv
.\.venv\Scripts\activate

python -m pip install --upgrade pip
pip install -e .

jesse --help
```

Linux:

```bash
git clone https://github.com/Smallsheeps/jesse-cn.git
cd jesse-cn

python3.11 -m venv .venv
source .venv/bin/activate

python -m pip install --upgrade pip
pip install -e .

jesse --help
```

注意: Python 包名仍然是 `jesse`，所以安装后命令仍然是 `jesse`，不是 `jesse-cn`。

## 使用 Docker 镜像

如果已经发布到 Docker Hub，可以直接拉取:

```bash
docker pull aplu001/jesse-cn:latest
docker run --rm aplu001/jesse-cn:latest jesse --help
```

指定版本示例:

```bash
docker pull aplu001/jesse-cn:2.3.4-cn.1
docker run --rm aplu001/jesse-cn:2.3.4-cn.1 jesse --help
```

## 本地构建 Docker 镜像

Windows PowerShell:

```powershell
$IMAGE = "aplu001/jesse-cn"
$VERSION = "2.3.4-cn.1"

docker build -t "${IMAGE}:${VERSION}" -t "${IMAGE}:latest" .
```

Linux/macOS:

```bash
IMAGE=aplu001/jesse-cn
VERSION=2.3.4-cn.1

docker build -t "${IMAGE}:${VERSION}" -t "${IMAGE}:latest" .
```

构建完成后验证:

```bash
docker run --rm aplu001/jesse-cn:latest jesse --help
```

## 推送 Docker Hub

先登录 Docker Hub:

```bash
docker login -u aplu001
```

再推送镜像:

```bash
docker push aplu001/jesse-cn:2.3.4-cn.1
docker push aplu001/jesse-cn:latest
```

如果网络环境需要代理，请先在 Docker Desktop 或服务器 Docker daemon 中配置代理，否则拉取基础镜像或推送镜像时可能超时。

## 部署说明

本仓库是 Jesse 框架源码仓库，不是策略项目模板仓库。直接克隆本仓库不会自动生成 `.env`、`docker-compose.yml`、`strategies/`、`storage/` 等部署项目文件。

实际部署时通常需要一个单独的 Jesse 项目目录，里面包含:

```text
.env
docker-compose.yml
strategies/
storage/
```

其中 `docker-compose.yml` 引用本项目发布的镜像，例如:

```yaml
services:
  jesse:
    image: aplu001/jesse-cn:latest
```

Jesse 运行时还需要 PostgreSQL 和 Redis。`.env` 中的数据库、Redis 用户名和密码必须与 `docker-compose.yml` 中配置一致，否则会出现数据库认证失败或 Redis 认证失败。

启动命令通常是:

```bash
docker compose up -d
docker compose logs -f
```

默认 Web 服务端口通常是 `9000`，也可以通过 `.env` 中的 `APP_PORT` 修改。

## 开发说明

安装开发环境后，可以在源码目录运行:

```bash
pytest
```

如果只是修改中文前端静态文件，建议至少检查 JavaScript 语法和页面能否正常打开。

## 免责声明

本软件仅用于学习、研究和技术交流，不构成任何投资建议。量化交易和加密货币交易存在高风险，使用本软件产生的任何交易结果由使用者自行承担。

## License

本项目沿用原项目的 MIT License。详见 [LICENSE](LICENSE)。
