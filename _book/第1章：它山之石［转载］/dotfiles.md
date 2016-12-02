# dotfiles-终端(terminal)一键配置

    github: [https://github.com/robertzhouxh/dotfiles](https://github.com/robertzhouxh/dotfiles)
    

##     手动配置：
    一. 修改文件(~/.bashrc)
      1. 使用Tab键自动补全的时候，报"cd dot-bash: compopt: command not found" 
        更改shell为bash: chsh -s /usr/local/bin/bash
      2. 使用nvm命令报错"nvm: command not found"
         配置nvm路径: 
         # For NVM
         export NVM_DIR=~/.nvm
         source $(brew --prefix nvm)/nvm.sh
         
     二. 修改文件(~/.bash_profile)
         1. 报错"bash: adb: command not found"
            * 在本地目录（home directory）中打开或创建文件.bash_profile
            * 在文件中写入以下内容：
              #for android开发环境
              export PATH=${PATH}:/Volumes/creasy/Software/android-sdk-macosx/tools:/Volumes/creasy/Software/android-sdk-macosx/platform-tools
         
     三. 新增配置
       1. Vim自动补全插件Plugin 'SirVer/ultisnips': [https://github.com/SirVer/ultisnips](https://github.com/SirVer/ultisnips)
       2. 