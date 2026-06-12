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
- Docker 部署支持

## 安装方式

推荐普通用户使用 Docker Compose 部署。需要二次开发或修改源码时，可以使用源码安装。

## 方式一: Docker Compose 部署

本仓库已经提供 Docker 部署需要的模板文件:

- `.env.example`: 环境变量模板，复制后改成 `.env`
- `docker/docker-compose.yml`: Docker Compose 部署文件
- `strategies/`: 策略目录
- `storage/`: 数据、日志和缓存目录

部署步骤:

```bash
git clone https://github.com/Smallsheeps/jesse-cn.git
cd jesse-cn
cp .env.example .env
```

编辑 `.env`，至少修改 `PASSWORD`。然后启动:

```bash
cd docker
docker compose --env-file ../.env pull
docker compose --env-file ../.env up -d
docker compose --env-file ../.env logs -f jesse
```

默认使用最新镜像:

```bash
aplu001/jesse-cn:latest
```

访问:

```bash
http://localhost:9000
```

如果部署在服务器上，把 `localhost` 替换成服务器 IP 或域名。

## 常用命令

查看容器状态:

```bash
cd docker
docker compose --env-file ../.env ps
```

查看日志:

```bash
cd docker
docker compose --env-file ../.env logs -f jesse
```

停止服务:

```bash
cd docker
docker compose --env-file ../.env stop
```

停止并删除容器:

```bash
cd docker
docker compose --env-file ../.env down
```

注意: 不要随意执行 `docker compose --env-file ../.env down -v`，它会删除数据库卷，历史数据也会被删除。

## 方式二: 从源码安装

源码安装适合本地开发、修改源码或调试。注意，源码安装不会自动安装 PostgreSQL 和 Redis，需要你自己先准备好数据库和 Redis 服务。

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

Python 包名和命令仍然是 `jesse`，不是 `jesse-cn`。

源码方式运行时，也需要在策略项目目录中准备 `.env`、`strategies/` 和 `storage/`。可以从本仓库的 `.env.example` 复制:

```bash
cp .env.example .env
```

本机直接运行时，把 `.env` 里的 `POSTGRES_HOST` 和 `REDIS_HOST` 改为本机地址，例如 `127.0.0.1`。

进入策略项目目录后启动:

```bash
jesse run
```

浏览器访问:

```bash
http://localhost:9000
```

## 方式三: 直接从 GitHub 安装

如果不需要修改源码，也可以直接从 GitHub 安装当前仓库:

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install "git+https://github.com/Smallsheeps/jesse-cn.git"
```

Windows PowerShell 激活虚拟环境命令为:

```powershell
.\.venv\Scripts\activate
```

这种方式同样需要你自己准备 PostgreSQL、Redis、`.env`、`strategies/` 和 `storage/`。

## 免责声明

本软件仅用于学习、研究和技术交流，不构成任何投资建议。量化交易和加密货币交易存在高风险，使用本软件产生的任何交易结果由使用者自行承担。

## License

本项目沿用原项目的 MIT License。详见 [LICENSE](LICENSE)。
