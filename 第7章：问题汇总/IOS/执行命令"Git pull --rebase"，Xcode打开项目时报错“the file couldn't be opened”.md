# 执行命令"Git pull --rebase"，Xcode打开项目时报错“the file couldn't be opened”

IOS开发过程中，当我们使用Git管理项目开发时，如果我们合并代码时，项目工程文件.xcodeproj有冲突，则会报错“the file couldn't be opened”，此时可以尝试以下解决办法：

    In that case you can try this:
    1. In finder right click on the .xcodeproj and choose 'Show Package contents'.
    2. Open project.pbxproj in an text editor (this is the actual project file, and has to be valid XML)
    3. Check for merge conflicts (look for <<<<<<< and >>>>>>>) and manually resolve them (be careful!), and ensure the file has valid XML format
    4. Save the file
    5. Try again opening the .xcodeproj with Xcode


执行以上步骤，解决完冲突后，需要重新执行"pod install"命令然后才能编译通过






参考：http://stackoverflow.com/questions/20105790/xcodeproj-file-is-lost-cannot-be-opened-after-a-branch-merge