# curve-dao-contracts

[Curve](https://www.curve.fi/)治理 DAO 使用 Vyper 合约编写.

## 总览

Curve DAO由[Aragon](https://github.com/aragon/aragonOS)的多个智能合约组成。与Aragon的交互是通过[Aragon Voting App](https://github.com/aragon/aragon-apps/tree/master/apps/voting)的[修改实现](https://github.com/curvefi/curve-aragon-voting)发生的。 Aragon是一种基于被锁定令牌的加权取代的投票方法标准令牌。Curve DAO具有令牌（CRV），可用于治理和价值累加。

查看[documentation](doc/README.md)，以获得有关Curve DAO的工作原理的更深入的说明。

## 测试与开发

### 依赖

* [python3](https://www.python.org/downloads/release/python-368/) version 3.6 or greater, python3-dev
* [vyper](https://github.com/vyperlang/vyper) version [0.2.4](https://github.com/vyperlang/vyper/releases/tag/v0.2.4)
* [brownie](https://github.com/iamdefinitelyahuman/brownie) - tested with version [1.11.0](https://github.com/eth-brownie/brownie/releases/tag/v1.11.0)
* [ganache-cli](https://github.com/trufflesuite/ganache-cli) - tested with version [6.10.1](https://github.com/trufflesuite/ganache-cli/releases/tag/v6.10.1)

### 安装

首先，请首先创建并初始化Python [virtual environment](https://docs.python.org/3/library/venv.html)。接下来，克隆存储库并安装开发人员依赖项：

```bash
git clone https://github.com/curvefi/curve-dao-contracts.git
cd curve-dao-contracts
pip install -r requirements
```

### 运行测试

The test suite is split between [unit](tests/unitary) and [integration](tests/integration) tests. To run the entire suite:
该测试套件分为[单元](tests/unitary)测试和[集成](tests/integration) 测试。要运行整个套件：

```bash
brownie test
```

仅运行单元测试或者集成测试:

```bash
brownie test tests/unitary
brownie test tests/integration
```

## 部署

有关如何部署Curve DAO的详细信息，请查看[部署文档](scripts/README.md) .

## 审计与安全

Curve DAO 合约已通过Trail of Bits和Quantstamp审计。这些审核报告可在[Curve网站](https://dao.curve.fi/audits)上获得。

对于可能导致大量金钱损失，严重错误（例如，不正常的生活状况或不可挽回的资金损失）的问题，这里有个活跃的[bug赏金](https://www.curve.fi/bugbounty)。

## 资源

您可能会发现以下指南很有用：

1. [Curve and Curve DAO Resources](https://resources.curve.fi/)
2. [How to earn and claim CRV](https://guides.curve.fi/how-to-earn-and-claim-crv/)
3. [Voting and vote locking on Curve DAO](https://guides.curve.fi/voting-and-vote-locking-curve-dao/)

## 社区

如果您对此项目有任何疑问，或希望与我们合作：

* [Telegram](https://t.me/curvefi)
* [Twitter](https://twitter.com/curvefinance)
* [Discord](https://discord.gg/rgrfS7W)

## License

该项目已获得 [MIT](LICENSE) 许可.
