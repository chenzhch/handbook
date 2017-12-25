### CentOS6上安装gitlab
#### 一、gitlab下载地址
https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el6/  
下载文件gitlab-ce-10.2.5-ce.0.el6.x86_64.rpm  
2017年12月24日前汉化补丁最高版为10.2.5
#### 二、修改yum资源库
mkdir /media/CentOS  
mount /dev/cdrom  /media/CentOS  
vi /etc/yum.repos.d/CentOS-Media.repo  
baseurl=file:///media/CentOS/ #若无此内容则增加  
#### 三、安装依赖库
sudo yum --disablerepo=\* --enablerepo=c6-media install -y curl policycoreutils-python openssh-server cronie   
sudo lokkit -s http -s ssh  
#### 四、安装gitlab-ce
rpm -ivh gitlab-ce-10.2.5-ce.0.el6.x86_64.rpm    
#### 五、gitlab汉化
git clone https://gitlab.com/xhang/gitlab.git  
git diff v10.2.5  v10.2.5-zh >patch.txt  
patch -d /opt/gitlab/embedded/service/gitlab-rails -p1 < patch.txt  
#### 六、配置与启动
##### 1、修改配置文件
vi /etc/gitlab/gitlab.rb 修改配置文件  
external_url 'http://gitlab.example.com'  #改为本机地址  
git_data_dirs({  
  "default" => {  
    "path" => "/home/gitlab"  
   }  
})  #增加git资源库存放路径  
gitlab-ctl reconfigure   
##### 2、启动  
gitlab-ctl start #启动  
gitlab-ctl status #查看状态  