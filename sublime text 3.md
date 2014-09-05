###Download
[download](http://www.sublimetext.com/3)

###安装 Package Control
ctrl + ` 或者 View -> Show Console
复制下面代码到控制台

	import urllib.request,os,hashlib; h = '7183a2d3e96f11eeadd761d777e62404' + 'e330c659d4bb41d3bdf022e94cab3cd0'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)


###插件安装
crtl + shift + P   输入pci 弹出命令行中输入相应插件名即可
* ConvertToUTF8　　支持 GBK, BIG5, EUC-KR, EUC-JP, Shift_JIS 等编码的插件
* Bracket Highlighter　　用于匹配括号，引号和html标签。对于很长的代码很有用。安装好之后，不需要设置插件会自动生效
* DocBlockr　　DocBlockr可以自动生成PHPDoc风格的注释。它支持的语言有Javascript, PHP, ActionScript, CoffeeScript, Java, Objective C, C, C++
* Emmet(Zen Coding)　　快速生成HTML代码段的插件，强大到无与伦比，不知道的请自行google
* SideBar Enhancements　　这个插件改进了侧边栏，增加了许多功能
* Themr　　主题管理，切换主题的时候，不用自己修改配置文件了，用这个可以方便的切换主题


###快捷捷
可参考菜单栏的提示