# 使用webstorm编写node.js操作redis

## 步骤

1. 安装node.js

   【说明】

   安装完成后，会设置windows环境变量的path中自动设置全局模块安装路径 C:\Users\jerry\AppData\Roaming\npm

   【注意：AppData文件夹是隐藏的】

2. 安装 express

   npm install express-generator -g

3. 安装express-generator

   C:\>npm install express-generator -g
   C:\Users\jerry\AppData\Roaming\npm\express -> C:\Users\jerry\AppData\Roaming\npm
   \node_modules\express-generator\bin\express-cli.js

   express-generator@4.15.5
   added 6 packages in 2.631s

4. 安装redis模块（全局安装）

   在任何目录下运行命令行：

   npm install redis -g

   【说明】

   redis模块会安装到全局位置：C:\Users\jerry\AppData\Roaming\npm

   查看当前全局安装位置：npm root -g

5. 也可以安装到当前项目中，也可以在当前项目中安装redis模块，这时只需要进入到项目当前文件夹中运行命令行：npm install redis

6. 在webstorm中设置redis环境

   File>Setting>Languages&Frameworks>Node.js and NPM

   设置Node interpreter：选择node安装位置

7. 建立nodejs项目

   file>new>project>Node.js Express App 输入项目位置，会自动选择node.js环境

8. 为该项目添加redis模块

   在webstorm中运行终端窗口，或者进入到项目文件夹，自己运行dos命令。

   webstorm终端运行方法：view>ToolWindows>Terminal

   在窗口中输入：nmp install redis

   此时会在当前项目文件夹下多出3个redis相关文件夹

   【注意：这种方式解决在即使全局安装redis仍旧无法加载redis模块问题。】

9. 编写js源码，并测试运行

   建立hello.js

   var redis = require("redis"); // 1
   var client = redis.createClient(); // 2
   client.set("my_key", "Hello World using Node.js and Redis"); // 3
   client.get("my_key", redis.print); // 4
   client.quit(); // 5

   右键点击源码文件hello.js选择运行“hello.js”

10. 输出结果

   Reply: Hello World using Node.js and Redis

   Process finished with exit code 0

   【运行时，注意要启动redis服务：进入redis安装目录运行命令：redis-server】

***



