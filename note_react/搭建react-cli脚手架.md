# 搭建react-cli脚手架

1. 先参考笔记搭建环境，安装npm

2. 安装react-cli脚手架：

   `npm install -g build-react`

3. 使用脚手架搭建项目

4. 搭建项目：

   `build-react init my-react`

   然后会有一些问题，例如项目名称作者等等，我设置的项目名称为demo_1

   然后可以发现在对应的文件夹下面有了我的工程demo_1

   <img src="/Users/ganjiaqi/Library/Application Support/typora-user-images/截屏2020-07-03 下午12.04.32.png" alt="截屏2020-07-03 下午12.04.32" style="zoom:50%;" align='left'/>

5. 进入项目，安装项目依赖后运行：

   `cd demo_1`

   `npm install`

   `npm start`

   <img src="/Users/ganjiaqi/Library/Application Support/typora-user-images/截屏2020-07-03 下午12.08.25.png" alt="截屏2020-07-03 下午12.08.25" style="zoom:50%;" align='left'/>

6. 启动项目，打开项目文件夹后，在`src/package.json` 中在`"scripts":{"start":}`里面加入`--open`

   <img src="/Users/ganjiaqi/Library/Containers/com.tencent.WeWorkMac/Data/Library/Application Support/WXWork/Temp/ScreenCapture/企业微信截图_e1b17e9d-81b5-4e18-aab3-9f79626ba166.png" alt="企业微信截图_e1b17e9d-81b5-4e18-aab3-9f79626ba166" style="zoom: 33%;" align='left'/>

   这样项目可以自动打开默认的浏览器运行。

7. 然后可以进行自己的react项目

8. 打包项目：

   `npm run build`

   

