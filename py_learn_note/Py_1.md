# Python learn note ____ 1

### ***pyke elysia***

### os

#### os.system

`os.system()` 函数用于执行 shell 命令。但是并不会返回命令的输出结果。可以用于一般的 `cmd` 命令。

```python
# test.py
print("Hello cmd")

# main.py
import os
os.system("python test.py")

# output
Hello cmd
```

#### os.popen

`os.popen()` 函数用于执行 shell 命令，并返回命令的输出结果。

```python
# test.py
print("Hello cmd")

# main.py
import os
output = os.popen("python test.py")
print(output.readlines())

# output
['Hello cmd\n']
```

### subprocess

#### subprocess.run

`subprocess.run()` 函数用于执行 shell 命令，并返回命令的输出结果。

关键参数：
- args: 输入的命令参数
- stdout: 输出结果，可以是一个文件对象，也可以是 `subprocess.PIPE`
- stderr: 错误结果，与 `stdout` 类似
- shell: 是否使用 `shell` 执行命令

```python
# test.py
print("Hello cmd")

# main.py
import subprocess
cmd = "python test.py"
response = subprocess.run(cmd, shell=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE, encoding="gbk")
output = response.stdout
error = response.stderr
print(output)
print(error)

# output
Hello cmd
None
None
```

#### subprocess.Popen

`subprocess.Popn()` 与 `subprocess.run()` 类似，但是运行时会新开一个进程，而不像 `subprocess.run()` 一样堵塞在当前进程。

#

***创建于2025/8/8***