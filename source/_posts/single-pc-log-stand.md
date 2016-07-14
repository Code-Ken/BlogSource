title: 单机做日志规范
date: 2016-07-14 12:01:35
categories: 规范
tags: [Log,Seaslog,日志]
---

<center>单机做日志制定的规范</center>

<!--more-->

## 日志说明

日志类是基于 [SeasLog](https://github.com/Neeke/SeasLog) 的二次封装。

## 日志路径
日志的根目录`{app}log`在入口文件index.php中已设置好，为方便调试，可以注释掉原有路径，设置成自己本地路径：

```
<?php

//SeasLog::setBasePath('/Users/Ken/log/seaslog/vyx/www');   
SeasLog::setBasePath('自己本地路径的绝对地址');     //设置成自己本地路径
```
`⚠️注意：任何人禁止上传自己本地配置到版本库！！！`

## 日志结构

日志结构为

```
└── {app}log                -------        APP日志
    ├── warning             -------        警告日志
    ├── error               -------        错误日志
    ├── emergency           -------        紧急错误日志
    └── default             -------        默认日志文件夹
        ├── debug           -------        debug日志
        ├── oooo            -------        oooo日志
        └── xxxx            -------        xxxx日志
```

## 日志级别

日志级别分为五种：

- 警告日志(warning) => 如：前台传参有问题，有非法字符，异地登录等
- 错误日志(error) => 影响项目运行的错误
- 紧急日志(emergency) => 严重影响项目运行的错误，需要运维人员马上处理的
- 一般日志(info) => 重要参数传递接收日志，订单日志，操作日志等
- 调试日志(debug) => 开发调试时打印出的日志

一般日志和调试日志都放到default文件夹下，其他日志放到相对应文件夹下

## 日志格式

log的同一格式为

```
{type} | {pid} | {timeStamp} |{dateTime} | {logInfo}
//demo
debug | 29657 | 1461229197.928 | 2016:04:21 16:59:57 | aaaaa 
```

## 日志调用方式

```
//错误日志调用方式
Lib_Log::errorLog($message)
//默认为error级别，若想修改成warning或emergency级别需加入第二个参数
Lib_Log::errorLog($message,'warning'|'emergency')

//一般日志调用方式
Lib_log::log($message,$model)
//$model为日志模块

//debug日志调用方式
Lib_log::debugLog($message)
```

`⚠️注意：任何人禁止将debug函数传至svn`

调用相关函数后，日志会保存到相应的文件夹

## demo

```

Lib_Log::errorLog('this is error log');
Lib_Log::errorLog('this is warning log','warning');
Lib_Log::errorLog('this is emergency log','emergency');
Lib_Log::log('this is info log for oooo','oooo');
Lib_Log::log('this is info log for xxxx','xxxx');
Lib_Log::debugLog('this is debug log ');
```

```
www/
├── default
│   ├── debug
│   │   └── debug.20160421.log
│   ├── oooo
│   │   └── info.20160421.log
│   └── xxxx
│       └── info.20160421.log
├── emergency
│   └── emergency.20160421.log
├── error
│   └── error.20160421.log
└── warning
    └── warning.20160421.log
```

## 源代码

```
<?php

/**
 * Class Lib_Log 日志类
 */
class Lib_Log
{
    /**
     * 错误日志函数
     * @param $message
     * @param string $level
     * @return bool
     */
    public static function errorLog($message, $level = "error")
    {
        $map = array(
            "warning" => SEASLOG_WARNING,
            "error" => SEASLOG_ERROR,
            "emergency" => SEASLOG_EMERGENCY
        );
        if (!in_array($level, $map)) {
            return false;
        }

        $ext_info = "";
        $trace = debug_backtrace();
        if (count($trace)) {
            $tmp = isset($trace[1]) ? " | 执行函数名: " . $trace[1]['function'] : "";
            $ext_info = " | 出错文件地址: " . $trace[0]['file'] . $tmp . " | 出错位置在第" . $trace[0]['line'] . "行左右";
        }

        return SeasLog::log($map[$level], $message . $ext_info, array(), $level) ? true : false;
    }

    /**
     * 一般日志函数
     * @param $message
     * @param string $model
     * @return bool
     */
    public static function log($message, $model)
    {
        if (empty($model)) {
            return false;
        }
        return SeasLog::log(SEASLOG_INFO, $message . '===> Ip :' . $_SERVER['REMOTE_ADDR'], array(), '/default/' . $model) ? true : false;
    }

    /**
     * debug日志函数
     * @param $message
     * @return bool
     */
    public static function debugLog($message)
    {
        if (!SeasLog::log(SEASLOG_DEBUG, $message, array(), 'default/debug')) {
            return false;
        }
        return true;
    }
}
```