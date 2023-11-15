# 本系统xiaoe_learn_little_program 鹅学习小程序

## 目录

* [安装依赖并运行项目](#安装依赖并运行项目)

* [项目配置](#项目配置)

* [小程序发版](#小程序发版)

* [说明](#说明)

* [更新记录](#更新记录)

  

## 安装依赖并运行项目

* 执行命令行`npm run npm:install` (安装依赖，个别uniAPP的依赖cnpm安装会有问题，若进程卡住，请翻墙,若想看原理请查看配置文件`./cnpm.config.js`,后续私有库依赖在`./cnpm.config.js`文件里配置)

* 运行：

  1. 生产环境运行 `npm run build:mp-weixin`  一般执行这个，如果不想影响生产环境数据，可以使用开发环境

  2. 开发环境本地开发运行 `npm run dev:mp-weixin `  运行这个之前，需要先在`manifest`文件中的`mp-weixin`选项中把` appid`改成`wxe3273ced543fd618`

+ 微信开发者工具中打开：

  请选择`dist/dev/mp-weixin`目录打开

  

  

## 小程序发版

不涉及后端改动：

1. 开发完之后，保证`manifest`文件中的`mp-weixin`选项中` appid`为生产环境小程序appid，本地编译出小程序的二维码，给到测试验证

2. 验证通过后合并代码到线上master分支

3. 本地切到master分支，拉取远程master分支代码后，执行`npm run dev:mp-weixin`

4. 微信开发者工具中上传代码

5. 找有权限的人登录微信公众平台设置体验版

6. 找测试验证问题

7. 通过后找小程序管理员提交审核

8. 审核通过后找管理员发布，通知测试验证

9. 现网验证后同步全网信息(版本、更新点)

涉及后端改动：

1. 在开发环境开发联调，自测通过后编辑器编译二维码给测试验证

2. 修改`manifest`文件中的`mp-weixin`选项中` appid`为生产环境小程序appid，修改全局请求，加上准现网appid做灰度，执行`npm run prod:mp-weixin`，微信开发者工具中上传代码，找有权限的人登录微信公众平台设置体验版，等待后端系统更新到准线网环境后，测试验证

3. 验证通过后，将请求中灰度appid去掉，合并代码到线上master分支，执行`npm run prod:mp-weixin`，微信开发者工具中上传代码

4. 找到上面上传的代码，找小程序管理员提交审核

5. 审核通过后找管理员发布，通知测试验证

6. 现网验证后同步全网信息(版本、更新点)

   

## 说明

* 环境appid

  生产环境：`wxa3589f1193170aa3`

  测试环境：`wx9cc0739900b6f8ab`

  开发环境：`wxe3273ced543fd618`

* uni-app写法类似vue，具体区别请看[官方文档](https://uniapp.dcloud.io/frame)。 

* 文件目录：

  ```
      ├─src
          ├─App.vue 应用配置，用来配置App全局样式以及监听 应用生命周期
          ├─main.ts Vue初始化入口文件
          ├─manifest.json 配置应用名称、appid、logo、版本、使用的外部插件等打包信息
          │  ├─xevideo 视频播放插件
          │  ├─xepusher 推流插件，目前已无推流直播环节（可去除）
          ├─pages.json 配置页面路由、导航条、选项卡等页面类信息
          │  ├─page 登录页，首页和个人中心页代码分包
          │  ├─pages 音视频，分享等页面代码分包
          │  ├─e_learn_page 店铺首页和店铺列表页代码分包
          │  ├─verify_page PC端扫码登录代码分包
          ├─common 
          │  ├─constants.js 统一放置系统用到的常量的文件(包括课程类型，报错类型常量等)
          │  ├─config 根据NODE_ENV区分环境，确定配置
          │  ├─css
          |     ├─index.scss 封装的css，可以引入快速使用  包含了flex居中（水平，垂直和水平垂直），padding边距，文字省略等 
          |     ├─var.scss 通用的css变量，包括主题背景色，主题字体等
          │  └─utils 
          │	  ├─sensor_data 埋点相关目录
          │		  ├─init_sensors.ts 埋点方法封装			
          │		  └─sensor_data_constants.ts 各页面通用埋点参数
          │	  ├─api.ts 小程序所有的接口调用
          │	  ├─commom.ts 公共方法封装
          │	  ├─login.ts 登录相关方法封装
          │	  └─reuqest.ts 对请求的二次封装
          ├─global_components 自己封装的全局公共组件                     
          ├─page 该目录主要放置小程序tab栏映射的页面          
          │  ├─ home 店铺首页
          │		  └─index 店铺首页		
          │		  └─business/oldUserBind 绑定升级手机号
          │		  └─components 首页组件
          │		  └─pages/searchPage 首页搜索页面
          │  ├─ contentDownload h5课程详情资料下载中间页（目录路径请勿移动，保持和店铺小程序一致）
          │  ├─ top_bar 首页顶部状态栏       
          │  ├─ user_center 个人中心页
          │		  └─feedback 意见反馈
          │		  └─relatedPhone 绑定手机
          │		  └─userSetting 设置
          │		  └─index 个人中心首页      
          │  └─ webview 公共页面      
          │		  └─customerService 客服组件	
          │		  └─index h5课程详情页面
          │		  └─jumpMiniProgram 跳转小程序处理页面
          │		  └─protocol 协议页面
          ├─pages 分包，在这里放置了content(有音视频详情页),分享页以及落地页（授权页） 在pages.json中的subPackage的第一项的root指定
          │  ├─content 放置音视频详情页相关内容
          │  └─ index 落地页（授权页）
          │		  └─authorizePage 其他小程序跳转课程详情中间页 	
          │		  └─index 登录授权页
          │		  └─navigateBack 进店前h5登录态授权完成后跳转页（路径勿移动，h5_wechat里配置）
          │		  └─sharePage 课程分享页面
          │		  └─other_sharePage 其他分享页面
          ├─verify_page 分包，PC端扫码登录授权页
          │  ├─ error 授权失败页
          │  └─ index 落地页（授权页）
          ├─static 存放应用引用的本地静态资源（如图片、视频等）的目录，注意：静态资源只能存放于此
          ├─env.dev.ts 给开发环境读取的配置
          ├─env.prod.ts 给生产环境读取的配置	
          ├─env.test.ts 给测试环境读取的配置
          └─store vuex 
  ```

## 相关依赖和业务流程图

* 小鹅通小程序登录授权及手机绑定流程整理，请参考(https://www.processon.com/view/link/628a54d06376893bcc106ebb)。 
* 小鹅通小程序进店流程（涉及打通h5登录态部分），请参考(https://www.processon.com/view/link/628a54d06376893bcc106ebb)。

## 定位和排查方式

* 如何变身为学员身份排查看课问题?

1. 通过学员的手机号或者 appid + c_user_id 去找后台(hokit)查询token；

2. 微信开发者工具登录进首页后，替换本地储存的userToken为刚才查到的token，重新刷新页面即可；

3. 若需要手机调试或者PC端微信调试，可以在study_request.ts将config.header.TOKEN设定为固定的token，然后重新编译即可；

## 本地储存的字段和场景周期

* 本地storage
unionId             微信unionId
openId              微信openId
session_key         用于微信授权手机号
userToken           小程序登录态，请求头中会携带用于校验用户信息
XeUserId            小鹅通用户id，请求头中会携带
b_user_id           B端用户id（没有为空）
userPhone           B端用户绑定手机号（没有为空）
nation_code         手机区号
assistant_user_id   用户商家助手经营端的id，用于个人中心绑定手机号
b_account_old       是否B端老账户（没有B端账号不返回）
userInfo            登录用户信息（头像，昵称等）
shopTokens          店铺对应h5登录态（ko_token），进店后请求头中会携带
CURRENT_APP_ID      上次访问过的经营端店铺
video_play_process  视频播放进度记录
common_code         经营端遗留
bingAfterRefreshBusiness  经营端遗留

* vuex
inviting_shop        正在访问的店铺信息
isSharePage          是否从分享的方式进入到页面
isInviting           是否处于店铺内访问阶段（用于店铺内请求头携带ko_token校验）
nextPage 
  ├─isNextPage       登陆后是否存在需要跳转的页面
  ├─nextPageUrl      登录后跳转的页面

## 特殊情况

1. 目前小程序中，支持所有的课程类型，音视频课程（单课+组合课）为原生开发，其他类型课程均为H5页面(训练营pro中的音视频为h5页面)。
   训练营pro中的音视频为h5页面原因：业务侧暂无查询接口，无法调用原生接口查出资源

2. 目前小程序中，打卡，社群和直播（单课+组合课）分别是跳到鹅打卡，鹅社群和鹅直播小程序中浏览。

3. 由于小程序资质问题，电子书目前是通过客服组件下发链接引导到h5观看

4. 课程详情内目前已屏蔽任何营销和支付入口，支付入口替换成客服咨询按钮，通过客服组件下发链接去h5购买（场景：分享出去的课程，其他人没购买，打开显示售前页）

5. 鹅打卡，鹅圈子，鹅直播和企学院的店铺目前不支持进店

6. 分销专栏里面的音视频课程参数需要带上content_app_id（购买渠道：https://admin.xiaoe-tech.com/chosen/content_market#/wareHouse/index）

7. 小程序首页只能查出微信身份订阅的课程和店铺（三端统一项目2022.5.17上线后可查手机号课程）


## 其他涉及的工程及关联点

1. https://talkcheap.xiaoeknow.com/XiaoeFE/h5-resource-detail  大专栏和训练营的支付营销入口的屏蔽以及内容跳转处理（售后）

2. https://talkcheap.xiaoeknow.com/XiaoeFE/h5-shop-fe         专栏和(单课: 图文，音视频，电子书等)的支付营销入口的屏蔽，内容跳转处理（售后）

3. https://talkcheap.xiaoeknow.com/XiaoeFE/h5-goods-fe         课程售前页面入口屏蔽和支付引导（售前）（场景：分享无权益）

4. https://talkcheap.xiaoeknow.com/XiaoeFE/common_h5_jssdk     wx.client环境设置为小鹅通小程序的逻辑

5. https://talkcheap.xiaoeknow.com/XiaoeFE/evaluation_wechat_h5_front   考试测评前端，针对考试页面进行屏蔽

6. venus  分享海报组件（h5工程引入）
  
   goose  屏蔽h5页面悬浮窗还有底部导航栏（h5工程引入）

7. https://talkcheap.xiaoeknow.com/coursefe/uni_e_course  鹅课程，训练营pro工程

8. https://talkcheap.xiaoeknow.com/coursefe/h5_course_fe  课程加速项目，新的课程系统

## 更新记录

版本更新记录请参考 [小鹅通小程序&APP版本功能记录]：https://doc.weixin.qq.com/sheet/e3_ADcApgY7ACk7GuiFBPpQvGBrd7o7R?scode=ALQAnAdhAAYm7ikX1EAOEA3AbDACU

## 待办事项

1. XeEnterShop组件优化，目前是各调用侧都写了一套校验登录态后的跳转方法，是否可以传入回调函数在组件中处理

2. 目前涉及到跳转课程详情的有首页，搜索页，店铺主页，分享页和其他小程序跳转中间页，是否可以统一用shopIndexHooks来跳转# xiaoe_learn_mini_program
# xiaoe_learn_mini_program
# xiaoe_learn_mini_program
# xiaoe_learn_mini_program
# xiaoe_learn_mini_program
# learn_mini_program
