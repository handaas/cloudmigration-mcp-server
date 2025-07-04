# 招投标大数据服务

[该MCP服务提供上云资产相关服务信息，包括云服务使用情况、技术栈分析、云迁移方案等功能，帮助用户了解企业的上云使用情况。](https://www.handaas.com/)


## 主要功能

- 🔍 企业关键词模糊搜索
- ☁️ 上云资产分析
- 🌐 域名信息查询
- 📊 云服务使用评估
- 📈 上云资产分布


## 环境要求

- Python 3.10+
- 依赖包：python-dotenv, requests, mcp

## 本地快速启动

### 1. 克隆项目
```bash
git clone https://github.com/handaas/cloudmigration-mcp-server
cd cloudmigration-mcp-server
```

### 2. 创建虚拟环境&安装依赖

```bash
python -m venv mcp_env && source mcp_env/bin/activate
pip install -r requirements.txt
```

### 3. 环境配置

复制环境变量模板并配置：

```bash
cp .env.example .env
```

编辑 `.env` 文件，配置以下环境变量：

```env
INTEGRATOR_ID=your_integrator_id
SECRET_ID=your_secret_id
SECRET_KEY=your_secret_key
```

### 4. streamable-http启动服务

```bash
python server/mcp_server.py streamable-http
```

服务将在 `http://localhost:8000` 启动。

#### 支持启动方式 stdio 或 sse 或 streamable-http

### 5. Cursor / Cherry Studio MCP配置

```json
{
  "mcpServers": {
    "handaas-mcp-server": {
      "type": "streamableHttp",
      "url": "http://127.0.0.1:8000/mcp"
    }
  }
}
```

## STDIO版安装部署

### 设置Cursor / Cherry Studio MCP配置

```json
{
  "mcpServers": {
    "cloudmigration-mcp-server": {
      "command": "uv",
      "args": ["run", "mcp", "run", "{workdir}/server/mcp_server.py"],
      "env": {
        "PATH": "{workdir}/mcp_env/bin:$PATH",
        "PYTHONPATH": "{workdir}/mcp_env",
        "INTEGRATOR_ID": "your_integrator_id",
        "SECRET_ID": "your_secret_id",
        "SECRET_KEY": "your_secret_key"
      }
    }
  }
}
```

## 使用官方Remote服务

### 1. 直接设置Cursor / Cherry Studio MCP配置

```json
{
  "mcpServers": {
    "cloudmigration-mcp-server":{
      "type": "streamableHttp",
      "url": "https://mcp.handaas.com/cloudmigration/cloudmigration?token={token}"  
      }
  }
}
```

### 注意：integrator_id、secret_id、secret_key及token需要登录 https://www.handaas.com/ 进行注册开通平台获取


## 可用工具

### 1. cloudmigration_fuzzy_search
**功能**: 企业关键词模糊查询

根据提供的企业名称、人名、品牌、产品、岗位等关键词模糊查询相关企业列表。

**参数**:
- `matchKeyword` (必需): 匹配关键词 - 查询各类信息包含匹配关键词的企业
- `pageIndex` (可选): 分页开始位置
- `pageSize` (可选): 分页结束位置 - 一页最多获取50条数据

**返回值**:
- `total`: 总数
- `resultList`: 结果列表，包含企业基本信息

### 2. cloudmigration_cloud_assets
**功能**: 云上资产分析

查询企业的云上资产信息，输出企业在云上的各种资产状况及特征信息，如有效域名、云服务厂商及云用量等。

**参数**:
- `matchKeyword` (必需): 匹配关键词 - 企业名称/注册号/统一社会信用代码/企业id
- `keywordType` (可选): 主体类型枚举 - name/nameId/regNumber/socialCreditCode

**返回值**:
- `effectiveSubDomainNum`: 有效域名数量
- `subDomainNum`: 子域名数量
- `effectiveSubDomainList`: 有效域名列表
- `subDomainList`: 子域名列表
- `cloudServerList`: 云服务厂商列表
- `cloudConsumptionScale`: 上云资产等级
- `cloudServerNumInterval`: 云用量范围
- `cloudServiceProviderRatio`: 云服务厂商比例
  - `cloudService`: 云服务商名称
  - `ratio`: 云服务厂商比例
- `hasOverseasCloudService`: 有无海外服务器（0:无 1：有）
- `hasCdn`: 有无使用CDN（0:无 1：有）
- `cdnServerNum`: CDN使用规模
- `cdnServerList`: CDN服务商列表
- `hasIDC`: 有无使用IDC（0:无 1：有）
- `hasCloudStorage`: 有无使用云存储（0:无 1：有）

### 3. cloudmigration_domain_info
**功能**: 域名信息查询

根据企业标识信息查询与该企业相关的所有已注册的域名信息，包括域名名称、对应网址、审核时间等详情。

**参数**:
- `matchKeyword` (必需): 匹配关键词 - 企业名称/注册号/统一社会信用代码/企业id
- `keywordType` (可选): 主体类型枚举 - name/nameId/regNumber/socialCreditCode
- `pageIndex` (可选): 页码
- `pageSize` (可选): 分页大小 - 一页最多获取50条数据

**返回值**:
- `total`: 总数
- `resultList`: 列表结果
  - `domainName`: 域名名称
  - `domainUrl`: 网址
  - `websiteRecord`: 网站备案号
  - `filingAuditTime`: 审核时间
  - `isHomePage`: 是否官网

## 使用场景

1. **云迁移规划**: 企业制定云迁移策略和技术选型，评估现有云资产
2. **技术选型**: 了解行业内企业的云技术使用情况，制定技术路线
3. **竞争分析**: 分析竞争对手的云计算应用水平和技术架构
4. **投资决策**: 评估企业的数字化转型程度和云化水平
5. **成本优化**: 参考同行企业的云计算成本控制经验和资源配置
6. **品牌保护**: 监控企业域名资产，防范品牌侵权和域名抢注
7. **合规审查**: 进行域名备案审查和网站合规性检查

## 使用注意事项

1. **企业全称要求**: 在调用需要企业全称的接口时，如果没有企业全称则先调取cloudmigration_fuzzy_search接口获取企业全称
2. **技术保密**: 部分核心技术信息可能不会完全公开，存在信息限制
3. **成本数据**: 成本数据可能为估算值，仅供参考，建议结合实际情况分析
4. **技术更新**: 云技术发展迅速，需关注数据的时效性和技术演进
5. **域名验证**: 域名信息需要验证其真实性和有效性，避免误判
6. **资产评估**: 云资产等级评估基于多项指标，需综合分析

## 使用提问示例

### cloudmigration_cloud_assets (云上资产分析)
3. 阿里巴巴的云上资产规模如何？使用了哪些云服务厂商？
4. 腾讯的有效域名数量有多少？云服务厂商比例分布如何？
5. 抖音有使用海外云服务吗？CDN使用情况怎样？

### cloudmigration_domain_info (域名信息查询)
7. 腾讯注册了多少个域名？都有哪些主要的业务网站？
8. 阿里巴巴的域名备案情况如何？审核时间都是什么时候？
9. 抖音的官网域名有哪些？网站备案号是什么？
