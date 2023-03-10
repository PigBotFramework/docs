# json配置
在第一节中，我们注意到，一个插件文件夹下还会包含两个json文件。分别是`commands.json`和`data.json`
  
## `commands.json`
- 描述：这个json中包含插件所有的指令。  
- 类型：列表套字典（`[{}]`）
- 字典字段：
| 字段名 | 可能的值（类型） | 描述 |
| --- | --- | --- |
| content | ["查 ", "开盒 "] | 指令名称 |
| usage | "查 <@对方>" | 指令用法 |
| promise | "anyone" | 指令权限 |
| eval | "tools.chaQQ(meta_data)" | 要执行的语句 |
| description | "人肉开盒" | 指令描述 |
| mode | "实用工具" | 指令归类 |
| isHide | 1 | 是否隐藏 |
| type | "message" | 该指令类型 |
  
  - `content`具体  
    当用户发送的消息以`content`的内容开头时，会触发该指令
    - 当该指令只有一个触发关键词时，`content`为字符串类型，就是那一个关键词
    - 当该指令有多个关键词都可以触发时，`content`为列表，每一项为一个触发关键词  
    **还需要注意**，触发指令后，猪比会将`self.message`类变量中的开头的`content`去掉。**也就是说`self.message`只包含消息本体，不包含`content`触发关键词**
  
  - `promise`具体
    - "anyone" 所有人可用
    - "admin" 群管可用
    - "ao" 群管和机器人正副主人可用
    - "owner" 机器人正副主人可用
    - "ro" 机器人正主人可用
    - "xzy" 最高管理员可用
  
  - `isHide`具体
    - 0 不隐藏
    - 1 隐藏
    - 2 简略显示
  
  - `type`具体  
    gocq上报有四种类型，分别为：`消息`、`请求`、`通知`、`元事件`，而您的插件可以监听这些上报。举个栗子，如果`type`为`meta_event`则如果上报事件为元事件时会调用`eval`指定的函数；如果`type`为`command`则为普通的监听指令；如果为`message`则任何消息上报会调用指定的函数。以此类推...
    - "message" 监听所有消息上报
    - "request" 监听所有请求上报
    - "notice" 监听所有通知上报
    - "meta_event" 监听所有元事件上报
    - "command" 指令监听
    - "chatterbot" 监听ChatterBot（自定义逻辑）
      - CatterBot监听相当于泛匹配，当消息中包含`content`关键词时，就会触发。[实例](https://github.com/PigBotFrameworkPlugins/basic/blob/main/commands.json#L141)
      
    **请注意：**
    - 由于某些上报事件没有`sender`字段，机器人无法判断是否为管理员还是普通群员。**因此当监听`request`、`notice`、`meta_event`时，请不要使用`ao`和`admin`权限！**您可以设置为`anyone`然后在您的函数内再`CallApi`做判断
    - ~~**请尽量保证一个插件中针对某一种上报监听只包含一个方法。**这样能减少`execPlugin`次数，提高效率。~~ 猪比现在的调用插件使用多线程调用，无需担心会有阻塞
    - **当监听类型不是`command`时，`content`字段请填写监听的名字（事实上不会在任何地方显示）、`usage`字段可以不填写（当然他也不会在任何地方显示），而`isHide`无论填多少该指令（监听）都会在菜单隐藏**
  

## `data.json`
- 描述：该插件的基本信息
- 类型：字典（`{}`）
- 字典字段：
| 字段名 | 可能的值（类型） | 描述 |
| ------ | ---------------- | ---- |
| name | "好感度系统扩展" | 插件名 |
| version | "1.0.1" | 版本号 |
| description | "扩展猪比自带的好感度系统" | 插件描述 |
| author | "xzyStudio" | 插件作者 |
| cost | 0.00 | 插件价格 |
  
  - `cost`插件价格为浮点数，为`0`时则免费
  **以上字段是必须的**
  
| 字段名 | 可能的值（类型） | 描述 |
| ------ | ---------------- | ---- |
| init | "tools.addJob()" | 当机器人处理器的Py进程启动时执行的方法 |
| require | ["test_tick"] | 引入到bot处理器中的函数 |
  
  **以上字段可选，使用方法可见实例TOOLS:**
  - [tools/data.json](https://github.com/PigBotFrameworkPlugins/tools/blob/main/data.json#L7)
  - [tools.addJob()](https://github.com/PigBotFrameworkPlugins/tools/blob/main/main.py#L493)
  - [tools-test_tick()](https://github.com/PigBotFrameworkPlugins/tools/blob/main/main.py#L512)
  - [APScheduler](/api/apscheduler)
  

# md文件
GitHub作为全球最大的同性交友网站，玩过的都知道在一个项目的首页GitHub会默认显示`readme.md`。  
同样的，在机器人实例管理中安装插件时，用户也能看到位于插件根目录下的`readme.md`。  
您可以在`readme.md`中充分介绍您的插件来吸引用户。  
**尽量不要让`readme.md`为空或者不新建`readme.md`！**