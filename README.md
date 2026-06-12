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

## 环境要求

- Docker
- Docker Compose

## 使用 Docker 安装

默认使用最新镜像:

```bash
aplu001/jesse-cn:latest
```

先拉取镜像并验证命令可用:

```bash
docker pull aplu001/jesse-cn:latest
docker run --rm aplu001/jesse-cn:latest jesse --help
```

## Docker Compose 部署

Jesse 运行时需要 PostgreSQL 和 Redis。建议单独创建一个策略项目目录，不要直接在框架源码目录里运行。

```bash
mkdir my-jesse-project
cd my-jesse-project
mkdir strategies storage
```

创建 `.env`:

```env
PASSWORD=change-me
APP_PORT=9000

POSTGRES_HOST=postgres
POSTGRES_PORT=5432
POSTGRES_NAME=jesse_db
POSTGRES_USERNAME=jesse_user
POSTGRES_PASSWORD=password

REDIS_HOST=redis
REDIS_PORT=6379
REDIS_DB=0
REDIS_PASSWORD=
```

创建 `docker-compose.yml`:

```yaml
services:
  jesse:
    image: aplu001/jesse-cn:latest
    container_name: jesse-cn
    working_dir: /home
    command: sh -c "jesse run"
    env_file:
      - .env
    ports:
      - "${APP_PORT:-9000}:${APP_PORT:-9000}"
    volumes:
      - ./:/home
    depends_on:
      postgres:
        condition: service_healthy
      redis:
        condition: service_healthy

  postgres:
    image: postgres:14-alpine
    container_name: jesse-postgres
    environment:
      POSTGRES_DB: ${POSTGRES_NAME}
      POSTGRES_USER: ${POSTGRES_USERNAME}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
    volumes:
      - postgres-data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U $$POSTGRES_USER -d $$POSTGRES_DB"]
      interval: 5s
      timeout: 5s
      retries: 20

  redis:
    image: redis:6-alpine
    container_name: jesse-redis
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 5s
      timeout: 5s
      retries: 20

volumes:
  postgres-data:
```

启动:

```bash
docker compose up -d
docker compose logs -f jesse
```

访问:

```bash
http://localhost:9000
```

如果部署在服务器上，把 `localhost` 替换成服务器 IP 或域名。

## 常用命令

查看容器状态:

```bash
docker compose ps
```

查看日志:

```bash
docker compose logs -f jesse
```

停止服务:

```bash
docker compose stop
```

停止并删除容器:

```bash
docker compose down
```

注意: 不要随意执行 `docker compose down -v`，它会删除数据库卷，历史数据也会被删除。

## 免责声明

本软件仅用于学习、研究和技术交流，不构成任何投资建议。量化交易和加密货币交易存在高风险，使用本软件产生的任何交易结果由使用者自行承担。

## License

本项目沿用原项目的 MIT License。详见 [LICENSE](LICENSE)。
