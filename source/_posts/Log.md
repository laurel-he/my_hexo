---
title: Laravel 日志系统
date: 2019-04-11 15:23:44
tags:
categories: 
- 服务端语言
- php
- laravel
---
## 1 （自己摸索版本）5.2
#### 1 定义路径
.env配置文件添加日志路径，例如
``` 
LOG_PATH=/tmp/paytest
```
#### 2 启动文件添加日志路径
```php
$app->configureMonologUsing(function($logger){
    $logger->pushHandler(new \Monolog\Handler\RotatingFileHandler(env('LOG_PATH').'.log'), 10);
    return $logger;
});
```
## 2 官方文档
#### 1 异常处理
**介绍**
当您启动一个新的 Laravel 项目时，错误和异常处理就已为您配置。 应用程序触发的所有异常都被 App\Exceptions\Handler 类记录下来，然后渲染给用户。 我们将在本文档中深入介绍此类。
**配置**
（1）config/app.php 文件的 debug 选项，决定了是否向用户显示错误信息。默认情况下，此选项设置为存储在 .env 文件中的  APP_DEBUG 环境变量中。
（2）Laravel 支持 single 、daily 、 syslog 和 errorlog 日志模式。要配置 Laravel 使用的存储机制，应该修改 config/app.php 配置文件中的 log 选项。例如，如果您希望使用每日一个日志文件而不是单个文件，则应将 app 配置文件中的 log 值设置为 daily
**日志等级**
使用 Monolog 时，日志消息可能具有不同的日志等级。默认情况下，Laravel 将所有日志级别写入存储。但是，在生产环境中，您可能希望通过将 log_level 选项添加到 app.php 配置文件中来配置应记录的最低日志等级。

一旦配置了此选项，Laravel 将记录大于或等于指定日志等级的所有级别。例如，默认将 log_level 设置为 error 那么将会记录 error , critical , alert 和 emergency 日志信息：
``` 
'log_level' => env('APP_LOG_LEVEL', 'error'),
```
**注意事项**
 Monolog 识别以下日志等级 - 从低到高为: debug , info , notice , warning , error , critical , alert , emergency
**Report方法**
所有异常都由 App\Exceptions\Handler 类处理。 这个类包含两个方法：report 和 render 。 我们将详细研究这些方法。 report 方法用于记录异常或将其发送到外部服务，如 Bugsnag 或 Sentry 。默认情况下，report 方法只是将异常传递给记录异常的基类。然而，你可以自由选择任何方式进行处理。
**render方法**
render 方法负责将异常转换成 HTTP 响应发送给浏览器。默认情况下，异常会传递给为您生成响应的基类。但是，您可以自由检查异常类型或返回您自己的自定义响应
``` 
/**
 * 渲染异常并添加到 HTTP 响应中。
 *
 * @param  \Illuminate\Http\Request  $request
 * @param  \Exception  $exception
 * @return \Illuminate\Http\Response
 */
public function render($request, Exception $exception)
{
    if ($exception instanceof CustomException) {
        return response()->view('errors.custom', [], 500);
    }

    return parent::render($request, $exception);
}
```
**HTTP异常**
可直接用abort()，例如abort(404),abort(403, 'Unauthorized action.');
**自定义错误页面**
如果您要自定义404 HTTP状态代码的错误页面，请创建一个 resources/views/errors/404.blade.php 。此文件将会用于渲染所有404错误。此目录中的视图文件命名应与它们对应的HTTP状态代码匹配。由 abort 函数引发的 HttpException 实例将作为 $exception 变量传递给视图。
#### 2 记录
**介绍**
默认情况下，Laravel 日志目录为 storage/logs
可以传递参数，例如：
``` 
$test = "test";
Log::info(222222222, ['te' => $test]);
```



