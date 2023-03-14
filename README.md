---
eip: 9999
title: 元宇宙公开资源协议（Metaverse Public Source Protocol）
description: 元宇宙公共资源协议的技术标准
author: Changchun Chen, Derek Zhou(@zhous,@zhous98@daotodon.me)
discussions-to: 
status: Draft
type: Standards Track
category: ERC
created: 2023-03-08
requires: 165
---

## 摘要（Abstract）

本文档中描述的智能合约接口集不兼容现有EIP标准。该标准引入了SLOT、SOURCE两重结构模型。两重结构模型之间的关系灵活的体现了元宇宙的公共资源关系。该标准引入了新的转移与授权模型。

标准引入了slot使得合约可以纵向扩展，与source形成一对多的关系。

由slotId和sourceId共同定义一个具体的公共资源,标识每个资源是独一无二的.

这样就形成了一个类似菜单的关系，使得资源的整合与运用更加灵活。



## 动机（Motivation）
致力于规范元宇宙公共资源开放协议暨元宇宙公共资源库的技术标准。它是“Code is Law”的一次实践，即“Smart Contract is Law”，只要你的智能合约与该技术标准对应的智能合约进行了交互，那就意味着接受了将自己的智能合约及其所有资源定为元宇宙公共资源这一协议。同时，这一智能合约就成为元宇宙里所有公共资源的检索库。
从技术上，元宇宙需要一种灵活的资源及资源量化的管理工具，该标准提供了便利的定义接口集。


## 技术规范（Specification）



每个符合EIP-9999 的合约都必须实现 EIP-9999和EIP-165接口

```solidity

pragma solidity ^0.8.6;
import "@openzeppelin/contracts/utils/introspection/IERC165.sol";
interface IMNFTCore is IERC165 {
    struct SourceInfo{
        uint256 id;
        string symbol;
        uint8 decimals;
        uint256 amount;
    }
    /**
        说明：获取拥有者卡槽数量
        参数：
        _owner：拥有者
        返回：
        数量
     */
    function slotLengthOf(address _owner) external view  returns (uint256) ;
     /**
        说明：通过卡槽获取资源数量
        参数：
        _slotId：卡槽ID
        返回：
        数量
     */
    function sourcesLengthOf(uint256 _slotId) external view returns (uint256) ;
    /**
        说明：获取拥有者所有卡槽ID
        参数：
        _owner：拥有者
        返回：
        卡槽ID数组
     */
   function slotListOfAll(address _owner) external view  returns (uint256[] memory);
   /**
        说明：分页获取拥有者卡槽ID
        参数：
        _owner：拥有者
        _pageNum：页码（从0开始）
        _pageSize：每页数量
        返回：
        卡槽ID数组
     */
   function slotListOfPage(address _owner,uint256 _pageNum,uint256 _pageSize) external view returns(uint256[] memory);
   /**
        说明：通过slotID获取所有资源ID
        参数：
        _slotId：卡槽ID
        返回：
        资源ID数组
     */
    function sourcesListOfAll(uint256 _slotId) external view returns (uint256[] memory);
     /**
        说明：通过slotID分页获取资源ID
        参数：
        _slotId：卡槽ID
        _pageNum：页码（从0开始）
        _pageSize：每页数量
        返回：
        资源ID数组
     */
    function sourcesListOfPage(uint256 _slotId,uint256 _pageNum,uint256 _pageSize) external view returns(uint256[] memory);
    
    /**
        说明：通过slotId获取拥有者资源ID数量
        参数：
        _owner：拥有者
        _slotId：卡槽ID
        返回：
        资源ID数量
     */
    function assetsLengthOf(address _owner,uint256 _slotId) external view  returns(uint256);
    /**
        说明：通过slotId获取拥有者所有资源ID
        参数：
        _owner：拥有者
        _slotId：卡槽ID
        返回：
        资源ID数组
     */
    function assetsListOfAll(address _owner,uint256 _slotId) external view  returns(uint256[] memory);
    /**
        说明：通过slotId分页获取拥有者资源ID
        参数：
        _owner：拥有者
        _slotId：卡槽ID
        _pageNum：页码（从0开始）
        _pageSize：每页数量
        返回：
        资源ID数组
     */
    function assetsListOfPage(address _owner,uint256 _slotId,uint256 _pageNum,uint256 _pageSize) external view  returns(uint256[] memory);
    /**
        说明：获取授权资源数量
        参数：
        _owner：拥有者
        _operator：被授权地址
        _slotId：卡槽ID
        返回：
        数量
     */
    function approveSourceLengthOf(address _owner,address _operator,uint256 _slotId) external view  returns(uint256);
    /**
        说明：获取授权资源ID数组
        参数：
        _owner：拥有者
        _operator：被授权地址
        _slotId：卡槽ID
        返回：
        资源ID数组
     */
    function approveSourceListOfAll(address _owner,address _operator,uint256 _slotId) external view  returns(uint256[] memory);
    /**
        说明：分页获取授权资源ID数组
        参数：
        _owner：拥有者
        _operator：被授权地址
        _slotId：卡槽ID
        _pageNum：页码（从0开始）
        _pageSize：每页数量
        返回：
        资源ID数组
     */
    function approveSourceListOfPage(address _owner,address _operator,uint256 _slotId,uint256 _pageNum,uint256 _pageSize) external view  returns(uint256[] memory);
    
    /**
        说明：授权资源
        参数：
        _to：被授权地址
        _slotId：卡槽ID
        _sourceIds：资源ID数组
        返回：
        
     */
    function approveSources(address _to, uint256 _slotId,uint256[] memory _sourceIds) external   ;
   
    /**
        说明：转移资源
        参数：
        _from：从from地址转移到to地址
        _to：从from地址转移到to地址
        _slotId：卡槽ID
        _sourceIds：转移到资源id数组
        返回：
        
     */
    function transferSourcesFrom(
        address _from,
        address _to,
        uint256 _slotId,
        uint256[] memory _sourceIds
    ) external ;
    /**
        说明：获取拥有者地址
        参数：
        _slotId：卡槽ID
        _sourceId：资源id
        返回：
        拥有者地址
     */
    function ownerOf(uint256 _slotId,uint256 _sourceId) external view   returns (address);
     /**
        说明：获取卡槽创建者地址
        参数：
        _slotId：卡槽ID
        返回：
        创建者地址
     */
    function minterOf(uint256 _slotId) external view   returns (address) ;
     /**
        说明：授权所有资源
        参数：
        _to：被授权地址
        _slotId：卡槽ID
        返回：
        
     */
    function approveAll(address _to, uint256 _slotId) external   ;

    /**
        说明：转移资源,校验实现接口
        参数：
        _from：从from地址转移到to地址
        _to：从from地址转移到to地址
        _slotId：卡槽ID
        _sourceIds：转移到资源id数组
        返回：
        
     */

    function safeTransferSourcesFrom(
        address _from,
        address _to,
        uint256 _slotId,
        uint256[] memory _sourceIds
    ) external  ;
    /**
        说明：转移资源,校验实现接口
        参数：
        _from：从from地址转移到to地址
        _to：从from地址转移到to地址
        _slotId：卡槽ID
        _sourceIds：转移到资源id数组
        _data：携带数据
        返回：
        
     */

    function safeTransferFrom(
       address _from,
        address _to,
        uint256 _slotId,
        uint256[] memory _sourceIds,
        bytes memory _data
    ) external  ;
    /** 
        说明：创建资源,校验实现接口
        参数：
        _to：资源拥有者
        _slotId：卡槽ID
        _sourceIds：资源id数组
        返回：
        
     */
    function safeMint(address _to, uint256 _slotId,uint256[] memory _sourceIds) external payable;
    /** 
        说明：创建资源,校验实现接口
        参数：
        _to：资源拥有者
        _slotId：卡槽ID
        _sourceIds：资源id数组
        _data：携带数据
        返回：
        
     */
    function safeMint(address _to, uint256 _slotId,uint256[] memory _sourceIds,bytes memory data) external payable ;
    /** 
        说明：创建资源,不校验实现接口
        参数：
        _to：资源拥有者
        _slotId：卡槽ID
        _sourceIds：资源id数组
        返回：
        
     */
    function mint(address _to, uint256 _slotId,uint256[] memory _sourceIds) external payable  ;


}
```



### EIP-9999 资源转移接收器

如果一个智能合约想要在从其他地址接收到资源时得到通知，它应该实现接口中的所有功能IMNFTCoreReceiver，在实现中它可以决定是接受还是拒绝传输。有关详细信息，请参阅“传输规则”。

```solidity

pragma solidity ^0.8.6;
interface IMNFTCoreReceiver {
    
    function onMNFTCoreReceived(
        address operator,
        address from,
        uint256 slotId,
        uint256[] memory sourceIds,
        bytes calldata data
    ) external returns (bytes4);
    
}
```

### 资源交易

#### 场景

**转移:_**

本次EIP的转账模型：sourceId的转移

```solidity
    function transferSourcesFrom(
        address _from,
        address _to,
        uint256 _slotId,
        uint256[] memory _sourceIds
    ) external ;
	
```

允许槽内的资源进行批量转移

#### 规则

**审批规则:_**

本EIP提供的审批类型：针对source的审批

- approveSources，批准授权的资源，这意味着授权运营商能够管理所有者批准的资源集合.
- 对于任何批准功能，调用者必须是所有者或已获得更高级别的权限。

**transferFrom 规则:_**

- function transferSourcesFrom(address _from,address _to,uint256 _slotId,uint256[] memory _sourceIds) 该函数应该根据以下规则指示从一个地址到两一个地址转移:

  - _from 必须是所有者或者被授权的_sourceIds操作者。
  - _from、_to为0地址则失败。
  - _slotId不存在则失败。
  - 该slotId下的具体的资源ID合集.
  - 如果_sourceIds 不属于_from 所有或者未被授权则失败。


## 技术原理（Rationale）


### 设计决策：
slot与source的一对多关系形成了树形结构,其中slotId和sourceId共同标识一个具体的资源,使得通过该协议注册之后的公共资源能够标识为独一无二的公共资源,这种不采用数字量化的方式更能适用与元宇宙资源.资源的设计者更能灵活的给资源赋予其属性满足元宇宙多样化的生态.

## 向下兼容（Backwards Compatibility）
无

## 测试用例（Test Cases）


## 参考实例（Reference Implementation）



## 版权

通过CC0放弃版权和相关权利。
