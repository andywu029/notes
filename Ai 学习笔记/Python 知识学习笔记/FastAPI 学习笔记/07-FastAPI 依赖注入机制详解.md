&emsp;&emsp;依赖注入（Dependency Injection，DI）是编程模式中的一种依赖倒置原则的范式应用实现。在复杂的软件工程中，软件都是由各种不同的程序模块组成的，各模块之间相互依赖。在依赖倒置原则中，依赖注入对设计模式提出了设计规范的建议，如上层模块不应该依赖于底层模块，应该依赖于抽象，而抽象不应该依赖细节，细节本身应该依赖于抽象。基于上述原则引申出新设计模式——控制反转，这种设计模式引入 IOC 容器的概念，容器用于管理其他依赖对象所依赖的外部资源对象。

&emsp;&emsp;依赖注入是控制反转的具体应用实现。它的本质是对应用中的各个模块、类或组件等进行解耦分离。模块、类或组件既可以具有不同的配置，也可以具有不同的作用域对象，这样可以保持组件之间的松耦合度。各组件可以灵活地根据实际需求进行依赖组合，让模块或类的设计具有更高的灵活性。

&emsp;&emsp;从某种程度上看，控制反转可从容器方面为其他对象提供需要调用的外部资源。依赖注入则强调要创建的对象依赖于 IOC 容器对外提供的资源对象。在依赖注入中，对象构建和对象注入是相互分离的。简而言之，对于依赖注入，可以理解为，是一种能让一个对象接收来自其所依赖的其他对象的一种模式，是一种能通过某种依赖机制自动对依赖对象进行依赖处理，并对依赖对象执行具体实例化的操作。

&emsp;&emsp;依赖注入中涉及4个比较关键的概念：

+ <font color=orchid>**服务：**</font> 它可以是一个类，也可以是一个函数，主要负责提供具体功能的实现
+ <font color=orchid>**客户：**</font> 它是负责对服务进行使用的对象
+ <font color=orchid>**接口：**</font> 它负责连接客户和服务。因为客户不需要对服务内部的实现细节进行了解，只需要指定具体的服务对象名称即可进行关联
+ <font color=orchid>**注入器：**</font> 它主要负责在客户中引入具体的服务实例

&emsp;&emsp;FastAPI 框架中存在一个依赖树机制，依赖树在某种程度上扮演了整个 IOC 容器的角色，它可自动解析并处理每层中所有依赖项的注册和执行。依赖注入的实现其实是FastAPI框架的另一个特性，它在框架中的应用场景主要有：

+ <font color=orchid>**业务逻辑共享：**</font> 使用依赖注入机制来统一定义业务逻辑处理，可以避免在每个函数中重复创建，快速共享一段业务逻辑处理
+ <font color=orchid>**数据库连接：**</font> 使用依赖注入机制来管理数据库连接，可以避免在每个函数中重复创建连接，共享同一个上下文中的连接对象
+ <font color=orchid>**认证和授权：**</font> 使用依赖注入机制来管理用户认证和授权，可以避免在每个函数中重复编写认证和授权代码
+ <font color=orchid>**缓存管理：**</font> 使用依赖注入机制来管理缓存，可以避免在每个函数中重复编写缓存管理代码
+ <font color=orchid>**外部API调用：**</font> 使用依赖注入机制来管理外部API调用，可以避免在每个函数中重复编写API调用代码
+ <font color=orchid>**参数校验和转换：**</font> 使用依赖注入机制来管理参数校验和转换，可以避免在每个函数中重复编写参数校验和转换代码

# 依赖注入框架

&emsp;&emsp;Python 中目前有比较多的开源依赖注入框架，如：`python-dependency-injector`（GitHub地址：https://github.com/ets-labs/python-dependency-injector）​、`injector`（GitHub地址：https://github.com/alecthomas/injector）等。下面基于  `python-dependency-injector` 官方提供的示例来讲解依赖注入框架的使用，该库是基于 `BSD-3-Clause license` 开源协议方式发布的。首先需要安装 `python-dependency-injector` 库：

```shell
pip install dependency-injector
```

&emsp;&emsp;使用依赖注入之前的写法：

```python
import os  
  
  
class ApiClient:  
    def __init__(self) -> None:  
        self.api_key = os.getenv('API_KEY')  
        self.timeout = int(os.getenv('TIMEOUT'))  
  
  
class Service:  
    def __init__(self) -> None:  
        self.api_client = ApiClient()  
  
  
def main() -> None:  
    service = Service()  
  
  
if __name__ == '__main__':  
    main()
```

&emsp;&emsp;依赖注入的写法（通过函数传参方式）：

```python
import os  
  
  
class ApiClient:  
    def __init__(self, api_key: str, timeout: int) -> None:  
        self.api_key = api_key  
        self.timeout = timeout  
  
  
class Service:  
    def __init__(self, api_client: ApiClient) -> None:  
        self.api_client = api_client  
  
  
def main(service: Service) -> None:  
    pass  
  
  
if __name__ == '__main__':  
    main(  
        service=Service(  
            api_client=ApiClient(  
                api_key=os.getenv("API_KEY"),  
                timeout=int(os.getenv("TIMEOUT"))  
            )  
        )  
    )
```

&emsp;&emsp;但是这种方式实现的话，函数调用时需要创建很多对象，不便于管理和维护。这个时候就需要依赖注入器（dependency_injector 框架）来帮助组建对象和函数：

```python
import os  
from dependency_injector import containers, providers 
from dependency_injector.wiring import Provide, inject  
  
  
class ApiClient:  
    def __init__(self, api_key: str, timeout: int) -> None:  
        self.api_key = api_key  
        self.timeout = timeout  
  
  
class Service:  
    def __init__(self, api_client: ApiClient) -> None:  
        self.api_client = api_client  
  
  
class Container(containers.DeclarativeContainer):  
    config = providers.Configuration()  
  
    app_client = providers.Singleton(  
        ApiClient,  
        api_key=config.api_key,  
        timeout=config.timeout,  
    )  
  
    service = providers.Factory(  
        Service,  
        api_client=app_client,  
    )  
  
  
@inject  
def main(service: Service=Provide[Container.service]) -> None:  
    pass  
  
  
if __name__ == '__main__':  
    container = Container()  
    container.config.api_key.from_env("API_KEY", default='api_key', required=True)  
    container.config.timeout.from_env("TIMEOUT", as_=int, default=5)  
    container.wire(modules=[__name__])  
  
    main()  
  
    with container.api_client.override(mock.Mock()):  
        main()
```

&emsp;&emsp;这样实现有几个好处：

+ 依赖自动组装和注入
+ 可以通过 `container.api_client.override(mock.Mock())`  使用测试函数
+ 可以方便配置多套不同的开发环境的依赖注入
+ 函数的结构更加显示，容易看出入参的结构

&emsp;&emsp;`dependency_injector` 框架主要构成：

| 工具集           | 提供的方法/类                                                                                                                     | 备注                              |
| ------------- | --------------------------------------------------------------------------------------------------------------------------- | ------------------------------- |
| Providers     | **`Factory`, `Singleton`**, `Callable`, `Coroutine`, `Object`, `List`, `Dict`, `Configuration`, `Resource`, `Dependency等方法` | 1. 帮助组装对象;<br>2. 方便Overriding覆盖 |
| Overriding    | container.api_client_factory.override<br>container.api_client_factory.reset_override                                        | 覆盖对象改变注入                        |
| Configuration | 提供了从多种源读取配置：yaml、ini、json、env等                                                                                              |                                 |
| Resource      | 提供一种类似Singleton的资源加载方式，提供初始化，和卸载资源方法。                                                                                       |                                 |
| container     | 容器，里面定义了多个实例                                                                                                                |                                 |
| Wiring        | 提供@inject装饰器，指示需要被装饰的函数                                                                                                     |                                 |

# 依赖项及其声明方式

&emsp;&emsp;在 FastAPI 框架中，依赖项是一种可注入的组件，用于为路径操作函数和其他依赖项提供所需资源。依赖项可以是函数（包括同步函数或协程函数）​、类或任何实现了_ `_call__()` 方法的对象。

&emsp;&emsp;在 FastAPI 框架中有两种类型的依赖项：<font color=red>路径操作函数依赖项和全局依赖项</font>。路径操作函数依赖项是依赖单个路径操作函数的函数，而全局依赖项则是整个应用程序共享的依赖项。在 FastAPI 框架中，依赖注入机制通过声明函数参数来实现。依赖项可以在路径操作函数中使用，通过 Depends 函数来注入。FastAPI 框架会在调用路径操作函数之前解析所有依赖项，并将它们传递给路径操作函数。如果依赖项之间有依赖关系，则 FastAPI 框架将按照正确的顺序解析它们，并确保每个依赖项只被解析一次（也可以取消缓存机制，强制每次都重新解析）​。如果依赖关系不存在，则会引发异常。所以有多种声明依赖项的方式，具体如下：

+ 函数式依赖项
+ 类方式依赖项
+ yield 生成器方式依赖项

&emsp;&emsp;依赖项需要一个可调用对象，所以它也有多种实现方式。

## 函数式依赖项

```python
from fastapi import FastAPI, Request  
from fastapi import Query, Depends  
from fastapi.exceptions import HTTPException  
  
app = FastAPI()  
  
  
def username_check(username: str= Query(...)):  
    if username != 'zhong':  
        raise HTTPException(status_code=403, detail="没有权限访问")  
    return username  
  
  
@app.get("/user/login/")  
def user_login(username: str=Depends(username_check)):  
    return username  
  
@app.get("/user/info")  
def user_info(username: str=Depends(username_check)):  
    return username
```

+ <font color=orchid>**定义函数式依赖项：**</font> 导入相关包之后，定义了一个同步函数 `username_check()`，这个函数中定义了一个需要传入名为 `username` 的查询参数，`username` 参数遵循之前所介绍的相关校验规则，所以这里增加相关字段参数的验证逻辑。此时 `username_check()` 这个同步函数就可以称为一个依赖项。在这个函数依赖项的内部通过获取传入的查询参数值，对 `username` 做业务逻辑判断，即如果用户名不是 `zhong`，则抛出 `HTTPException` 无访问权限异常，否则直接返回传入 `username` 参数的值。

> <font color=orange>**注意：**</font>函数类型的依赖项可以是同步函数，也可以是协程函数，FastAPI 框架会自动进行转换处理。

+ <font color=orchid>**把函数依赖项注入路径操作函数中：**</font> 在处理完依赖项的声明之后，通过 `Depends()` 函数把这个 `username_check()` 依赖项注入两个接口的路径操作函数中。`Depends()` 的使用方式与 `Body`、`Query` 等相同，只是内部逻辑的处理方式不一样

&emsp;&emsp;通过上面的示例可知，使用依赖注入的方式可以达到快速共享一段业务逻辑处理的目的。

## 类方式依赖项

&emsp;&emsp;前面强调 `Depends()` 函数传入的参数必须是一个可调用对象，对于类来说它本身也可以是一个可调用的对象，所以 FastAPI 支持使用类的方式来定义依赖项：

```python
from fastapi import FastAPI, Request  
from fastapi import Query, Depends  
from fastapi.exceptions import HTTPException  
  
app = FastAPI()  
  
  
class UsernameCheck:  
    def __init__(self, username:str=Query(...)):  
        if username != 'zhong':  
            raise HTTPException(status_code=403, detail="没有权限访问")  
        self.username = username  
  
  
@app.get("/user/login/")  
def user_login(username: UsernameCheck = Depends(UsernameCheck)):  
    return username  
  
  
@app.get("/user/info")  
def user_info(username: UsernameCheck = Depends(UsernameCheck)):  
    return username
```

+ <font color=orchid>**定义类方式的依赖项：**</font> 这里把函数式依赖项变为使用类的方式（即UsernameCheck）来定义，该类所需的参数和函数式所需的参数是一样的，因为在依赖项内部实现的逻辑一样。在类的初始化过程中，需要接收 `username` 查询参数，如果 `username` 参数值不为 `zhong`，就会抛出 `HTTPException` 无访问权限异常，否则会直接把当前传入的 `username` 参数值赋给类中的 `username` 属性
+ <font color=orchid>**把类作为依赖项注入路径操作函数中：**</font> 在完成 `UsernameCheck` 类依赖项声明之后，通过 `Depends()` 函数把这个 `UsernameCheck` 类当作依赖项注入定义的两个接口路径操作函数中。

&emsp;&emsp;还可以使用如下简化写法：

```python
@app.get("/user/info")
def user_info(username: UsernameCheck=Depends()):
	return username
```

## 多个依赖项注入和依赖项传参

&emsp;&emsp;相较于函数方式，类方式可以更好地管理内部依赖项以及其他函数或类中的本地变量，甚至可以在类中声明多个依赖项：

```python
from fastapi import FastAPI, Request  
from fastapi import Query, Depends  
from fastapi.exceptions import HTTPException  
  
app = FastAPI()  
  
  
class UsernameCheck:  
    def __init__(self, password: str):  
        pass  
        self.password = password  
  
    def username_form_query(self, username: str=Query(...)):  
        if username != 'zhong':  
            raise HTTPException(status_code=403, detail="没有权限访问")  
        self.username = username  
  
    def username_form_post(self, username: str=Query(...)):  
        if username != 'zhong':  
            raise HTTPException(status_code=403, detail="没有权限访问")  
        self.username = username  
  
upw = UsernameCheck(password='123456')  
  
  
@app.get("/user/login/")  
def user_login(username: UsernameCheck = Depends(upw.username_form_query)):  
    return username  
  
  
@app.get("/user/info")  
def user_info(username: UsernameCheck = Depends(upw.username_form_post)):  
    return username
```

# 多层依赖项嵌套注入

&emsp;&emsp;在复杂的业务场景中，有可能需要处理更多的业务逻辑，比如：A 依赖项会依赖于 B 依赖项的返回值，而 B 依赖项又依赖 C 依赖项的返回值。下面以一个示例来说明：首先定义用户登入接口、用户信息获取接口，两个接口需要仅限用户名为 zhong 的用户进行接口请求；其次两个接口还需要满足“仅限年满18岁的用户才可以进行接口请求”的需求。用户名非 zhong 的用户请求调用接口时会抛出 “用户名错误！没有权限访问！” 的异常；若用户名为 zhong 但是年龄未满18周岁的用户调用接口，则会抛出 “用户未满18岁！禁止吸烟！” 的异常：

```python
from fastapi import FastAPI, Request  
from fastapi import Query, Depends  
from fastapi.exceptions import HTTPException  
from typing import Tuple  
  
app = FastAPI()  
  
  
def username_check(username: str = Query(...)):  
    if username != 'zhong':  
        raise HTTPException(status_code=403, detail="用户名错误！没有权限访问！")  
    return username  
  
  
def age_check(username: str=Depends(username_check), age:int=Query(...)):  
    if age < 18:  
        raise HTTPException(status_code=403, detail="用户未满18岁！禁止吸烟！")  
    return username, age  
  
  
@app.get("/user/info")  
def user_info(username_and_age:Tuple=Depends(age_check)):  
    return {  
        "username": username_and_age[0],  
        "age": username_and_age[1]  
    }
```

+ 定义函数类型的用户检测依赖项 username_check()，该依赖项需要传入 username 查询参数
+ 定义函数类型的年龄检测依赖项 age_check()，该依赖项依赖于 username_check() 依赖项。在 age_check() 函数中，使用 Depends() 注入 username_check() 的依赖项，也就是说，age_check() 依赖于 username_check()，用这种方式来表示依赖项之间的层级关系
+ 把 age_check() 函数依赖项注入路径操作函数。因为 age_check() 函数依赖于 username_check()，所以只需要把 age_check() 函数依赖项注册关联到指定的路径操作函数，即可完成层级类型的依赖项注入

&emsp;&emsp;如果存在多个子依赖项嵌套，那么在默认的情况下不会重复进行子依赖项内部的逻辑处理，而是在处理一次之后把依赖项的返回值直接缓存到内存中。在某些特殊业务场景中，需要获取相关依赖项重新计算后的值，所以需要去除缓存机制：

```python
sername: str = Depends(username_check, use_cache=false)
```

&emsp;&emsp;对 Depends() 提供 `use_cache` 参数，可控制该依赖项处理结果是否写入内存中。

# 多个依赖对象注入

&emsp;&emsp;在多层嵌套依赖注入示例代码中同时定义了两个依赖项，但是在这种定义方式下，两个依赖项之间是存在依赖关系的。如果不需要存在依赖关系，但是又需要同时注入多个依赖项，那么可以使用如下代码：

```python
from fastapi import FastAPI, Request  
from fastapi import Query, Depends  
from fastapi.exceptions import HTTPException  
from typing import Tuple  
  
app = FastAPI()  
  
  
def username_check(username: str = Query(...)):  
    if username != 'zhong':  
        raise HTTPException(status_code=403, detail="用户名错误！没有权限访问！")  
    return username  
  
  
def age_check(age:int=Query(...)):  
    if age < 18:  
        raise HTTPException(status_code=403, detail="用户未满18岁！禁止吸烟！")  
    return age  
  
  
@app.get("/user/info")  
def user_info(username: str = Depends(username_check), age: int = Depends(age_check)):  
    return {  
        "username": username,  
        "age": age  
    }
```

# 不同位置上的依赖项

&emsp;&emsp;依赖项在整个 FastAPI 框架中根据注入位置的不同可分为如下几种依赖项

+ <font color=orchid>**全局依赖项：**</font> 它会自动注册到所有的路由对象里面
+ <font color=orchid>**路径操作依赖项：**</font> 它需要注册到路由装饰器上（也就是路径操作参数中）​
+ <font color=orchid>**路由分组依赖项：**</font> 它是单独针对某一个路由分组的依赖项
+ <font color=orchid>**路径函数依赖项：**</font> 它需要注册到视图函数上

## 全局依赖项的注入

&emsp;&emsp;FastAPI 类提供了一个 `dependencies` 参数，该参数就是实现全局依赖项注入的关键：

```python
from fastapi import FastAPI  
from fastapi import Query, Depends  
from fastapi.exceptions import HTTPException  
  
  
def username_check(username: str = Query(...)):  
    if username != 'zhong':  
        raise HTTPException(status_code=403, detail="用户名错误！没有权限访问！")  
    return username  
  
  
def age_check(age:int=Query(...)):  
    if age < 18:  
        raise HTTPException(status_code=403, detail="用户未满18岁！禁止吸烟！")  
    return age  
  
  
app = FastAPI(dependencies=[Depends(username_check), Depends(age_check)])  
  
  
@app.get("/user/login/")  
def user_login():  
    return {  
        'code': 'ok'  
    }  
  
  
@app.post("/user/info/")  
def user_info():  
    return {  
        'code': 'ok',  
    }  
  
  
from fastapi import APIRouter  
api_router = APIRouter()  
  
  
@api_router.get("/user/apirouter/")  
def user_apirouter():  
    return {  
        'code': 'ok',  
    }
```

&emsp;&emsp;在上述代码中，分别定义了 username_check() 和 age_check() 两个函数式的依赖项。在 FastAPI 实例化时，通过 dependencies 参数，把上面两个函数式的依赖项注入app对象中。这种方式可实现全局依赖项的注入。需注意，通过这种方式注入的依赖项会作用到所有app的路由上，当有请求进来时，所有的路由都会自动执行所有的依赖项。

## 路径操作依赖项的注入

&emsp;&emsp;路径操作依赖项的注入方式和路径操作函数依赖项的注入方式基本是一样的，只不过它们注入的位置和依赖项的返回值是否接收处理不一样。如果依赖项是通过路径操作函数进行依赖注入的，那么依赖项里面的返回值是可以被接收处理的，也就是视图函数内部可以接收对应依赖项的返回值。反之，路径操作的依赖项返回值是不会被接收处理的：

```python
@app.get("/user/login/", dependencies=[Depends(username_check), Depends(age_check)])  
def user_login():  
    return {  
        'code': 'ok'  
    }
```

## 路由分组依赖项的注入

&emsp;&emsp;可以通过 FastAPI 框架提供的 APIRouter 类进行路由分组。通过路由分组可以更好地规划项目中的 API 模块结构，且在特定路由分组上就可进行依赖项的注入：

```python
from fastapi import FastAPI  
from fastapi import Query, Depends  
from fastapi.exceptions import HTTPException  
from fastapi import APIRouter  
  
  
def username_check(username: str = Query(...)):  
    if username != 'zhong':  
        raise HTTPException(status_code=403, detail="用户名错误！没有权限访问！")  
    return username  
  
  
app = FastAPI()  
  
  
user_group_router = APIRouter(prefix="/user", dependencies=[Depends(username_check)])  
  
  
@user_group_router.get("/login")  
def user_login():  
    return {  
        'code': 'ok'  
    }  
  
  
app.include_router(user_group_router)
```

&emsp;&emsp;除了上面的方式，用户还可以通过 `app.include_router()` 函数提供的 `dependencies` 参数实现路由分组对象的依赖项注入：

```python
from fastapi import FastAPI  
from fastapi import Query, Depends  
from fastapi.exceptions import HTTPException  
from fastapi import APIRouter  
  
  
def username_check(username: str = Query(...)):  
    if username != 'zhong':  
        raise HTTPException(status_code=403, detail="用户名错误！没有权限访问！")  
    return username  
  
  
app = FastAPI()  
  
  
user_group_router = APIRouter(prefix="/user")  
  
  
@user_group_router.get("/login")  
def user_login():  
    return {  
        'code': 'ok'  
    }  
  
  
app.include_router(user_group_router, dependencies=[Depends(username_check)])
```


