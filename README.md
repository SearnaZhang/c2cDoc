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
            basicInfo.uid 
            basicInfo.phone 
            basicInfo.mail 
            basicInfo.username //若未设置则为""
            basicInfo.username_state // 0未设置，1已设置
            basicInfo.freeze_asset 
            basicInfo.asset  
            basicInfo.lock_asset  =
            basicInfo.create_time = this.create_time;
            deal_pwd_state // 0未设置，1已设置
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
# 获取充币地址
    uri: /service/create_address
    mothod: GET
    input: 
        symbol: string 币种名 btc,eth...
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
        }
# C2C下单
    uri: /c2c/order
    mothod: GET
    input:
        uid: number
        pend_id: string 下单对应的挂单ID
        type: number 买卖类型 （1：购买，2出售）
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
    
