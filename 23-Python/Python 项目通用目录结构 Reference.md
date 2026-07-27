# Flask 项目结构

*   [Flask 官方文档 - 项目布局](https://flask.palletsprojects.com/en/stable/tutorial/layout/)
*   [Flask 大型应用结构指南](https://flask.palletsprojects.com/en/stable/patterns/packages/)

```
flask_backend/
├── app/
│   ├── __init__.py              # 创建应用实例，初始化扩展
│   ├── models/                  # 数据模型定义（SQLAlchemy）
│   ├── routes/
│   │   ├── __init__.py
│   │   ├── auth.py              # 认证相关路由
│   │   ├── api.py               # 主要API端点
│   │   └── admin.py             # 管理后台路由
│   │ 
│   ├── services/                 # 业务逻辑层
│   │   ├── user_service.py
│   │   └── order_service.py
│   │ 
│   ├── utils/                    # 辅助函数
│   │   ├── validators.py
│   │   └── decorators.py
│   │ 
│   ├── config.py                # 配置类（开发/测试/生产）
│   └── extensions.py            # 第三方扩展初始化（可选）
├── migrations/                   # 数据库迁移脚本（Alembic）
├── tests/                        # 测试文件
│   ├── test_models.py
│   └── test_routes.py
├── instance/                     # 实例文件夹（存放本地配置、数据库）
│   └── config.py                # 不提交到版本控制的配置
├── requirements.txt              # 项目依赖
├── .env                          # 环境变量（不提交）
├── .gitignore
└── run.py                        # 应用启动入口
```

**核心特点**：

*   **应用工厂模式**：在 `app/__init__.py` 中使用工厂函数创建应用实例
*   **蓝图组织**：使用 Flask Blueprint 在 `routes/` 目录下模块化路由
*   **配置分离**：通过类继承实现不同环境配置（开发/测试/生产）


# Django 项目结构

*   [Django 官方文档 - 编写你的第一个 Django 应用](https://docs.djangoproject.com/en/stable/intro/tutorial01/)
*   [Django 项目结构最佳实践](https://docs.djangoproject.com/en/stable/intro/reusable-apps/)
*   [Django 设置模块化模式](https://docs.djangoproject.com/en/stable/topics/settings/#designating-the-settings)

```
django_backend/
├── manage.py                    # Django 命令行工具入口
├── config/                      # 项目设置目录（可重命名）
│   ├── __init__.py
│   ├── settings/
│   │   ├── __init__.py
│   │   ├── base.py             # 基础通用设置
│   │   ├── development.py      # 开发环境设置
│   │   ├── production.py       # 生产环境设置
│   │   └── testing.py          # 测试环境设置
│   ├── urls.py                  # 根URL配置
│   └── wsgi.py                  # WSGI应用入口
├── apps/                        # 自定义应用目录
│   ├── users/
│   │   ├── __init__.py
│   │   ├── admin.py
│   │   ├── apps.py
│   │   ├── models.py
│   │   ├── views.py
│   │   ├── urls.py
│   │   ├── serializers.py      # DRF序列化器（如使用DRF）
│   │   └── migrations/
│   └── products/
│       ├── ...（类似结构）
├── static/                      # 静态文件（CSS, JS, images）
├── media/                       # 用户上传文件
├── templates/                   # 全局模板
├── requirements/
│   ├── base.txt                # 基础依赖
│   ├── development.txt         # 开发环境额外依赖
│   └── production.txt          # 生产环境依赖
├── .env
└── README.md
```

**最佳实践改进**：

*   **设置模块化**：将 `settings.py` 拆分为多个环境特定文件
*   **应用容器化**：所有自定义应用放在 `apps/` 目录下
*   **依赖管理**：使用多个 requirements 文件管理不同环境依赖


# FastAPI 项目结构

*   [FastAPI 官方文档 - 项目结构](https://fastapi.tiangolo.com/tutorial/bigger-applications/)
*   [FastAPI 完整项目示例](https://fastapi.tiangolo.com/advanced/advanced-dependencies/)
*   [FastAPI 最佳实践项目模板](https://github.com/tiangolo/full-stack-fastapi-template)

```
fastapi_backend/
├── app/
│   ├── __init__.py
│   ├── main.py                 # FastAPI应用实例创建
│   ├── config.py               # 配置
│   ├── core/                   # 核心功能
│   │   ├── config.py           # 配置管理（Pydantic Settings）
│   │   ├── security.py         # 认证授权逻辑
│   │   └── dependencies.py     # 依赖注入
│   │ 
│   ├── api/
│   │   ├── __init__.py
│   │   ├── v1/                 # API版本管理
│   │   │   ├── __init__.py
│   │   │   ├── auth.py
│   │   │   └── items.py
│   │   │   └── api.py          # v1路由聚合
│   │ 
│   ├── models/
│   │   ├── __init__.py
│   │   ├── user.py             # Pydantic模型/SQLAlchemy模型
│   │   └── item.py
│   │ 
│   ├── schemas/                # Pydantic模式（请求/响应）
│   ├── crud/                   # 数据库操作层
│   ├── database.py             # 数据库连接配置
│   └── utils/
│   
├── alembic/                    # 数据库迁移（可选）
│   ├── versions/
│   └── alembic.ini
│   
├── tests/
│   ├── conftest.py             # pytest配置
│   ├── test_api/
│   └── test_units/
├── requirements.txt
├── .env
└── docker-compose.yml          # 容器化部署（可选）
```

**架构优势**：

*   **清晰分层**：模型、模式、CRUD操作分离
*   **API版本化**：便于后续迭代和兼容性管理
*   **依赖注入**：充分利用 FastAPI 的依赖注入系统


