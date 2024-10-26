systemctl restart docker 重启docker
挂载点不存在问题docker: Error response from daemon: cgroups: cgroup mountpoint does not exist: unknown.
执行命令一：sudo mkdir /sys/fs/cgroup/systemd

执行命令二：sudo mount -t cgroup -o none,name=systemd cgroup /sys/fs/cgroup/system



docker删除镜像 docker rmi 【images】

docker删除已经停止的镜像 docker rm $(docker ps -a -q)





sonar

docker run -d --name sonarqube -e SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true -p 9000:9000 sonarqube:latest