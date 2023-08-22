- *光纤交换机作为SAN网络的重要组成部分，在日常应用中非常普遍，本次将以常用的博科交换机介绍基本的配置方法*
- 博科300实物图：
  ![image.png](../assets/image_1692695472216_0.png)
- ## 一、环境描述：
	- ![image.png](../assets/image_1692695538167_0.png)
- *四台服务器通过各自的双HBA卡连接至两台博科300光纤交换机，IBM V3700为双控制器，每个控制器再分别与两台光纤交换机相连。*
- 完成所有的连线及配置工作后，还需对光纤交换机作相应配置，当然不配置当傻瓜机也能使用，但一般还是需要配置，创建别名、划分ZONE等，这里只介绍一台光纤交换机的配置过程，另一台配置方法相同
- 交换机接口详细连接信息如下：
	- ![image.png](../assets/image_1692695623636_0.png)
- 准备创建的别名信息如下：
	- ![image.png](../assets/image_1692695684251_0.png)
- 准备划分的ZONE信息如下：
	- ![image.png](../assets/image_1692695716593_0.png)
- ## 二、配置步骤
	- 开始配置光纤交换机，可以从图形界面(JAVA)或者命令行两种方式配置，首先介绍图形化配置方式，在浏览器中输入10.77.77.77进入登陆界面，输入默认登陆信息admin/password，进入管理系统。
		- ![image.png](../assets/image_1692695815030_0.png)
	- 管理系统界面：
		- ![image.png](../assets/image_1692695851745_0.png)
	- 点击上图红框中的Zone Admin进入ZONE配置界面，如下图，配置顺序为首先创建Alias（别名）、再创建Zone（区域）、最后配置Zone Config（保存配置）：
		- ![image.png](../assets/image_1692695894210_0.png)
	- 按照上面规划好的别名开始创建，点击上图中的New Alias，如下图，输入名称，点击OK
		- ![image.png](../assets/image_1692695924882_0.png).
	- 如下图，Name中选择好刚创建的别名，然后在Member Selection List中选择存储V3700实际接的端口，这里为1,0，选中1,0，再点击中间的“Add Member >>”，完成端口的添加。这样一个别名就创建好了，其它5个别名也按此方法创建
		- ![image.png](../assets/image_1692695957508_0.png)
	- 接着开始创建Zone，如下图，选择中间的Zone选项栏，如下图，点击New Zone，输入ZONE name，点OK
		- ![image.png](../assets/image_1692695986256_0.png)
	- Name选中刚才创建的Zone，Member selection List中选择Aliases，选择该区域的两个端口，即esxi01_hba01和ibmv3700_spa01，选中点击Add Members添加至zone中，其它7个zone也同样方法添加。
		- ![image.png](../assets/image_1692696018344_0.png)
	- 完成8个ZONE创建后如下图所示；
	  接着点Zone Config开始保存配置，输入Config name名称，如下图
		- ![image.png](../assets/image_1692696044992_0.png)
	- 如下图，在Member Selection List中，全选刚才建的8个Zone，点击Add Member，添加至右边窗口中，再点击下图中的Enable Config按扭来生效配置；
		- ![image.png](../assets/image_1692696074457_0.png)
	- 选择刚才创建的配置文件newconfig，点击OK，完成后，再点右边的Save Config来保存配置信息。
		- ![image.png](../assets/image_1692696097438_0.png)
- 通过命令配置：（可选，建议用WEB配置）
	- ### 1.alias创建：
		- 第一台交换机：
		- ```
		  alicreate “ibm_v7000_spa01”,“1,0”
		  alicreate “ibm_v7000_spb01”,“1,1”
		  alicreate “esxi01_hba01”,“1,2”
		  alicreate “esix_02_hba01”,“1,3”
		  alicreate “yingyongserver01_hba01”,“1,4”
		  alicreate “yingyongserver02_hba01”,“1,5”
		  -----------------------------------
		  博科FC光纤交换机详细配置教程
		  https://blog.51cto.com/chelaoer/4758247```
		-