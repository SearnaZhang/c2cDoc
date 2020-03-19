"use strict";

//common
module.exports.E_NO_ERROR = 0;
module.exports.E_ERROR = 10000;
module.exports.E_SERVER_EXCEPTION = 10001;
module.exports.E_ARGUMENTS_ERROR = 10002; //参数错误
module.exports.E_SECURE_ERROR = 10003; //非法数据
module.exports.E_PLEASE_RETRY = 10004;//服务器忙
module.exports.E_SIGN_ERROR = 10005;
module.exports.E_ACTION_FAILED = 10006;//服务器原因操作失败

//database
module.exports.E_DATABASE_QUERY = 11000;
module.exports.E_PHONE_NOT_FOUND = 11001; //未找到用户
module.exports.E_MAIL_EXISTS = 11002;//邮箱已注册
module.exports.E_INFO_NOT_FOUND = 11003;//未找到请求对应的信息
module.exports.E_PHONE_EXISTS = 11004;//号码已注册

//passport
module.exports.E_AUTH = 12000;
module.exports.E_PHONE_LOGIN_NOT_EXISTS = 12001;//未登录
module.exports.E_VERIFY_CODE_NOT_MATCH = 12002;//验证码错误
module.exports.E_PASSWORD_NOT_MATCH = 12003; //密码错误
module.exports.E_DEAL_PASSWORD_NOT_MATCH = 12004;//交易密码错误


//segment
module.exports.E_SEGMENT_NOT_EXISTS = 13000;
module.exports.E_VIDEO_NOT_EXISTS = 13001;
module.exports.E_NOT_ENOUGH_MONEY = 13002;
module.exports.E_FILE_304 = 13003; 
module.exports.E_BAD_FILE = 13004;  //文件类型不支持
module.exports.E_FILE_RECEIVE_FAIL = 13005;//文件上传失败

//Pay
module.exports.E_BAD_RECEIPT = 14000;
module.exports.E_BAD_ALREADY_PAID = 14001;

//userinfo
module.exports.E_INFO_EXISTS = 15000;//信息已存在
module.exports.E_ACCOUNT_LOCKED = 15001; //账户已锁定
module.exports.E_VERIFY_CODE_NOT_EXPIRE = 15002; //当前存在有效的验证码
module.exports.E_INVALID_ADDRESS = 15003;//无效的地址
module.exports.E_FUNDS_ERROR = 15004;//资产不足
module.exports.E_FREEZE_FAILED = 15005;//冻结资产失败
module.exports.E_DEFREEZE_FAILED = 15006;//解冻资产失败
module.exports.E_TRANSFER_FAILED = 15007;//转移资产失败


//withdraw
module.exports.E_RECORD_EXITS = 16000;//提币记录已存在

//pend
module.exports.E_PEND_EXITS = 17000;//已存在类似挂单
module.exports.E_PEND_NOT_FOUND = 17001;//未找到满足条件的挂单，可能挂单数据已变化

//order
module.exports.E_ORDER_ERROR = 18000;//系统原因下单失败
module.exports.E_ORDER_NOT_FOUND = 18001;//未找到订单