docker技术日益普遍，我的项目大多数都使用docker运行，这极大的方便了部署与维护。但是在docker的使用中产生的数据会让docker的数据目录越来越大，默认安装安装docker，将会占用根目录，对于大部分云服务器而言，所给的系统盘容量较小，通常可以加购数据盘，本帖记录数据盘挂载及docker数据迁移过程。
![Image](https://github.com/user-attachments/assets/0b5cc253-1516-4452-a2b2-d14167bca811)

## 挂载数据盘

1. `df -h`查看磁盘情况
![Image](https://github.com/user-attachments/assets/768e6e08-8137-4508-b543-e620664fe664)
若只有一个磁盘/dev/vda1，说明数据盘没有挂载。

2. `fdisk -l`
![Image](https://github.com/user-attachments/assets/69173492-dcb8-47d4-b1fe-2379e26a8599)
如果发现上面输出结果中没有类似 Disk /dev/vdb:的部分，说明没有数据盘，下面的挂载操作没有意义，可以直接跳到下一部分。

3. 对磁盘分区`fdisk  /dev/vdb`
依次输入m 、p、1、回车，回车、wq 即可。

4. 格式化磁盘`mkfs.ext4  /dev/vdb1`

5. 将磁盘挂载到系统中
`mount  /dev/vdb1  /mnt/data`(需要提前创建需要挂载的位置/mnt/data)

6. 配置服务器重启自动挂载
`blkid`查询磁盘UUID
![Image](https://github.com/user-attachments/assets/186a92b1-2483-4493-a9d0-3be307c81bac)
修改/etc/fstab文件 `vim /etc/fstab`
![Image](https://github.com/user-attachments/assets/324b4fec-8bfe-4ee1-944e-96feea1d471c)
添加`UUID=2b2f2aea-4153-4f32-a0ba-8258c849929f /mnt/data ext4 defaults 0 2`

## Docker数据迁移

1. 停止docker服务
`sudo systemctl stop docker`

2. 创建新文件夹
`mkdir /mnt/data/docker`

3. 移动文件
`sudo rsync -avzh /var/lib/docker/ /mnt/data/docker/`

4. 更新Docker配置
`vim /etc/docker/daemon.json`
如果文件 /etc/docker/daemon.json 不存在，就创建它。添加或更新以下内容：
```
{
    "data-root": "/mnt/data/docker"
}
```

5. 重新启动 Docker 服务
`sudo systemctl start docker`

6. 验证 Docker 是否正常工作，并且新的数据存储位置是否正在使用。可以通过运行容器来测试。
一旦确认一切正常，删除旧的 Docker 数据目录：
`sudo rm -rf /var/lib/docker`
通过`docker info`查看Docker信息
![Image](https://github.com/user-attachments/assets/0d961e2e-c122-4351-b679-29d7556f2a29)
` Docker Root Dir: /mnt/data/docker`
该行表示docker数据位置。
