# curve-dao-contracts/contracts

所有的合约源码都在这个目录中

## 子目录

* [`testing`](testing): 专门用于测试的合同。不被视为本项目的核心部分。

## 合约

* [`ERC20CRV`](ERC20CRV.vy): Curve DAO Token (CRV), 一个 [ERC20](https://eips.ethereum.org/EIPS/eip-20) 分段线性挖矿的提供者
* [`GaugeController`](GaugeController.vy): 控制流动性估算和通过流动性估算CRV的产出
* [`LiquidityGauge`](LiquidityGauge.vy): 衡量每个用户提供的流动资金量
* [`LiquidityGaugeReward`](LiquidityGaugeReward.vy): 提供流动性和股权的措施[Synthetix rewards contract](https://github.com/Synthetixio/synthetix/blob/master/contracts/StakingRewards.sol)
* [`Minter`](Minter.vy): 用于发行新CRV的挖矿合约
* [`PoolProxy`](PoolProxy.vy): 用于 DAO 和池合约之间交互的 StableSwap 池的代理合约
* [`VestingEscrow`](VestingEscrow.vy): 多个授予周期内为多个地址授予CRV tokens
* [`VestingEscrowFactory`](VestingEscrowFactory.vy): Factory 储存 CRV 和部署许多简化的授予合约
* [`VestingEscrowSimple`](VestingEscrowSimple.vy): 简化的授予合同，为单个地址持有CRV
* [`VotingEscrow`](VotingEscrow.vy): 锁定CRV参与DAO治理的授予合同
