# greys pid, trace

有赞使用方法：切换为app角色：sudo su app;bash ./greys.sh pid

使用greys工具跟踪线上线程的执行栈
   1. 在psp上申请一周的应用管理员角色
   2. 先ssh登陆到目的机上，执行下载并安装tbox工具箱：
      curl -sLk http://tbox.cn-hangzhou.oss-cdn.aliyun-inc.com/install.sh | sh && source ~/.bash_profile
   3. 找到目标进程
      ps -ef | grep admin
   4. 进入目标进程
      greys pid
   5. 跟踪目标类的目标方法
      trace class method  
   6. 查看调用的参数值  
      tt -t class method
      tt -i 对应index    
    
    trace com.taobao.trip.hotel.hso.service.impl.OuterRoomSearchServiceImpl searchOuterRates
    
使用例子：
  1. 观察函数调用之前之后的参数值
  - 之前: 
  watch -b *SubscriptionInnerServiceImpl  createSub params[0]
  - 之后（可能在函数处理过程中改了值，可以通过该方式来检查）: 
  watch -f *OuterRoomSearchServiceImpl  searchOuterRates params[0]
  - 观察函数调用之后的返回值
  watch -f  *OuterRoomSearchServiceImpl  searchOuterRates returnObj
  
  2. 查看函数调用路径
  trace  *OuterRoomSearchServiceImpl  searchOuterHotels

  3. 查看一次调用记录栈
  tt -t *OuterRoomSearchServiceImpl  searchOuterRates 
  通过 tt -i 1006  看具体某一次调用的细节
  -w params[0] -x1：查看第一个参数展开第一层的内容
  -w returnObj  -x1：查看返回值展开第一层的内容
      
  具体可参考：http://www.open-open.com/lib/view/open1452174472698.html