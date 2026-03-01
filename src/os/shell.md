Shell

## 条件 test & []

### 字符串比较

#### 字符串相同则结果为真

```sh
if test str1 = str2
if [ str1 = str2 ]
```

#### 字符串不同则结果为真

```sh
if test str1 != str2
if [ str1 != str2 ]
```

#### 字符串不为空则结果为真

```sh
if test -n str
if [ -n str ]
```

#### 字符串为null则结果为真

```sh
if test -z str
if [ -z str ]
```

### 算术比较

#### 相等

```sh
if test exp1 -eq exp2
if [ exp1 -eq exp2 ]
```

#### 不相等

```sh
if test exp1 -ne exp2
if [ exp1 -ne exp2 ]
```

#### 大于

```sh
if test exp1 -gt exp2
if [ exp1 -gt exp2 ]
```

#### 大于等于

```sh
if test exp1 -ge exp2
if [ exp1 -ge exp2 ]
```

#### 小于

```sh
if test exp1 -lt exp2
if [ exp1 -lt exp2 ]
```

#### 小于等于

```sh
if test exp1 -le exp2
if [ exp1 -le exp2 ]
```

#### 非

```sh
if test ! exp
if [ ! exp ]
```

### 文件有关的条件测试

#### 如果文件是一个目录结果为真

```sh
if test -d file
if [ -d file ]
```

#### 如果文件存在则结果为真，历史上-e选项不可移植，所以通常使用的是-f选项

```sh
if test -e file
if [ -e file ]
```

#### 如果文件是一个普通文件则结果为真

```sh
if test -f file
if [ -f file ]
```

#### 如果文件的set-group-id位被设置则结果为真(set-group-id 和 set-gid 授予程序其所在组的访问权限 通过chmod 选项 g 设置，该标志对shell脚本程序不起作用，只对可执行的二进制文件有用)

```sh
if test -g file
if [ -g file ]
```

#### 如果文件可读则结果为真

```sh
if test -r file
if [ -r file ]
```

#### 如果文件的大小不为0则结果为真

```sh
if test -s file
if [ -s file ]
```

#### 如果文件的set-user-id位被设置则结果为真(set-user-id 和 set-uid 授予程序其拥有者的访问权限，不是其使用者的访问权限 通过chmod 选项 u 设置，该标志对shell脚本程序不起作用，只对可执行的二进制文件有用)

```sh
if test -u file
if [ -u file ]
```

#### 如果文件可写则结果为真

```sh
if test -w file
if [ -w file ]
```

#### 如果文件可执行则结果为真

```sh
if test -x file
if [ -x file ]
```

## 控制结构 if & elif & for & while & until & case & AND列表 & OR列表 & {}语句块
