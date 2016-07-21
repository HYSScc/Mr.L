# IOS 文件系统之目录



| Device Path | r/w? | persistent? | OS clears | sync | private | 
| --                         | -- | -- | -- | -- | -- | -- | -- |
| /var/mobile/Applications/<UUID>/|  r  | N/A | N/A  | N/A | Yes |
| --appname.app/                  |  r  | N/A | N/A  | N/A | Yes | 
| ----www/                        |  r  | N/A | N/A  | N/A | Yes |
| --Documents/                    | r/w | Yes | No   | Yes | Yes | 
| --Library/                      | r/w | Yes | No   | Yes | Yes | 
| ----Caches/                     | r/w | Yes*|Yes***| No  | Yes |
| ----Preferences/                | r/w | Yes | No   | Yes | Yes | 
| --tmp/                          | r/w | No**|Yes***| No  | Yes |




参考: 
1. 设置禁止Document和Library目录下的文件同步到iCloud: http://blog.csdn.net/benbenxiongyuan/article/details/40147033
2. 阻止文件不被上传到iCloud: http://blog.csdn.net/a921800467b/article/details/38386787
