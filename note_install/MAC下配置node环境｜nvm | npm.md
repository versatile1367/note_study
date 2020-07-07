# MAC下配置node环境｜nvm|npm

1. 利用brew来安装nvm

   `brew install nvm`

2. 但是此时nvm的命令还是识别不了，每次重启中断，nvm的命令就失效了，所以每次运行：

   `export NVM_DIR=~/.nvm`

   `source $(brew --prefix nvm)/nvm.sh`

3. 此时nvm命令已经可以被识别了。输入`nvm`可以识别

4. 为了以后每次重启都不用再运行，在`~/.zshrx` 中配置一下，保证每次重启时，系统自动配置好。

   `vi ~/.zshrc`

   加入上面的两句话

   `source ~/.zshrc`

5. 但是目前仍然不能通过nvm来安装node，`nvm ls-remote` 无内容

6. 在./bash_profile文件中，写入：

   ```
   export NVM_DIR="$HOME/.nvm"
   [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
   [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
   ```

7. 然后重新打开terminal

8. 接下来开始下载安装node,我选择安装稳定版本的node:

   `npm install stable`

9. 查看安装的node版本：

   `nvm ls`

10. 为了避免重启终端后node和npm失效，要手动绑定node的版本号，所以运行:

    `nvm use v14.5.0`

    `nvm ls`

    如果还没有，就使用：

    `nvm alias default v14.5.9`

11. node部署完成

12. npm 已经随着nodejs安装一并安装的，尝试用npm全局安装react-cli脚手架：

    `nvm install -g build-react`

13. 报错信息

    根据提示，运行命令：

    `sudo chown -R 501:20 "/Users/ganjiaqi/.npm"`

14. 然后再安装：

    `npm install -g build-react`

    安装成功！

    

