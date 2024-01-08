此文档用于ubuntu系统激活typora license

#### 1.下载typora安装包

```
# 查看电脑架构
uname -m

# 下载 Typora 安装包
# X86(Amd) 架构 
wget https://download2.typoraio.cn/linux/typora_1.7.4_amd64.deb --output-document typora.deb

# Arm 架构
wget https://download2.typoraio.cn/linux/typora_1.7.4_arm64.deb --output-document typora.deb

# 安装 Typora 软件包
sudo dpkg -i typora.deb
```

#### 2.克隆Yporaject项目

```
# 可以直接克隆本项目的仓库, depth=1 表示仅克隆最新版本,以减少等待时间
git clone https://github.com/hazukieq/Yporaject.git --depth=1
```

#### 3.配置Rust环境

由于编译项目需要 **Rust** 的支持，所以我们需要配置相关环境(若已有，则可跳过该步骤)

```
#安装
curl https://sh.rustup.rs -sSf | sh
#在出现的选项中选择 1 ，enter
```

```
#配置执行路径
#安装工具会自动在 ~/.profile 里加入 ~/.cargo/bin 的 PATH 设置
#如果你的文件没有自动加上 PATH 设置，你可以尝试手动加上

# set PATH so it includes user's private bin if it exists
if [ -d "$HOME/.local/bin" ] ; then
    PATH="$HOME/.local/bin:$PATH"
fi
. "$HOME/.cargo/env"
```

为了使上面的配置生效，我们需要：

```
source ~/.profile
```

测试是否成功安装

```
$ rustc --version
#以及
$ cargo --version
#输出版本信息则成功
```

#### 4.编译Yporaject

```
# 进入 Yporaject 项目
cd Yporaject
# 运行编译命令
cargo build
# 查看二进制是否生成,程序名称为 node_inject
ls target/debug
# 尝试运行该二进制程序
cargo run
output: 
no node_modules.asar found
move me to the root of your typora installation(the same directory as executable of electron)
```

请务必确认当前项目目录 **target/debug 下** 是否生成了 **node_inject 二进制程序**

#### 5.复制二进制程序到安装目录下

```
# 记录当前目录路径，待会返回需要用到
cur=`pwd`

# 复制二进制程序到相关目录下
sudo cp target/debug/node_inject /usr/share/typora
# 进入相关目录
cd /usr/share/typora
# 给予二进制程序执行权限
sudo chmod +x node_inject

# 运行二进制程序
# (请注意程序运行输出信息，观察是否运行成功！！)
# 若无读写权限,建议使用 sudo ./node_inject
sudo ./node_inject
```

#### 6.获取许可证激发码

```
# 返回项目
cd $cur
# 进入 license-gen 文件夹
cd license-gen
# 编译代码
cargo build
# 运行二进制程序
cargo run
# 你将会得到以下输出
output:
    Finished dev [unoptimized + debuginfo] target(s) in 0.00s
     Running `target/debug/license-gen`
License for you: xxxxxx-xxxxxx-xxxxxx-xxxxxx
```

复制 **License for you: xxxxxx-xxxxxx-xxxxxx-xxxxxx** 的那一串激发码，待会需要用到

#### 7.激活软件

依次点击界面上方菜单栏选项 **help > my license(帮助 > 我的许可证...)**

邮箱随便填，激活码一栏**粘贴刚才得到的激发码**

over