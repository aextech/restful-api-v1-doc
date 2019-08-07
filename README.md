AEX RESTful API 协议说明文档 （V1）
---

# 目录
+ [公开数据接口](#请求应答)
   + [获取交易对行情数据](#获取指定交易对行情数据)
   + [获取交易对深度数据](#获取指定交易对深度)
   + [获取交易对历史成交数据](#获取指定交易对历史成交数据)
+ [用户数据接口]
   + [我的账户余额]
   + [挂单接口 / 错误码]
   + [撤单]
   + [我的挂单]
   + [我的成交记录]

# submitOrder.php的错误码
错误码 | 说明
-----  | ---------
"[]"      | 系统错误
"param time should be a timestamp"   | time参数错误，正确的time参数是一个10位整数，单位是秒，不是毫秒
"param time over 30 seconds"   | time参数跟服务器时间相比偏移超过30秒
"public key error"   | 公钥格式错误
"wrong public key"   | 公钥不存在
"wrong md5 value"   | md5参数错误，跟服务器计算出来的不一致
"{IP} is not allowed."   | IP不在白名单中
"system_busy"   | 系统错误
"input_error1"  | 请求参数错误
"noOrder"  | mk_type和c参数指定的交易对无效
"invalid_market_name" | mk_type参数指定的市场无效
"trade_conf_err" | 系统错误，交易参数配置无效
"invalid_order_type" | type参数错误，只有1=买单，2=卖单
"input_error2" |  挂单价格无效
"sys_amt_precision_err#1" | 系统错误，挂单数量精度配置错误
"sys_amt_precision_err#2" | 系统错误，挂单数量精度配置错误
"input_error3" | 挂单数量精度超过配置
"min" | 挂单数量超过最大限制
"sml" | 挂单数量少于最小限制
"lag" | 挂单价格超过最大限制
"sys_price_precision_conf" | 系统错误，价格精度配置错误
"sys_price_precision_err#1" | 挂单价格精度超过8位
"sys_price_precision_err#2" | 挂单价格精度<0
"deciError2" | 挂单价格精度超过系统配置
"amountsmall" | 挂单金额少于最小限制: 挂单金额=挂单价格x挂单数量
"succ" | 当前挂单直接撮合了，没有生成订单
"succ\|{number}" | 当前挂单部分成交或者完全没有成交，剩余的部分生成了订单，订单ID是number
"system_busy#8" | 可能已经有部分成交或者完全没成交，剩余的部分生成订单失败



# 请求/应答
## 获取所有交易对信息   

  请求URL
  ```
  GET /v2/allpair.php
  ```
  请求参数: 无
  
  应答（json）
  ```
  {
    "eno":0,     // 错误码
    "emsg":"",   // 错误描述
    "data":[
      {
        "market":"usdt",  // 市场
        "coin":"btc",     // 币名
        "limits":{
          "PricePrecision":0,  // 挂单价格精度
          "AmtPrecision":6,    // 挂单数量精度
          "AmtMax":"10000000.00000000",  // 最大挂单数量
          "AmtMin":"0.00000100",         // 最小挂单数量
          "PriceMax":"1000000.00000000", // 最大挂单价格
          "MoneyMin":"1.00000000"        // 最小挂单资金 <= 挂单价格* 挂单数量
        }
      }
    ]
  }	
  ```
## 获取指定交易对信息   

  请求URL
  ```
  GET /v2/pair.php?market={market}&coin={coin}
  ```
  请求参数:   
  
  参数名  | 说明
  -----  | ---------
  market | 交易区，比如 cnc
  coin   | 币名，比如 btc
  
  
  应答（json）
  ```
  {
    "eno":0,     // 错误码
    "emsg":"",   // 错误描述
    "data":[
      {
        "market":"usdt",  // 市场
        "coin":"btc",     // 币名
        "limits":{
          "PricePrecision":0,  // 挂单价格精度
          "AmtPrecision":6,    // 挂单数量精度
          "AmtMax":"10000000.00000000",  // 最大挂单数量
          "AmtMin":"0.00000100",         // 最小挂单数量
          "PriceMax":"1000000.00000000", // 最大挂单价格
          "MoneyMin":"1.00000000"        // 最小挂单资金 <= 挂单价格* 挂单数量
        }
      }
    ]
  }	
  ```  
## 获取所有交易对行情数据   

  请求URL
  ```
  GET /v2/alltickers.php
  ```
  请求参数: 无
  
  应答（json）
  ```
  {
    "eno":0,     // 错误码
    "emsg":"",   // 错误描述
    "data":[
      {
        "market":"cnc",   // 市场
        "coin":"btc",     // 币名
        "ticker":{
          "high":75501,     // 24小时内的最高价
          "low":72231,      // 24小时内的最低价
          "last":74654,     // 最近1次成交价
          "vol":6448.69997, // 24小时成交量
          "buy":74666,      // 当前买1价
          "sell":74728,     // 当前卖1价
          "range":0.0257    // 最新成交价跟24小时之前成交价的波动比例
        }
      }
    ]
  }
  ```  
## 获取指定交易对行情数据   

  请求URL
  ```
  GET /v2/tickers.php?market={market}&coin={coin}
  ```
  请求参数:   
  
  参数名  | 说明
  -----  | ---------
  market | 交易区，比如 cnc
  coin   | 币名，比如 btc
  
  
  应答（json）
  ```
  {
    "eno":0,     // 错误码
    "emsg":"",   // 错误描述
    "data":[
      {
        "market":"cnc",   // 市场
        "coin":"btc",     // 币名
        "ticker":{
          "high":75501,     // 24小时内的最高价
          "low":72231,      // 24小时内的最低价
          "last":74654,     // 最近1次成交价
          "vol":6448.69997, // 24小时成交量
          "buy":74666,      // 当前买1价
          "sell":74728,     // 当前卖1价
          "range":0.0257    // 最新成交价跟24小时之前成交价的波动比例
        }
      }
    ]
  }
  ```    
## 获取指定交易对深度   

  请求URL
  ```
  GET /v2/depth.php?market={market}&coin={coin}
  ```
  请求参数:   
  
  参数名  | 说明
  -----  | ---------
  market | 交易区，比如 cnc
  coin   | 币名，比如 btc
  
  
  应答（json）
  ```
  {
    "eno":0,    // 错误码
    "emsg":"",  // 错误描述
    "data":{
      "bids":[
        [
          74665,    // 买价
          0.13298   // 买量
        ]
      ],
      "asks":[
        [
          74734,    // 卖价
          1.198882  // 卖量
        ]
      ]
    }
  }
  ```    
## 获取指定交易对最新成交数据   

  请求URL
  ```
  GET /v2/lasttrades.php?market={market}&coin={coin}&limit={limit}
  ```
  请求参数:   
  
  参数名  | 说明
  -----  | ---------
  market | 交易区，比如 cnc
  coin   | 币名，比如 btc
  limit  | 查询数量，可选，默认是10，最大为50
  
  
  应答（json）
  ```
  {
    "eno":0,    // 错误码
    "emsg":"",  // 错误描述
    "data":[
      {
        "tradeid":3428804,  // 成交ID
        "time":1563413853,  // 成交时间，单位秒
        "type":"buy",       // 成交类型: buy=买, sell=卖
        "price":40200,      // 成交价
        "amount":0.004975   // 成交数量
      }
    ]
  }
  ```      
## 获取指定交易对历史成交数据   

  请求URL
  ```
  GET /v2/trades.php?market={market}&coin={coin}&fromid={fromTradeID}&limit={limit}
  ```
  请求参数:   
  
  参数名  | 说明
  -----  | ---------
  market | 交易区，比如 cnc
  coin   | 币名，比如 btc
  fromid | 从哪个交易ID开始查询，结果包含此交易ID
  limit  | 查询数量，可选，默认是10，最大为50
  
  
  应答（json）
  ```
  {
    "eno":0,    // 错误码
    "emsg":"",  // 错误描述
    "data":[
      {
        "tradeid":3428804,  // 成交ID
        "time":1563413853,  // 成交时间，单位秒
        "type":"buy",       // 成交类型: buy=买, sell=卖
        "price":40200,      // 成交价
        "amount":0.004975   // 成交数量
      }
    ]
  }
  ```      
  
  
