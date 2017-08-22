[TOC]

# loadrunner utils

## 设置sockets选项

```
	web_set_sockets_option("SSL_VERSION","TLS");
```

## 添加请求头

```
	web_add_header("X-Requested-With","XMLHttpRequest");
```

## 开启/结束事务

```
	lr_start_transaction("agent_authentication");

	lr_end_transaction("agent_authentication", LR_PASS);
```

## 关联数据

```
	web_reg_save_param("lt_status",
		"LB=",
		"RB=",
		"SaveLen=2",
		"Search=Body",
		LAST);

	web_reg_save_param("lt",
		"LB=OK_",
		"RB=",
		"Search=Body",
		LAST);

	// 关联多个数据，保存为Array
	web_reg_save_param("orderNums",
		"LB=\"orderNum\":\"",
		"RB=\"",
	    "ORD=All",
		"Search=Body",
		LAST);

```

## 自定义请求

```
	web_custom_request("agent_login",
		"URL=https://sso.dinghuo123.com/login",
		"Method=POST",
		"TargetFrame=",
		"Resource=0",
		"Referer=",
		"Mode=HTTP",//若需要下载login页面资源则需要将Mode修改为HTML，若不需要下载login页面资源则将Mode修改为HTTP
		"Body=username={username}&password=123456&verifyCode=&remember_me=on&lt={lt}&service=&relayState=",
		LAST);

	web_custom_request("agent_v2_goods_list",
		"URL=https://agent.dinghuo123.com/v2/goods/list",
		"Method=GET",
		"TargetFrame=",
		"Resource=0",
		"Referer=",
		"Body=",
		LAST);

```

## 状态码判断

```
	int httpRetCode;
	httpRetCode = web_get_int_property(HTTP_INFO_RETURN_CODE);
	if (httpRetCode == 200)
		lr_end_transaction("agent_authentication", LR_PASS);
	else
		lr_end_transaction("agent_authentication", LR_FAIL);
```

## 关联数据判断

```
	//比较关联数据
	if (strcmp(lr_eval_string("{lt_status}"), "OK") ==0 )
		lr_end_transaction("agent_authentication", LR_PASS);
	else
		lr_end_transaction("agent_authentication", LR_FAIL);
```

## 时间戳

```
	web_save_timestamp_param("timeStamp", LAST);
```