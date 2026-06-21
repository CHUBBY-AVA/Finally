# FinAlly — AI Trading Workstation

An AI-powered trading workstation that streams live market data, simulates portfolio
trading, and integrates an LLM chat assistant. Built entirely by coding agents as a
capstone project for an agentic AI coding course.

> **Status:** Early development. The market-data backend (simulator, Massive/Polygon
> client, price cache, SSE stream) is implemented and tested. Frontend, trading, and AI
> chat are planned — see [`planning/PLAN.md`](planning/PLAN.md).

## Quick Start

```bash
cd backend
uv sync --dev
uv run market_data_demo.py   # live terminal market dashboard
uv run pytest                # test suite
```

The simulator runs with no config. Set `MASSIVE_API_KEY` to use real market data.

## Structure

```
backend/    FastAPI uv project — market-data subsystem (app/market/), tests, demo
frontend/   Next.js static export (planned)
planning/   Project spec and agent contracts
```

## License

See [LICENSE](LICENSE).
