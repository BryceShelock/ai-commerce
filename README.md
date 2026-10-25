# AI Commerce

带导购和选品评分的电商原型。店面用 React 写，交易和库存用 Django 写。

## 店面能完成的事

浏览和下单、购物车、结算、订单跟踪、评价、收藏、退款售后、地址和 SKU。运营侧有库存监控、补货提醒、批量订单操作和数据看板。

## 两个 AI 模块

- **导购**（`ai_guide/`）：按消息判断咨询、推荐或优惠意图，结合用户画像标签从商品库取回结果。对话组件是 `AIGuideChat`。
- **选品**（`ai_selector/`）：按品类增长率和竞争度给 SKU 打潜力分，输出推荐清单。可用 Celery 定时跑。

导购的生成回复目前是服务内的意图规则，外呼大模型的位置留在 `AIGuideService`。选品分数来自代码里的品类趋势表，用来演示评分和排序，不是外部市场数据。

## 运行

后端（仓库根目录）：

```bash
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
python manage.py migrate
python manage.py runserver 8000
```

前端（另开一个终端）：

```bash
cd frontend
npm install --legacy-peer-deps
npm run dev
```

Vite 默认在 `http://localhost:5173/`。后端地址用环境变量 `VITE_API_BASE`，例如 `http://localhost:8000`。

库存定时任务需要 Redis，然后：

```bash
celery -A celery_app worker --loglevel=info
```

## 接口

商品、评价、购物车、订单、地址、分析、库存、行为、推荐都在 `core_ecommerce/urls.py` 的 `/api/` 下。

课程项目里完成的可本地运行原型。
