# robtgo 使用
## win11 安装mingw64
https://github.com/niXman/mingw-builds-binaries/releases/download/14.2.0-rt_v12-rev0/x86_64-14.2.0-release-win32-seh-ucrt-rt_v12-rev0.7z

```shell
>gcc -v
gcc version 14.2.0 (x86_64-win32-seh-rev0, Built by MinGW-Builds project)
```
## go env启用`CGO_ENABLE=1`和`CC=gcc`
## 然后运行go程序就可以监听键盘事件
```GO
package main

import (
	"fmt"

	hook "github.com/robotn/gohook"
)

func main() {
	add()

	low()
}

func add() {
	fmt.Println("--- Please press ctrl + shift + q to stop hook ---")
	hook.Register(hook.KeyDown, []string{"q", "ctrl", "shift"}, func(e hook.Event) {
		fmt.Println("ctrl-shift-q")
		hook.End()
	})

	fmt.Println("--- Please press w---")
	hook.Register(hook.KeyDown, []string{"w"}, func(e hook.Event) {
		fmt.Println("w")
	})

	s := hook.Start()
	<-hook.Process(s)
}

func low() {
	evChan := hook.Start()
	defer hook.End()

	for ev := range evChan {
		fmt.Println("hook: ", ev)
	}
}

```