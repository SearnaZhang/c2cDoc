# c2cBackend
c2c

# 注册验证码获取
    uri:/service/register_verify
    mothod: GET
    input:
        phone: string * 电话
        district: string * 国际区号
    return:
        success || error

# 注册
    uri:/service/register
    mothod: GET
    input:
        phone: string * 电话
        mail: string
        pwd: string *
        district: string * 国际区号
    return:
        success || error
        
# 登录验证码获取
    uri:/service/loginVerify
    mothod: GET
    input:
        phone: string  电话 
    return:
        success || error
# 登录
    uri:/service/login
    mothod: GET
    input:
        phone: string  电话 
        pwd: string 密码
    return:
        {
            uid: number
            phone: string
            mail: string
            asset: {}  //资产
            freeze_asset:{} //冻结资产
            lock_asset:{} //锁定资产
            create_time: Date 
        }
# 登录信息获取
    uri:/service/user_info
    mothod:GET
    input:
        null
    return:
        {
            basickinfo:{
                basicInfo.uid 
                basicInfo.phone 
                basicInfo.mail 
                basicInfo.username //若未设置则为""
                basicInfo.username_state // 0未设置，1已设置
                basicInfo.freeze_asset 
                basicInfo.asset  //资产信息
                basicInfo.lock_asset  =
                basicInfo.create_time = this.create_time;
                deal_pwd_state // 0未设置，1已设置
            }
            wechat_state: 0 未设置，1 已设置
            alipay_state: 0 未设置，1 已设置
            bankcard_state: 0 未设置，1 已设置
        }
        
# 登出
    uri: /service/logout
    mothod: GET
    input: NaN
    return: success
# 设置昵称
    uri:/service/set_nickname
    mothod:GET
    input:
        nickname:string 昵称
    return:
        success || error
# 重置昵称
    uri:/service/reset_nickname
    mothod:GET
    input:
        nickname:string 昵称
    return:
        success || error
# 设置交易密码
    uri:/service/set_deal_pwd
    mothod:GET
    input:
        dealPwd:string 交易密码
    return:
        success || error
# 重置交易密码
    uri:/service/reset_deal_pwd
    mothod:GET
    input:
        oldpwd:string 老交易密码
        newpwd:string 新交易密码
    return:
        success || error
# 重置登录密码
    uri:/service/resetpwd
    mothod:GET
    input:
        oldpwd:string 老密码
        newpwd:string 新密码
    return:
        success || error
# 获取币种列表
    uri:/service/coin_list
    mothod:GET
    input: null
    return:
        ['btc','usdt'...]
# 资产首页
    uri:/service/assets_detail
    mothod:GET
    input:null
    return:
        {
            asset:{
                'btc':1,
                ...
            },//资产
            freeze_asset:{
                'btc':1,
                ...
            }//冻结资产
        }
# 获取虚拟币价格
    uri:/service/current_price
    mothod:GET
    input:null
    return:
    {
        usdt: 7
        btc: 53000
        adr: 2
        ald: 3
    }

# 获取充币地址
    uri: /service/create_address
    mothod: GET
    input: 
        symbol: string 币种名 btc,eth...
    return: success || error
# 获取C2C操作验证码
    uri:/c2c/getVerifyCode
    mothod:GET
    input:null
    return: success || error
# C2C挂单
    uri: /c2c/pend
    mothod: GET
    input:
        symbol: string 币种
        amount: number
        price: number 价格（RMB）
        type: number 购买、出售类型（1：购买 2：出售）
        paytype_bank: number 是否支持银行卡（0：不支持，1：支持）
        paytype_wx: number 是否支持微信
        paytype_alipay:number 是否支持支付宝
        minnum: number 支持的最小交易数量
        maxnum: number 支持的最大交易数量
        pwd:string 交易密码
        code:string 验证码
    return:
        {
            uid: uid,
            symbol: symbol,
            amount: mongoose.Types.Decimal128.fromString(amount),
            price: price,
            type: type,
            paytype_bank: paytype_bank,
            paytype_wx: paytype_wx,
            paytype_alipay: paytype_alipay,
            minmum: minmum,
            maxmum: maxmum,
            canceled: 0, //c2c挂单状态（0：正常, 1:已取消）
            time: Date.now(),
            memo: memo, 
        }
# C2C挂单列表获取
    uri:/c2c/getPendList
    mothod:GET
    input:
        symbol:string 币种
    return:
        [
            {
                uid: uid,
                symbol: symbol,
                amount: mongoose.Types.Decimal128.fromString(amount),
                price: price,
                type: type,
                paytype_bank: paytype_bank,
                paytype_wx: paytype_wx,
                paytype_alipay: paytype_alipay,
                minmum: minmum,
                maxmum: maxmum,
                canceled: 0, //c2c挂单状态（0：正常, 1:已取消）
                time: Date.now(),
                memo: memo, 
                deals: Number //月成交量
            },
            ...
        ]
# C2C用户发布挂单列表获取
    uri:/c2c/getUserPendList
    mothod:GET
    input: null
    return:
        [
            {
                uid: uid,
                symbol: symbol,
                amount: mongoose.Types.Decimal128.fromString(amount),
                price: price,
                type: type,
                paytype_bank: paytype_bank,
                paytype_wx: paytype_wx,
                paytype_alipay: paytype_alipay,
                minmum: minmum,
                maxmum: maxmum,
                canceled: 0, //c2c挂单状态（0：正常, 1:已取消）
                time: Date.now(),
                memo: memo, 
                deals: Number //月成交量
            },
            ...
        ]        
# C2C取消挂单
    uri: /c2c/pend_cancel
    mothod: GET
    input:
        pend_id:string 挂单id
    return:
        更新的挂单信息
        {
            uid: uid,
            symbol: symbol,
            amount: mongoose.Types.Decimal128.fromString(amount),
            price: price,
            type: type,
            paytype_bank: paytype_bank,
            paytype_wx: paytype_wx,
            paytype_alipay: paytype_alipay,
            minmum: minmum,
            maxmum: maxmum,
            canceled: 0, //c2c挂单状态（0：正常, 1:已取消）
            time: Date.now(),
            memo: memo, 
            deals: Number //月成交量
        }
# C2C下单
    uri: /c2c/order
    mothod: GET
    input:
        uid: number
        pend_id: string 下单对应的挂单ID
        type: number 买卖类型 （1：购买，2出售）
        amount: number 交易数量
        pwd:string  交易密码
        code: string 验证码
    return:
        C2C下单信息
        {
            symbol: pendRecord.symbol,
            pend_id: pendId,
            pend_type: pendType,
            amount: decimalAmount,
            code: tools.random(10000001, 99999999),
            status: C2COrderModel.STATUS_WAIT_PAY,
            transfer_name: ctx.args.transfer_name,
            transfer_bank: ctx.args.transfer_bank,
            transfer_subbank: ctx.args.transfer_subbank,
            transfer_account: ctx.args.transfer_account,
            seller: sellerId,
            buyer: buyerId,
            time: Date.now(),
        }   
# C2C获取用户订单列表
    uri:/c2c/getOrderList
    mothod:GET
    input:
        status 订单状态
    return:
        {
            total_page 总页数
            total_count z总条数
            [
                {
                    symbol: pendRecord.symbol,
                    pend_id: pendId,
                    pend_type: pendType,
                    amount: decimalAmount,
                    code: tools.random(10000001, 99999999),
                    status: C2COrderModel.STATUS_WAIT_PAY,
                    transfer_name: ctx.args.transfer_name,
                    transfer_bank: ctx.args.transfer_bank,
                    transfer_subbank: ctx.args.transfer_subbank,
                    transfer_account: ctx.args.transfer_account,
                    seller: sellerId,
                    buyer: buyerId,
                    time: Date.now(),
                    pay_type: 付款方式（1，银行卡 2，微信 3，支付宝）
                },
                ...
            ]
        }

# C2C买家确认付款
    uri:/c2c/pay
    mothod: GET
    input:
        order_id: string 对应order_id
        pay_type: number 付款方式（1，银行卡 2，微信 3，支付宝）
    return:
        更新后的订单信息
        {
            symbol: pendRecord.symbol,
            pend_id: pendId,
            pend_type: pendType,
            amount: decimalAmount,
            code: tools.random(10000001, 99999999),
            status: C2COrderModel.STATUS_WAIT_PAY,
            transfer_name: ctx.args.transfer_name,
            transfer_bank: ctx.args.transfer_bank,
            transfer_subbank: ctx.args.transfer_subbank,
            transfer_account: ctx.args.transfer_account,
            seller: sellerId,
            buyer: buyerId,
            time: Date.now(),
            pay_type:  Number,//1：银行卡，2：微信，3：支付宝
        }
# 卖家确认放币
    uri:/c2c/confirm
    mothod: GET
    input: 
        order_id: string 订单id
    return:
        更新后的订单信息
        {
            symbol: pendRecord.symbol,
            pend_id: pendId,
            pend_type: pendType,
            amount: decimalAmount,
            code: tools.random(10000001, 99999999),
            status: C2COrderModel.STATUS_WAIT_PAY,
            transfer_name: ctx.args.transfer_name,
            transfer_bank: ctx.args.transfer_bank,
            transfer_subbank: ctx.args.transfer_subbank,
            transfer_account: ctx.args.transfer_account,
            seller: sellerId,
            buyer: buyerId,
            time: Date.now(),
        }
#  取消订单
    uri:/c2c/cancel
    mothod: GET
    input: 
        order_id: string 订单id
    return:
        更新后的订单信息
        {
            symbol: pendRecord.symbol,
            pend_id: pendId,
            pend_type: pendType,
            amount: decimalAmount,
            code: tools.random(10000001, 99999999),
            status: C2COrderModel.STATUS_WAIT_PAY,
            transfer_name: ctx.args.transfer_name,
            transfer_bank: ctx.args.transfer_bank,
            transfer_subbank: ctx.args.transfer_subbank,
            transfer_account: ctx.args.transfer_account,
            seller: sellerId,
            buyer: buyerId,
            time: Date.now(),
        }
# 订单申诉
    uri:/c2c/appeal
    mothod: GET
    input: 
        order_id: string 订单id
        content: string 申诉内容
    return:
        更新后的订单信息
        {
            symbol: pendRecord.symbol,
            pend_id: pendId,
            pend_type: pendType,
            amount: decimalAmount,
            code: tools.random(10000001, 99999999),
            status: C2COrderModel.STATUS_WAIT_PAY,
            transfer_name: ctx.args.transfer_name,
            transfer_bank: ctx.args.transfer_bank,
            transfer_subbank: ctx.args.transfer_subbank,
            transfer_account: ctx.args.transfer_account,
            seller: sellerId,
            buyer: buyerId,
            buyer_name: 买家用户名
            seller_name: 卖家用户名
            time: Date.now(),
        }
# 获取提币地址列表
    uri:/service/withdraw_address_list
    mothod:GET
    input:
        symbol: string 币种
    return:
        [
            {
                _id: ObjectId,
                address: address,
                symbol: symbol,
                createTime: Date.now(),
                uid: uid,
                name: name,
            },
            {
                ...
            }
        ]

# 添加提币地址
    uri:/service/create_withdraw_address
    mothod:GET
    input:
        symbol: string 币种
        address: string 地址
        code: string 验证码
        name: string 钱包备注
    return:
        {
            _id: ObjectId,
            address: address,
            symbol: symbol,
            createTime: Date.now(),
            uid: uid,
            name: name,
        }

# 删除提币地址
    uri:/service/del_withdraw_address
    mothod:GET
    input:
        id:string 要删除的条目对应的ObjectId
    return:
        success || error

# 获取提币验证码
    uri:/service/custom_withdraw_request
    mothod:GET
    input:
    return:
        success||error

# 提交提币请求
    uri:/service/custom_withdraw_confirm
    mothod:GET
    input:
        symbol 币种
        to 提币地址，不在已添加的提币地址中会失败
        amount 数量
        code 提币验证码
    return:
        {
            symbol: symbol, //币种
            uid: uid,
            to: to, //目标地址
            amount: mongoose.Types.Decimal128.fromString(amount), // 数量
            fee: mongoose.Types.Decimal128.fromString(fee), // 手续费
            confirm: 0, // 0 待确认， 1已确认
            time: Date.now(),
        }
    
# 获取充币记录
    uri:/service/deposit_history
    mothod:GET
    input:
        symbol:string 币种
    return:
        {
            num 总数
            data:
                {
                    uid
                    symbol
                    from
                    to
                    amount
                    txid
                    time
                }
        }
# 获取提币记录
    uri:/service/withdraw_history
    mothod:GET
    input:
        symbol:string 币种
    return:
        {
            num 总数
            data:
                {
                    uid
                    symbol
                    from
                    to
                    amount
                    txid
                    time
                }
        }
# 添加微信收款方式
    uri:/service/addWechat
    mothod:POST
    input:
        pwd 交易密码
        code 验证码
        account 微信账号
        name 姓名
        file 二维码图片(jpg 或者 png)
    return:
        success || error
# 删除微信收款方式
    uri:/service/delWechat
    mothod:POST
    input:
        pwd 交易密码
        code 验证码
    return:
        success || error
# 添加银行卡收款方式
    uri:/service/addBankcard
    mothod:GET
    input:
        pwd 交易密码
        code 验证码
        name 开户名
        register_bank 开户行
        second_bank 支行
        card 卡号
    return:
        success || error
# 删除银行卡收款方式
    uri:/service/delBankcard
    mothod:GET
    input:
        pwd 交易密码
        code 验证码
    return:
        success || error
# 添加支付宝收款方式
    uri:/service/addAlipay
    mothod:POST
    input:
        pwd 交易密码
        code 验证码
        account 支付宝账号
        name 姓名
        file 二维码图片(jpg 或者 png)
    return:
        success || error
# 删除支付宝收款方式
    uri:/service/delAlipay
    mothod:POST
    input:
        pwd 交易密码
        code 验证码
    return:
        success || error
# 获取收款方式
    uri:/service/getPayPath
    mothod:GET
    input:
        uid 收款人uid
        paytype: 渠道(1.银行卡，2.微信，3.支付宝)
    return:
        //银行卡
        {
            name: String, //姓名
            register_bank: String, //开户行
            second_bank: String, //支行
            card: String, //卡号
            createTime: Date,
            uid: Number,
            status: { type: Number, default: 0 },
        }
        //微信，支付宝 
        {
            name: String, //姓名
            account: String, //微信账号
            path: String, //二维码路径
            createTime: Date,
            uid: Number,
            status: { type: Number, default: 0 },//0 有效 1 已删除
        }
        收款码文件名命名方式为 wechat+uid+后缀|| alipay+uid+后缀  如 wechat10001.jpg || alipay10001.png 
        收款码访问uri为 ip:port/wechat10001.jpg || ip:port/alipay10001.jpg
# 获取商家名称
    uri:/service/getNickName
    mothod:GET
    input:
        uid 商家uid
    return:
        "xxx" 商家名称 || "" 未查找到


# order订单状态
    es.STATUS_WAIT_PAY = 0; 等待付款
    es.STATUS_FINISHED = 1; 已完成
    es.STATUS_CANCELED = 2; 已取消
    es.STATUS_PAYED = 3; 已付款
    es.STATUS_APPEAL = 4; 已申诉
    es.STATUS_APPEAL_FINISHED = 5; 申诉完成，成交
    es.STATUS_APPEAL_CANCELED = 6; 申诉完成，已取消
