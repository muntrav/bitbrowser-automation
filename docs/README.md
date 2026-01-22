# Auto BitBrowser 项目文档

欢迎查阅 Auto BitBrowser 自动化管理系统的技术文档。

## 📚 文档目录

### 基础文档
- [架构设计](./architecture.md) - 系统架构与技术栈
- [快速开始](./quickstart.md) - 安装配置与快速上手
- [配置指南](./configuration.md) - 详细配置说明

### 核心模块
- [数据库设计](./database.md) - 数据模型与表结构
- [浏览器管理](./browser-management.md) - BitBrowser API 集成
- [任务系统](./task-system.md) - 任务编排与执行流程
- [自动化脚本](./automation-scripts.md) - Playwright 自动化实现

### API 文档
- [后端 API](./backend-api.md) - FastAPI 接口文档
- [WebSocket](./websocket.md) - 实时通信协议

### 开发指南
- [开发环境](./development.md) - 开发环境搭建
- [代码规范](./coding-standards.md) - 代码风格与最佳实践
- [调试技巧](./debugging.md) - 常见问题与调试方法

## 🎯 项目概述

Auto BitBrowser 是一个基于 **FastAPI + Vue 3 + Playwright** 的自动化管理系统，专为 Google 账号批处理场景设计。

### 核心功能
- 账号与浏览器窗口管理
- 2FA 自动化设置与修改
- 年龄验证与资格检测
- 绑卡订阅自动化
- 实时任务进度与日志

### 技术栈
- **后端**: Python 3.11+, FastAPI, SQLite
- **前端**: Vue 3, Vite, Tailwind CSS
- **自动化**: Playwright, BitBrowser API
- **通信**: WebSocket (实时推送)

## 🚀 快速导航

### 我想...
- **了解系统架构** → [架构设计](./architecture.md)
- **开始使用** → [快速开始](./quickstart.md)
- **配置账号和卡片** → [配置指南](./configuration.md)
- **理解任务执行流程** → [任务系统](./task-system.md)
- **开发新功能** → [开发指南](./development.md)
- **调试问题** → [调试技巧](./debugging.md)

## 📞 获取帮助

- **GitHub Issues**: 报告 Bug 或提出功能建议
- **文档反馈**: 如果文档有不清楚的地方，欢迎提 Issue

## 📄 许可证

本项目采用 MIT 许可证。详见 [LICENSE](../LICENSE) 文件。
