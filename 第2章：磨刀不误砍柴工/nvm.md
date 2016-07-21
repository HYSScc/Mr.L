# NVM

    github: [https://github.com/creationix/nvm](https://github.com/creationix/nvm)


## brew方式安装NVM：

    1. brew install nvm安装nvm
    2. 在shell的配置文件(~/.bashrc, ~/.profile, or ~/.zshrc)中添加如下内容：
        # For NVM
        export NVM_DIR=~/.nvm
        source $(brew --prefix nvm)/nvm.sh
      注意配置的顺序，以防开启新终端，node出现找不到的情况。  
    3. 重启终端，命令行下即可使用nvm，使用nvm install <version>进行对应的node版本安装. 
        使用nvm use <version>使用， 再通过nvm alias default <version>确保有默认版本。最后使用nvm ls查看
    4. 注意： 如果之前通过'brew install node'方式安装过node，那么需要先删除系统中存在的node：
        brew remove --force node
        sudo rm -r /usr/local/lib/node_modules
        brew prune
        sudo rm -r /usr/local/include/node
        #检查brew是否正常
        brew doctor
    