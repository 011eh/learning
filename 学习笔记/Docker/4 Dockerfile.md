# Dockerfile

```dockerfile
############### Build 时运行 ###############
from		# 指定基础镜像
maintainer	# 镜像维护者信息，姓名+邮箱
copy		# 将文件拷贝到镜像
add			# 添加镜像，会自动解压
run			# 指定构建时，需要运行的命令
onbuild		# 构建一个被继承的Dockerfile时，就会执行onbuild的指令

############### Build、Run 都运行 ###############
workdir		# 镜像工作目录
user		# 设置用户，默认为 root

############### Run 时运行 ###############
volume		# 设置挂载目录
env			# 构建时，设置环境变量
expose		# 声明暴露的端口
cmd			# 容器运行时运行的命令，会覆盖前面的cmd，docker run 的参数将覆盖
entrypoint	# 容器运行时运行的命令，指定运行所需的参数，搭配 cmd使用，cmd 上面的对应参数将作为参数值传入
```
