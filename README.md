# AutoApi-E5
office365 E5自动续期计划



# AutoApi v6.0 ———— E5自动续期01
AutoApi系列：AutoApi(v1.0)~~、~~AutoApiSecret(v2.0)~~、~~AutoApiSR(v3.0)~~、~~AutoApiS(v4.0)~~、~~AutoApiP(v5.0)~~、AutoApi(v6.0) 1

## 说明 ##
* E5自动续期程序，但是**不保证续期**
* 设置了**周六日(UTC时间)不启动**自动调用，周1-5每6小时自动启动一次 （修改看教程）
* 调用api保活：
     * 查询系api：onedrive,outkook,notebook,site等
     

## 步骤 ##
* 准备工具：
   * E5开发者账号（**非个人/私人账号**）
       * 管理员号 ———— 必选 
       * 子号 ———— 可选 （不清楚微软是否会统计子号的活跃度，想弄可选择性补充运行）    
   * rclone软件，[下载地址 rclone.org ](https://downloads.rclone.org/v1.53.3/rclone-v1.53.3-windows-amd64.zip)，(windows 64）
   * 教程图片看不到请科学上网
   
* 步骤大纲：
   * 微软方面的准备工作 （获取应用id、密码、密钥）
   * GIHTHUB方面的准备工作  （获取Github密钥、设置secret）
   * 试运行
   
#### 微软方面的准备工作 ####

* **第一步，注册应用，获取应用id、secret**

    * 1）点击打开[仪表板](https://aad.portal.azure.com/)，左边点击**所有服务**，找到**应用注册**，点击+**新注册**
    
     ![image](https://github.com/Mark-J81/Images/blob/main/01.png)
    
    * 2）填入名字，受支持账户类型前三任选，重定向填入 http://localhost:53682/ ，点击**注册**
    
    ![image](https://github.com/Mark-J81/Images/blob/main/02.png)
    
    * 3）复制应用程序（客户端）ID到记事本备用(**获得了应用程序ID**！)，点击左边管理的**证书和密码**，点击+**新客户端密码**，点击添加，复制新客户端密码的**值**保存（**获得了应用程序密码**！）
    
    ![image](https://github.com/Mark-J81/Images/blob/main/03.png)
    
    ![image](https://github.com/Mark-J81/Images/blob/main/04.png)
    
    * 4）点击左边管理的**API权限**，点击+**添加权限**，点击常用Microsoft API里的**Microsoft Graph**(就是那个蓝色水晶)，
    点击**委托的权限**，然后在下面的条例选中下列需要的权限，最后点击底部**添加权限**
    
    **赋予api权限的时候，选择以下12个**
  
                Calendars.ReadWrite、Contacts.ReadWrite、Directory.ReadWrite.All、
                
                Files.ReadWrite.All、MailboxSettings.ReadWrite、Mail.ReadWrite、
                
                Mail.Send、Notes.ReadWrite.All、People.Read.All、
                
                Sites.ReadWrite.All、Tasks.ReadWrite、User.ReadWrite.All
    
    ![image](https://github.com/Mark-J81/Images/blob/main/05.png)
    
    ![image](https://github.com/Mark-J81/Images/blob/main/06.png)
     
    ![image](https://github.com/Mark-J81/Images/blob/main/07.png)
    
    * 5）添加完自动跳回到权限首页，点击**代表授予管理员同意**
    
    ![image](https://github.com/Mark-J81/Images/blob/main/08.png)
    
* **第二步，获取refresh_token(微软密钥)**

    * 1）rclone.exe所在文件夹，shift+右键，在此处打开powershell，输入下面**修改后**的内容，回车后跳出浏览器，登入e5账号，点击接受，回到powershell窗口，看到一串东西。
           
                ./rclone authorize "onedrive" "应用程序(客户端)ID" "应用程序密码"
               
    * 2）在那一串东西里找到 "refresh_token"：" ，从双引号开始选中到 ","expiry":2021 为止（就是refresh_token后面双引号里那一串，不要双引号），如下图，右键复制保存（**获得了微软密钥**）
    
    ![image](https://github.com/Mark-J81/Images/blob/main/09.png)
    
 ____________________________________________________
 
 #### GITHUB方面的准备工作 ####

 * **第一步，fork本项目**
 
     登陆/新建github账号，回到本项目页面，点击右上角fork本项目的代码到你自己的账号，然后你账号下会出现一个一模一样的项目，接下来的操作均在你的这个项目下进行。
     
 * **第二步，新建github密钥**
 
    * 1）进入你的个人设置页面 (右上角头像 Settings，不是仓库里的 Settings)，选择 Developer settings -> Personal access tokens -> Generate new token

    ![image](https://github.com/Mark-J81/Images/blob/main/12.png)
    
    ![image](https://github.com/Mark-J81/Images/blob/main/13.png)
    
    * 2）设置名字为 **GITHUB_TOKEN** , 然后勾选repo，点击 Generate token ，最后**复制保存**生成的github密钥（**获得了github密钥**，一旦离开页面下次就看不到了！）
   
   ![image](https://github.com/Mark-J81/Images/blob/main/14.png)
  
 * **第三步，新建secret**
 
    * 1）依次点击上栏Setting > Secrets > Add a new secret，新建两个secret如图：CONFIG_ID、CONFIG_KEY。

  内容分别如下: ( 把你的应用id改成你的应用id , 你的应用机密改成你的机密，单引号不要动 )

  CONFIG_ID

  ```shell
  id=r'你的应用id'
  ```

  CONFIG_KEY

  ```shell
  secret=r'你的应用机密'
  ```
![image](https://github.com/Mark-J81/Images/blob/main/15.png)

![image](https://github.com/Mark-J81/Images/blob/main/16.png)
    
     **(以下填入内容注意前后不要有空格空行)**
 * 2）在线编辑你项目里的1.txt，将整个refresh_token覆盖粘贴进去（里面是我的数据，先删掉或者覆盖掉）。（千万不要改1.py）

    > refresh_token位置如图下。复制refresh_token紧接着的双引号里的内容（红竖线框起来的），不要把双引号复制进去。复制进1.txt后，留意结尾不要留空格或者空行

________________________________________________

#### 试运行 ####

   * 1）点击上栏中间的Action进入运行日志页面，中间应该有个绿色按钮（I understand my workflow...），点击。
   
       自动刷新后，会看到左边有三个流程，一个Run api.Read，一个Run api.Write，一个Update Token。
       
         工作流程说明
             Run api.Write：创建系api，一天自动运行一次
             Run api.Read:  查询系api，每6小时自动运行一次
             Update Token： 微软密钥更新，每2天运行一次
             
       这三个流程名字前面应该是都有黄色感叹号的
   
       分别点进去，然后会看到有个黄条（this schedule was disabled......），点击 enable workflow 按钮，**三个流程都要按这个！**
   
       （不确定是否都需要进行这一步，我自己做视频教程的时候发现有的。如果你没有，直接忽略并往下进行，能正常运行就可以了 ）
   
   * 2）点击两次右上角的星星（star，就是fork按钮的隔壁）启动action，
   
        再点击上面的Action选择Run api.Read或者api.Write流程 -> build -> run api 就能看到每次的运行日志

       （必需点进去build里面的run api.XXX看下，api有没有调用到位，操作有没有成功，有没有出错）
     
   * 3）再点两次星星，查看是否能再次成功运行
   
        然后点击Action里的 update token 流程 -> build -> update token ，日志里显示“微软密钥上传成功”。
       
        同时，依次点击页面上栏右边的 Setting -> 左栏 Secrets（也就是Github方面准备的第三步的secret页面），应该能看到MS_TOKEN显示刚刚update了
        
        （这一步是为了保证重新上传到secret的token是正确的）
    
        
#### 教程最后 ####

   程序会自行按计划启动，不必操心。
   
        但是github更新了防止薅羊毛的规则，如果仓库60天无任何变动，将会暂停Action，但是会发邮件通知，所以请留意邮箱，收到邮件请上来手动启动一下action。
       （我还没有收到过此邮件，但是据说邮件里会有启动链接，或者上来按两次星星按钮就行）
### 教程完 ###
