E-commerce_agent
基于 LangGraph + 通义千问 + MySQL 实现的电商多智能体客服系统，支持意图识别、订单查询、商品查询、数据统计、数据库交互与流式接口服务，具备对话记忆能力，可实时响应前端请求。
项目架构
主智能体（main.py）：统一意图识别，智能路由到对应子智能体
订单查询智能体（agent/db_agent.py）：根据订单号 / 手机号 / 用户名查询订单详情
数据统计智能体（agent/product_inquiry_agent.py）：统计已完成订单销量、销售额
商品查询智能体（agent/product_search_agent.py）：查询商品名称、价格、分类
数据库工具（tools/db_tools.py/mysql_connect/mysql_db.py）：MySQL 查询与写入封装
接口服务（app.py）：FastAPI 流式接口，支持前端 SSE 调用
功能清单
✅ 用户意图识别：自动识别商品查询 / 订单查询 / 客服咨询 / 数据统计 / 其它
✅ 订单查询：支持按订单号、手机号、用户名查询订单状态、金额、物流、时间等
✅ 商品查询：查询商品名称、价格、分类信息
✅ 订单数据统计：按日统计已完成订单的销量、销售额
✅ 数据库交互：支持 SELECT 查询 / INSERT 写入
✅ 流式接口：FastAPI + SSE 实时返回回答内容
✅ 对话记忆：基于 InMemorySaver 保留会话上下文
技术栈
Python 3.10+
LangGraph 1.1.10
LangChain 1.2.17
通义千问 qwen-plus
MySQL
FastAPI 0.136.1
python-dotenv
项目结构
plaintext
E-commerce_agent/
├── agent/
│   ├── db_agent.py              # 订单查询智能体
│   ├── product_inquiry_agent.py # 数据统计智能体
│   └── product_search_agent.py  # 商品查询智能体
├── tools/
│   └── db_tools.py              # 数据库工具封装
├── mysql_connect/
│   └── mysql_db.py              # MySQL 连接类
├── app.py                       # FastAPI 流式接口
├── main.py                      # 主智能体（意图识别+路由）
├── test_db.py                   # 数据库测试脚本
├── .env                         # 环境变量配置
├── requirements.txt             # 依赖清单
├── index.html                   # 前端交互页面
└── README.md                    # 项目说明
快速开始
1. 环境准备
确保已安装 Python 3.10+ 和 MySQL，并创建好项目所需数据库。
2. 克隆 / 下载项目
将项目文件下载到本地，进入项目根目录。
3. 安装依赖
打开终端，执行以下命令安装所有依赖包：
bash
运行
pip install -r requirements.txt
4. 配置环境变量
在项目根目录创建 .env 文件，填写以下配置：
env
# 通义千问 API Key
DASHSCOPE_API_KEY=你的通义千问API Key
# LangSmith API Key（可选，用于调试）
LANGCHAIN_API_KEY=你的LangSmith API Key
5. 配置数据库
提前在 MySQL 中创建数据库，并导入项目提供的 orders、user、product 数据表
修改 mysql_connect/mysql_db.py 中的数据库连接信息：
python
运行
db = MySQLDatabase(
    host="localhost",
    port=3306,
    user="root",
    password="你的数据库密码",
    database="你的数据库名"
)
6. 运行项目
启动 FastAPI 服务：
bash
运行
python -m uvicorn app:app --reload
7. 前端使用
服务启动成功后，直接双击打开项目中的 index.html 文件，即可与电商智能客服交互。
使用示例
表格
功能类型	提问示例
订单查询	帮我查一下订单 123456
商品查询	推荐一款手机
数据统计	今天的销售额是多少
普通咨询	什么时候发货
注意事项
确保通义千问 API Key 有效且有调用额度
确保 MySQL 服务正常运行，数据库连接信息配置正确
首次运行需提前创建好 orders、user、product 三张核心数据表
前端页面需在后端服务启动后再打开
License
MIT License
