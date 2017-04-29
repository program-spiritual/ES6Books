# 使用`Stdlib`创建`Node.js`微服务

微服务和无服务器架构都在软件行业大火。 在使用`Polybit`的惊人的[stdlib](https://stdlib.com/)平台后，我清楚地看到了这个有前途的技术的价值！ 今天我来介绍一下`stdlib`。 我建议您与我一起尝试，因为我们利用`stdlib`来构建可以在各种上下文中使用微服务。 让我们开始使用这个神话般的技术！

## 什么是`stdlib`？

Polybit的stdlib是一个`功能`即`服务`（`FaaS`）平台，用于使用`Node.js`创建模块化，可扩展的微服务器。它提供了很多的功能，也很有趣。 🙂

`Stdlib`被认为是"无服务器架构"。头几次我听到"无服务器"的含义，我对这个概念很是不爽。我认为自己是相当精明的，我知道没有像没有服务器的云架构那样的事情。或者,我错过了一些东西吗？为了将消息传播给每个人（为自己争取一个...），无服务器架构实际上使用服务器。我们将这些架构称为"无服务器"，因为它们运行在我们不拥有基础设施的系统上，因此我们可以侧重于完成编写代码工作。我们不必强调操作系统补丁，甚至担心应用程序基础架构是最新的。这确实是一个巨大的优势和生产力增强！

如`stdlib`文档中所述：

`stdlib`由三个组件组成：

- 微服务的中心注册表
- 大规模托管的分销平台
- 用于包管理和服务创建的开发框架

它是您自己或与团队开始构建微服务的最快，最简单的方法，目前支持`Node.js 6.9.1`。 用于执行服务建立在`AWS Lambda`的基础之上分发和托管平台，既保证了生产就绪服务所需的规模和可靠性。

顺便说一下，主`stdlib`文档页面写得很好，并提供了一个很好的`stdlib`介绍。在阅读本指南之前或之后，您应考虑阅读本文档以加强概念。 我的指南旨在扩展主要的`stdlib`文档，并通过创建一个现实的微服务，超越了我们都知道和喜欢的必要的"Hello World"应用程序。 🙂

## 为`stdlib`铺路

好的，足够的背景信息。 让我们继续并在云端创建一个,令人敬畏,可以使用和分享给我们的家人，朋友和同的微服务器。 好的，开始了！

第一步，从[这里](https://nodejs.org/en/download/)安装`Node.js`，如果您还没有安装。 您将需要运行`Nodev6.x` 或更高版本才能进入游戏。 如果您希望在`Raspberry Pi`上运行，请按照我的"新手指南"中的"在Raspberry Pi上安装`Node.js`"中的步骤进行操作。

接下来，从终端输入以下内容，在系统上全局安装一个简洁而简明的`lib.cli`包：

```
$ npm install -g lib.cli
```

很好 - 我们准备建立我们的第一个服务。

## 我们的微型服务 - 使用`GPS`坐标找到城市

让我们跳过"hello World"之外的几步之外，在`stdlib`中建立一个微服务器，在提供`GPS`坐标时告诉我们城市的名称。 我向所有的人，我的朋友，在其他国家提前道歉，因为我们只能在美国找到城市。 也许您可以使用这些概念，并在世界其他地方找到城市。😊

首先，拿出你的笔和纸（真实的或虚拟的），然后去这个[`GPS`坐标网站](http://www.gps-coordinates.net/)，在美国选择一个地方来测试你的（还有待写的）微型服务，并记下GPS坐标。 圣地亚哥靠近我的中心，，所以我将为我的微服务使用以下GPS坐标：

- 纬度：`32.72`
- 经度：`-117.16`

请注意，我将`GPS`坐标四舍五入为两位十进制数字。 这个精度将超过给定GPS坐标的城市定位。

好的，花一点时间来梦想，想想你将要建立的这个很棒微服务器的未来效用。 您将在美国的道路上行驶，并使用您的移动设备，您可以调用`stdlib`云中的微型服务，并在当前位置找到城市的名称！ 您甚至可能在圣地亚哥沿海滩的敞篷车驾驶。 很棒的梦想！ 让我们过渡到现实，让这个微型服务继续。 一语双关？ 🙂

## 从URL调用微服务器

最终的游戏是，我们要创建一个可以通过多种方式调用的微服务，包括从URL（HTTP GET）。我已经发布了我们将要建立的微服务器的样本。该网址将类似于此：

```
https://f.stdlib.com/thisdavej/gps/findcity?lat=32.72&lon=-117.16
```

现在开始运行微服务 URL（<https://f.stdlib.com/thisdavej/gps/findcity?lat=32.72&lon=-117.16），因为它已经部署在`stdlib`云中。您应该看到以下结果：>

```
{
  "zipcode": "92101",
  "state_abbr": "CA",
  "latitude": "32.719601",
  "longitude": "-117.16246",
  "city": "San Diego",
  "state": "California",
  "distance": 0.2343729936809432
}
```

随意调整URL参数（`lat`和`lon`），看看结果如何变化。 我们来看看这个`stdlib` URL的各个组成部分：

> <https://f.stdlib.com/>

> <username>/gps/findcity?lat=32.72&amp;lon=-117.16</username>

- `f.stdlib.com` - 这是`stdlib`功能即服务平台的主机名，所有这些都是魔术。
- <username> - <code>stdlib</code>用户名，这是我的<code>casedavej</code>，并将构建您的微服务后，您的用户名。</username>

- `gps` - 微服务器的名称。 您可以将其视为各种功能的命名空间。

- `findcity` - 在微服务器中创建的函数的名称
- `lat/lon` - 提供给函数的URL参数。

作为一个注解，我的微服务（以及您的构建之后）也可以通过以下形式的`HTTP GET`进行调用：

> <https://f.stdlib.com/thisdavej/gps?lat=32.72&lon=-117.16>

您将注意到，我们可以省略`findcity`，因为我们将配置`findcity`作为`gps microservice`后面调用的默认函数，如果没有提供函数名称。虽然这是一个个人的决定，我更喜欢使用完整的网址<https://f.stdlib.com/thisdavej/gps/findcity?lat=32.72&lon=-117.16，因为> `gps/findcity`感觉更自我记录，我有在`gps`命名空间下自由添加其他相关功能。如果我们没有计划在`gps`下创建其他功能，我们可以简单地创建一个`findcity`服务并创建一个名为`main`（或任何其他名称）的默认函数。

## 从命令行调用微服务器

`Polybit`的`stdlib`的另一个很酷的功能是我们也可以使用`lib`命令从终端调用微服务器。 `lib`命令作为上面已经完成的l`ib.cli`全局安装的一部分进行安装。让我们继续使用`lib`来调用`microservice`：

```
$ lib thisdavej.gps.findcity --lat 32.72 --lon -117.16
```

果然你应该看到一个返回的`JSON`对象，其中包含`“San Diego，CA”`数据。 与URL类似，可以调用`lib`，而不必具体包含`findcity`的默认函数名称，如下所示：

```
$ lib thisdavej.gps --lat 32.72 --lon -117.16
```

我也使用了一些特别的技巧（下面进一步解释），所以我们可以通过`lib`来调用我们的微服务，而不需要这样指定`--lat`和`--lon`：

```
$ lib thisdavej.gps.findcity 32.72 -117.16
```

我们将在稍后讨论这件事情，但是我希望你能在前面看到它。 现在我们已经看到了最终的目标，跟随我，所以你可以创建你的第一个微服务.

## 初始化您的工作区

首先，我们为`stdlib`文件创建一个目录并切换到这个目录：

```
$ mkdir stdlib-workspace
$ cd stdlib-workspace
```

自从我们全局安装了`lib npm`软件包以来，我们系统上的`lib`命令将被证明是我们最好的朋友，因为我们创建了我们的微服务器。我们现在开始使用它来初始化我们的工作区：

```
$ lib init
```

`init`命令将提示您输入与`stdlib`帐户相关联的电子邮件地址。假设您没有`stdlib`帐户，您将有机会创建一个具有关联的电子邮件地址和密码的帐户。您将需要一个帐户并需要登录，以便您最终将代码推送到`stdlib`云并运行您的微服务器。

如果您目前不想创建`stdlib`帐户，可以运行`lib init --no-login`;但是，您将处于离线状态，无法将服务部署到`stdlib`云端。这不是推荐的路由，因为如果在云中无法访问，创建微服务没有太多的好处。 🙂如果您在此脱机模式下启动，则当您准备`push`更改（稍后描述）时，最终需要运行`lib login`命令。作为登录过程的一部分，如果您还没有登录过程，您将有机会创建一个`stdlib`帐户。

## 创建您的服务

要创建您的第一个服务，请输入以下命令：

```
$ lib create
```

出现提示时输入以下信息：

```
Service Name: gps
Default Function Name: findcity
```

接着！您已成功配置您的微服务器的准系统结构。我们现在来看看这个结构。

## 简要介绍`stdlib`服务目录结构

导航到您新创建的`gps`服务的目录（用`<username>`替换上面创建的`stdlib`用户名）

在`gps`目录中，可以找到以下结构：

```
- f/
  - gps/
    - function.json
    - index.js
- package.json
- env.json
- README.md
```

以下是这些组件的一些亮点：

> package.json

此文件使用`npm package.json`格式，因此您可能会很熟悉：

```
{
  "name": "gps",
  "version": "0.0.0",
  "description": "GPS Service - Functions to return results based on GPS coordinates",
  "author": "thisdavej <thisdavej@gmail.com>",
  "main": "f/findcity/index.js",
  "dependencies": {},
  "private": true,
  "stdlib": {
    "name": "thisdavej/gps",
    "defaultFunction": "findcity",
    "timeout": 10000,
    "publish": false
  }
}
```

大部分信息是为您自动生成的。关键领域包括：

- `name` - 您的服务将被发布到的名称。通常格式为`<username>/`。
- `description` - 您的服务描述。当或者如果您将服务发布到`stdlib`中心注册表（暂时描述），则会显示该描述。我将第4行（`“description”`）从默认值`“Service”`更改为基于我们正在开发的服务的更详细的描述。
- `defaultFunction` - 如果没有提供功能，则您的服务的默认入口点。服务由`<username>/ <service>/<function>`访问。我们的默认功能是`findcity`，所以`<username>/gps`将映射到`<username>/gps/findcity`。
- `publish` - 如果您希望将服务发布到`stdlib`中央注册表，请将此值设置为`true`。 （我们还没有准备好。）将服务推送到`stdlib`云后，您将可以立即使用它;然而，将`publish`设置为`true`将使您的服务可以被其他人查看，并可从`stdlib`搜索页访问。

> env.json

默认`env.json`看起来像这样：

```
{
  "dev": {
    "key": "value"
  },
  "release": {
    "key": "value"
  }
}
```

此文件包含要发送到`process.env`变量的环境变量。 值如下使用：

- "dev" - 当您在本地工作或在`stdlib staging`（dev）环境中工作时使用
- "release" - 当您将发行版推送到`stdlib`时使用

> function.json

该文件位于文件夹`f`文件夹下，名为`findcity`，它是我们的默认功能：

```
{
  "name": "findcity",
  "description": "Function",
  "args": [
    "First argument",
    "Second argument"
  ],
  "kwargs": {
    "alpha": "Keyword argument alpha",
    "beta": "Keyword argument beta"
  },
  "http": {
    "headers": {}
  }
}
```

以下是有关字段的注释：

- "name"：函数名。 必须匹配`f/<function_path>`，否则注册表将抛出错误。
- "description"：功能的简短说明。
- "args"：包含有关函数期望的参数的信息的数组。
- "kwargs"：包含有关函数期望的关键字参数的信息的对象（键值对）。

> index.js

与我们的`function.json`文件相关联的代码驻留在`index.js`文件中。以下是默认内容（无意见）：

```
module.exports = (params, callback) => {
  callback(null, 'hello world');
};
```

## 安装我们服务所需的npm软件包

我们现在准备超越准系统结构并创建我们的服务。 如果还没有，请转到新创建的`gp`s服务的目录（用`<username>`替换上面创建的`stdlib`用户名）：

```
$ cd <username>/gps
```

第一步，我们从`Steven Lu`安装优秀的[城市npm包](https://www.npmjs.com/package/cities)，帮助我们完成基于`GPS`坐标获取城市名称的目标。

```
$ npm install --save cities
```

我们利用`npm --save`参数，以便在`package.json`文件中包含`cities`作为依赖关系。当我们将服务部署到云端时，这将是必需的。

## 写功能码

接下来，我们来编写我们函数的代码。打开`gps/f/findcity/index.js`文件，然后修改如下

```
const cities = require('cities');
module.exports = (params, cb) => {
  const lat = !isNaN(params.args[0]) ? params.args[0] : params.kwargs.lat;
  const lon = !isNaN(params.args[1]) ? params.args[1] : params.kwargs.lon;
  let format = params.kwargs.fmt;
  if (typeof format !== 'undefined')
    format = format.trim().toLowerCase();
  if (lat === undefined || lon === undefined) {
    let message;
    if (lat === undefined && lon == undefined)
      message = 'Need to specify values for lat and lon';
    else if (lat == undefined)
      message = 'Need to specify values for lat';
    else
      message = 'Need to specify values for lon';
    cb(message);
    return;
  }
  const info = cities.gps_lookup(lat, lon);
  if (info.distance > 75) {
    cb('City lookup limited to GPS coordinates in the United States');
  } else {
    let result;
    if (format === 'simple')
      result = `${info.city}, ${info.state_abbr} ${info.zipcode}`;
    else
      result = info;
    cb(null, result);
  }
};
```

在第`1`行中，我们加载了城市模块，使其可用于我们的功能。

在第`3`行中，我们导出使用由以下参数组成的`stdlib`函数格式的函数：

`params` ：一个包含传递给函数的参数（`params.args`）和关键字参数（`params.kwargs`）的对象。

`callback` ：结束函数执行的回调，期望一个错误参数（如果没有错误则返回`null`）和`JSON`序列化结果（或用于文件处理的缓冲区）。

在第`5`行中，我们利用一个特殊的技巧来从我们的函数中获取`lat（纬度）`的值。 回想一下，可以通过两种不同的方式从命令行调用我们的服务：

```
$ lib thisdavej.gps --lat 32.72 --lon -117.16
$ lib thisdavej.gps.findcity 32.72 -117.16
```

在第一个命令中，将一个纬度值提供给`params.kwargs.lat`（一个关键字参数）。 第二个命令为纬度的`params.arg[0]`提供一个值。 经度以类似的方式工作。

试想一下如何通过URL调用微服务器：

<https://f.stdlib.com/thisdavej/gps/findcity?lat=32.72&lon=-117.16>

通过`HTTP GET`访问服务时，所有查询参数都将转换为关键字参数。 这里，`lat`URL参数为`params.kwargs.lat`关键字参数提供一个值。 `args`数组不能通过URL参数使用，因此我们使用第5行中的以下代码从`kwargs`（关键字参数）中检索`lat`的值，因为在通过`HTTP GET`调用时，它在args数组上不可用。

```
const lat = !isNaN(params.args[0]) ? params.args[0] : params.kwargs.lat;
```

我不建议您定期使用此技巧从`args`参数中检索一个值，如果`args`参数不可用，则返回到`kwargs`参数。 如果您正在执行HTTP GET命令，可能更容易使用`kwargs`参数。 我主要是在这里使用它来帮助教你参形参与`kwargs`参数的细微差别。

在第8行中，我们寻找一个名为`fmt`的`kwargs`参数。 我们最后在函数中最后使用这个函数来确定调用函数时返回的数据。

在第12行中，我们确认我们收到纬度和纬度的值。 如果没有，我们调用回调（`callback`）来提供错误。

在第24行中，我们调用了`cities.gps_lookup`函数，根据GPS坐标找到城市。 当调用此函数时，它将返回具有以下结构的`JSON`对象（假设`fmt kwargs`参数不等于`simple`）：

```
{
    "zipcode": "92101",
    "state_abbr": "CA",
    "latitude": "32.719601",
    "longitude": "-117.16246",
    "city": "San Diego",
    "state": "California",
    "distance": 0.2343729936809432
}
```

如图所示，返回的对象包括一个距离值，该值告诉我们所提供的GPS坐标距离最近的城市的中心有多远。 因此，我们可以使用第`14`行中的这一信息作为现实检查，以确保我们的城市在美国境内。

但是，如果我们将`fmt`的值设置为简单，并通过URL调用该服务，例如<https://f.stdlib.com/thisdavej/gps/findcity?lat=32.72&lon=-117.16&fmt=> 简单来说，将返回以下数据：

```
"San Diego, CA 92101"
```

## 修改`function.json`

接下来，我们修改我们的服务的`function.json`：

```
{
  "name": "findcity",
  "description": "Find city based on GPS Coordinates",
  "kwargs": {
    "lat": "Latitude",
    "lon": "Longitude",
    "fmt": "Format of the data to return. If fmt='simple', just the city, state, and zip code are returned."
  },
  "http": {
    "headers": {}
  }
}
```

为了向未来的消费者提供我们功能的文档，我们添加一个描述并指定有关我们的`kwargs`（关键字参数）的信息。 我们删除`args`部分，因为我们主要使用关键字参数，并且不想通过列出我们的纬度和经度两次参数来混淆试听。

## 在本地调用我们的服务

我们准备在本地调用我们的服务，所以我们可以在将其推送到`stdlib`云之前对其进行测试和优化。 如果您还没有，请转到您的服务目录：

```
$ cd <username>/gps
```

我们可以调用我们的函数（`index.js`），如下所示：

```
$ lib .
```

由于我们不提供纬度和经度的值，所以我们应该看到一个"需要指定`lat`和`lon`值"的错误消息。 完善！ 看起来我们的错误检查代码正在运行。

现在，让我们通过args数组来提供纬度和经度值：

```
$ lib . 32.72 -117.16
```

您应该看到与"San Diego，CA"数据相同的JSON对象。 我们还可以通过包含fmt关键字参数来调用我们的函数来返回一个更简单的数据结构。

```
$ lib . --lat 32.72 --lon -117.16 --fmt simple
```

您应该看到文本"San Diego，CA 92101"返回而不是更全面的JSON对象。 上面通过lib调用我们的服务使用我们的默认功能。我们也可以通过调用lib来实现相同的结果：

```
$ lib .findcity --lat 32.72 --lon -117.16
```

## 在登录云时注册我们的服务

我们可以注册我们的服务，以在分段（dev）云或生产（发布）云中运行。 分段环境是可变的，可以被覆盖。 释放是不可变的，不能被覆盖; 但是，它们可以被删除。

要在登台（dev）云中注册我们的服务，请从我们的服务目录（gps）发出以下命令：

```
$ lib up dev
```

我们给它一个考验！首先，让我们通过使用lib的命令行调用我们的基于云的服务，其中

<username>是你的用户名</username>

```
$ lib <username>.gps[@dev].findcity 32.72 -117.16
```

请注意，我们包括`[@dev]`在分段云中调用我们的服务。 您应该看到我们返回的JSON对象结果相同。

接下来，我们使用以下URL从网络调用我们的服务：

> <https://f.stdlib.com/>

> <username>/gps@dev/findcity?lat=32.72&amp;lon=-117.16</username>

注意：分段的dev名称是任意的。 您可以使用其他名称（例如`test`）创建不同的分段环境，前提是在调用时将`@test`添加到服务名称中。

## 在生产云中注册我们的服务

要在生产中注册我们的服务，请从服务目录（gps）发出以下命令：

```
$ lib release
```

让我们看看它在行动！首先，让我们通过命令行使用lib来调用它，其中

<username>是你的用户名：</username>

```
$ lib <username>.gps.findcity 32.72 -117.16
```

接下来，我们可以使用以下URL从网络调用我们的服务：

> <https://f.stdlib.com/>

> <username>/gps/findcity?lat=32.72&amp;lon=-117.16 </username>

我们的微服务正在生产环境中！

## 将我们的服务发布到注册表

对您创建的服务感到满意后，您可以（如果您选择）发布服务，并在[`stdlib`注册表](https://stdlib.com/search)上公开搜索。

## 更新`README.md`文件

第一步，修改位于服务目录根目录中的`README.md`文件中的标记，因为该文件的内容将用于在`stdlib`标记服务的帮助下为您发布的服务提供主要文档， 将`markdown`转换为HTML幕后。

`stdlib markdown`服务支持标准降序和`GFM`（GitHub Flavored Markdown），并在h`ighlight.js`的帮助下支持栅栏代码块的语法突出显示。 您将在这里找到一个可用于语法突出显示代码块的`highlight.js`语言和别名的列表。 栅栏代码块在每个代码块的开始和结束处用三个后跟标记表示，如以下JSON语法示例所示：

```
JSON{
  "zipcode": "92101",
  "state_abbr": "CA",
  "latitude": "32.719601",
  "longitude": "-117.16246",
  "city": "San Diego",
  "state": "California",
  "distance": 0.2343729936809432
}
```

继续更新您的`README.md`文件。以下是gps服务的完整示例，您可以将其用作起点：

````
# GPS Service
This service provides functions to return results based on GPS coordinates.  There is currently one function available:
## findcity - Find the name of city in the USA based on GPS coordinates
### Parameters
#### Keyword arguments
- lat: latitude (the angular distance of a place north or south of the earth's equator)
- lon: longitude (the angular distance of a place east or west of the meridian at Greenwich, England)
- fmt: format of the data to return.  If fmt = `simple`, just the city, state, and zip code are returned
rather than a full JSON object.
### Usage
#### Command line
Return a JSON object with city information based on latitude and longitude
```bash
$ lib thisdavej.gps.findcity --lat 32.72 --lon -117.16
````

This returns the following JSON object:

```json
{
  "zipcode": "92101",
  "state_abbr": "CA",
  "latitude": "32.719601",
  "longitude": "-117.16246",
  "city": "San Diego",
  "state": "California",
  "distance": 0.2343729936809432
}
```

Return a more concise string representing the city:

```bash
$ lib thisdavej.gps.findcity --lat 32.72 --lon -117.16 --fmt simple
```

This returns the following simple string:

```json
"San Diego, CA 92101"
```

### HTTP GET

Return a JSON object with city information based on latitude and longitude

```http
https://f.stdlib.com/thisdavej/gps/findcity?lat=32.72&lon=-117.16
```

### Web and Node.js

```javascript
const f = require('f');
f('thisdavej.gps.findcity')({lat: 32.72, lon: -117.16}, (err, response) => {
  // handle error or response
});
```

### Bugs/limitations

This function is currenly only capable of finding cities in the USA.

## Author

Dave Johnson (<http://thisdavej.com/>) ```

```
##发布到`stdlib`注册表  

非常好 - 您修改了`README.md`文件，为您的服务用户提供文档。我们现在准备发布了！要完成此目标，请修改`package.json`：

- 将“`publish`”设置为`true。`
在“版本”字段中增加`version`号。 这是必要的，因为我们无法覆盖已经在生产中发布的版本。 在为您的服务指定新版本时，请遵循语义版本控制`semvar`指南（`MAJOR.MINOR.PATCH`）。 例如，如果您在微服务器的API表面引入了更改，则会增加主版本。
最后，从终端将新版本的服务发布到生产中：
```

$ lib release

```
## 删除服务

使用以下命令删除`<environment>`将要开发或发布的服务：
```

$ lib down

<environment>
</environment>

```
要删除特定版本，请使用以下命令：
```

$ lib down -r

<version>
</version>

```
您也可以使用` lib rollback`来删除当前指定的版本，如果它是意外发布的。

## 重新启动服务
```

如果停止在云中工作，请使用以下命令语法重新启动您的微服务器：

```

```

lib restart [

<environment>] | [-r <version>]</version></environment>

```
> 笔记：

- `-r`指定释放（生产环境） 非释放（可变）
- 非发布环境（例如，`dev`）不具有与它们相关联的版本。


这里有些例子：
```

$ lib restart dev # restart dev environment (there are no versions associated with dev/staging environents.) $ lib restart -r # restart release envivonment $ lib restart -r 0.0.2 # release release environment 0.0.2

```
这不是必需的，但如果存在任何错误，则该选项存在。

## 重建服务

您也可以重建服务。 这将重新安装软件包依赖关系，并使用最新版本的`stdlib microservice`软件。 这可能会被鼓励，因为`Polybit`推出了旨在提高性能或增强安全性的`stdlib更新`。 以下是重建服务的语法：
```

lib rebuild [

<environment>] | [-r <version>]</version></environment>

```
这里有些例子：
```

$ lib rebuild dev # rebuild dev environment (there are no versions associated with dev/staging environents.) $ lib rebuild -r # rebuild release envivonment $ lib rebuild -r 0.0.2 # rebuild release environment 0.0.2

```
## 创建附加的服务功能

 要为您的服务创建其他端点（函数），请导航到服务目录（用stdlib用户名替换<username>）。例如
```

$ cd

<username>/gps</username>

```
接下来，使用以下命令语法：
```

lib f:create

<function>
</function>

```
要创建一个名为`navigate`的函数，例如，您将调用以下命令：
```

$ lib f:create navigate

```
这将创建一个新的`navigate`功能，您可以修改为世界创建您的下一个令人敬畏的功能。 🙂由于`navigate`不是默认的功能，它将被调用如下：
```

$ lib .navigate ```

您已经配备好并准备创建各种惊人的微服务和功能！

## THE END
