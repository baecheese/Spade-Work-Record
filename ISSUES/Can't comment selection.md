# 🛠 comment selection not working in xcode

업데이트 하더니 comment + / 가 먹통이다.
다음처럼 따라해서 해결.

> If Cmd-/ still doesn't work in Xcode 8 on an OS X 10.11 (and apparently on a macOS Sierra - thanks to @DanBlakemore), and sudo /usr/libexec/xpccachectl and a reboot didn't help, try the following.
> 
> 1. Close Xcode.
> 2. Open /Applications in Finder, and rename Xcode.app to Xcode2.app (or any other name).
> 3. Rename it back to Xcode.app, and relaunch.
> 4. It should work now.
> 
> The problem seems to be that for whatever reason the system "uninstalls" Xcode extensions at some point, and won't "install" them again. This can be checked by opening Console, and grepping for INSTALL. If you have INSTALLED/UNINSTALLED for com.apple.dt.XcodeBuiltInExtensions, it won't work if UNINSTALLED was the last action on it, and will work it if was INSTALLED.
> 
> Figured this out when debugging an Xcode 8 extension.

출처 - https://stackoverflow.com/questions/38712365/xcode-8-beta-4-comment-shortcut-disabled

#Development/study