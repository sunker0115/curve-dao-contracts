# curve-dao-contracts/scripts

Curve DAO的部署脚本。

## 依赖 - Dependencies

* [Brownie](https://github.com/eth-brownie/brownie)
* [Aragon CLI](https://github.com/aragon/aragon-cli)
* [Ganache](https://github.com/trufflesuite/ganache-cli)

## Process Overview

### 1. 初始化安装 - Initial Setup

[`deployment_config.py`](deployment_config.py) 保存与部署相关的可配置/敏感值。开始之前，必须设置以下变量：

* 修改 `get_live_admin` 函数返回主管理员 [`Account`](https://eth-brownie.readthedocs.io/en/stable/api-network.html#brownie.network.account.Account) 和四个资金管理员账户.有关如何解锁本地账户的信息，请参阅Brownie [account management](https://eth-brownie.readthedocs.io/en/stable/account-management.html)。
* 在`STANDARD_ESCROWS` 和 `FACTORY_ESCROWS` 中设置授权信息. 每个变量的结构都在注释中列出.
* 确认`LP_VESTING_JSON` 指向定义 [每个历史LP将获得的百分比](https://github.com/curvefi/early-user-distribution/blob/master/output-with-bpt.json)的JSON。

### 2. Deploying the Curve DAO

1. 如果你尚未准备好, 请安装 [Brownie](https://github.com/eth-brownie/brownie):

    ```bash
    pip install eth-brownie
    ```

2. 通过一个分支主网测试来验证 [`deploy_dao`](deploy_dao.py):

    ```bash
    brownie run deploy_dao development --network mainnet-fork
    ```

3. 运行第一阶段 [`deploy_dao`](deploy_dao.py) 的脚本:

    实时部署将分为两个调用。第一个操作仅部署
    `ERC20CRV` and `VotingEscrow`:

    ```bash
    brownie run deploy_dao live_part_one --network mainnet
    ```

    通过部署这些合约，可以在部署Curve DAO的其余部分时开始Aragon DAO的设置。

4. 运行第二阶段 [`deploy_dao`](deploy_dao.py)的脚本:

    ```bash
    brownie run deploy_dao live_part_two --network mainnet
    ```

    这个部署将链接所有核心Curve DAO合约。生成一个包含每个已部署合约地址的JSON文件. **请勿移动或删除此文件**. 以后的部署阶段需要它.

### 3. 部署 Aragon DAO

1. 如果尚未安装，请安装 [Aragon CLI](https://github.com/aragon/aragon-cli):

    ```bash
    npm install --global @aragon/cli
    ```

Aragon: [自定义部署](https://hack.aragon.org/docs/guides-custom-deploy)

# 部署 [Curve Aragon 投票 App](https://github.com/curvefi/curve-aragon-voting/blob/master/README.md)

# 部署 Aragon DAO 
    
阅读 [Deploy Aragon DAO ](./Deploy_Aragon_DAO_README.md) 中的说明

成功部署DAO后, 修改 [`deployment_config`](deployment_config.py) 使 `ARAGON_AGENT` 指向 [Aragon Ownership Agent](https://github.com/aragon/aragon-apps/blob/master/apps/agent/contracts/Agent.sol) 部署.

部署 Curve Voting App 和 VotingEscrow 的 subgraphs

### 4. 将Curve DAO的所有权转让给Aragon

1. 通过一个分支主网测试来验证 [`transfer_dao_ownership`](transfer_dao_ownership) :

    ```
    brownie run transfer_dao_ownership development --network mainnet-fork
    ```

2. 运行 [`transfer_dao_ownership`](transfer_dao_ownership) 脚本:

    修改 [`deployment_config`](deployment_config.py) 使 `ARAGON_AGENT` 指向 [Aragon Ownership Agent](https://github.com/aragon/aragon-apps/blob/master/apps/agent/contracts/Agent.sol) 部署地址.然后:

    ```bash
    brownie run transfer_dao_ownership live --network mainnet
    ```

    这将使 [`GaugeController`](../contracts/GaugeController.vy), [`PoolProxy`](../contracts/PoolProxy.vy), [`VotingEscrow`](../contracts/VotingEscrow.vy) 和 [`ERC20CRV`](../contracts/ERC20CRV.vy) 的所有权从主管理员账户转移到 [Aragon Ownership Agent](https://github.com/aragon/aragon-apps/blob/master/apps/agent/contracts/Agent.sol).

### 5. 分配token归属

在流动性提供者和其它账户(股东，团队等)之间分配.

1. 这个阶段配置 [`deployment_config`](deployment_config.py) :

    * 在`get_live_admin`中设置资金管理员。资金管理员是临时管理员帐户，只能用于创建新的归属。它们用于更快地完成分配工作-该脚本进行了100多个事务！
    * 在`STANDARD_ESCROWS` 和 `FACTORY_ESCROWS`中添加有关已授权帐户的信息。该结构在配置文件的注释中列出。

2. 通过本地测试验证 [`vest_lp_tokens`](vest_lp_tokens.py) :

    ```bash
    brownie run vest_lp_tokens development
    ```

3. 运行 [`vest_lp_tokens`](vest_lp_tokens.py) 脚本:

    确认管理员和资金帐户中有足够的以太币来支付所需的gas费用。然后：

    ```bash
    brownie run vest_lp_tokens live --network mainnet
    ```

    此脚本生成一个JSON日志文件，以防在执行过程中失败，因此您可以确切地知道哪些事务已完成。

4. 通过本地测试验证 [`vest_other_tokens`](vest_other_tokens.py) :

    ```bash
    brownie run vest_other_tokens development
    ```

5. 运行 [`vest_other_tokens`](vest_other_tokens.py) 脚本:

    ```bash
    brownie run vest_other_tokens live --network mainnet
    ```
6. Transfer reserve vesting escrow admin to Aragon Ownership Agent

### 6. 将Curve池的所有权转让给Aragon

转让池的所有权，第一次调用和第二次调用间隔需要有三天的时间。调用地址必须是[pool owner](https://etherscan.io/address/0xc447fcaf1def19a583f97b3620627bf69c05b5fb).

1. 通过一个分支主网测试来验证 [`transfer_pool_ownership`](transfer_pool_ownership.py):

    Ganache允许我们在不解锁帐户的情况下从池所有者广播-这里不需要设置。

    ```bash
    brownie run transfer_pool_ownership development --network mainnet-fork
    ```

    此测试验证初始提交和最终传输。

2. 运行 [`transfer_pool_ownership`](transfer_pool_ownership.py) 脚本以启动所有权转移:

    在运行此脚本之前，请确保池所有者帐户已被解锁。

    ```bash
    brownie run transfer_pool_ownership live --network mainnet
    ```

3. 三天后，运行 [`transfer_pool_ownership`](transfer_pool_ownership.py) 再次申请转移:

    ```bash
    brownie run transfer_pool_ownership live --network mainnet
    ```


Subgraph 安装UI

部署 [connect-thegraph-voting](https://github.com/curvefi/connect-thegraph-voting)

部署 [votingescrow-subgraph](https://github.com/curvefi/votingescrow-subgraph)