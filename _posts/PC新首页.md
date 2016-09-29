PC新首页

Service

```JAVA
public interface PCMainViewService {
  
    /**
     * 获得可管理的门店列表
     * @param keeperId
     * @return ShopNameId 门店切换显示店铺列表
     * @throws ServiceException
     */
    public List<ShopNameId> getAllShopWithKeeperId(int keeperId) throws ServiceException;

    /**
     * 获得餐厅营业状态
     * @param shopId
     * @return ShopBusinessStatus 营业状态枚举
     * @throws ServiceException
     */
    public ShopBusinessStatus getShopBusinessStatus(int shopId) throws ServiceException;

    /**
     * 获得gprs打印机状态
     * @param shopId
     * @return boolean 已连接返回true,未连接返回false
     * @throws ServiceException
     */
    public boolean getGPRSPrinterStatus(int shopId) throws ServiceException;

    /**
     * 帐号操作菜单,退出登录
     * @param keeperId
     * @return ShopNameId 门店切换显示店铺列表
     * @throws ServiceException
     */
    public boolean loginOut(int keeperId) throws ServiceException;

    /**
     * 待处理订单模块
     * @param shopId
     * @return OrderNotice 订单提醒
     * @throws ServiceException
     */
    public OrderNotice getShopOrderNotice(int shopId) throws ServiceException;

    /**
     * 待处理反馈
     * @param shopId
     * @return UnHandleFeedback 待处理反馈
     * @throws ServiceException
     */
    public UnHandleFeedback getUnHandleFeedback(int shopId) throws ServiceException;

    /**
     * 今日总览
     * @param shopId
     * @return TodayBusinessData 今日营业信息
     * @throws ServiceException
     */
    public TodayBusinessData getTodayBusinessData(int shopId) throws ServiceException;


    /**
     * 活动
     * @param shopId
     * @return MarketingActivities
     * @throws ServiceException
     */
    public MarketingActivities getMarketingActivities(int shopId) throws ServiceException;

    /**
     * 餐厅体检
     * @param shopId
     * @return ShopHealthCheck
     * @throws ServiceException
     */
    public ShopHealthCheck getShopHealthCheck(int shopId) throws ServiceException;

    /**
     * 通知中心
     * @param shopId
     * @return PCNotice
     * @throws ServiceException
     */
    public PCNotice getPCNotice(int shopId) throws ServiceException;
}

```



Struct：

```
public class AccountMenu {
    private String KeeperName;//最多显示20个字符
    private String LoginOut;
}
```

```
public class MarketingActivities {
    private Map<MarketingActivitiesType, ValueAndUrl> todayBusinessData;

    @NoArgsConstructor
    @AllArgsConstructor
    @Getter
    public static enum MarketingActivitiesType {
        enrolled("已报名"),
        waitBegin("待开始"),
        enrollAcessed("可报名活动");

        private String name;
    }
}
```

```
public class OrderNotice {
    private Map<OrderType, ValueAndUrl> orderNotice;

    @NoArgsConstructor
    @AllArgsConstructor
    public static enum OrderType {
        NewOrder("新订单"),
        abnormalOrder("异常订单"),
        urgeOrder("催单"),
        cancleOrder("退单");

        private String name;
    }
}
```

```
public class PCNotice {
    private String title;
    private String url;
}
```

```
public enum ShopBusinessStatus {
    OnBusiness("营业中"),
    AutoClosedOnTime("已打烊"),//若店铺处于营业时间到点后的自然关闭状态（含延时5分钟关店）
    OperateClosed("已关闭");//若店铺被手动设置为立即关闭
    private String name;
}
```

```
public class ShopHealthCheck {
    private Map<HealthCheckResult, List<Map<HealthCheckReason,ValueAndUrl>>> todayBusinessData;
    //对于已完成的项目,value设为1,未完成设为0,前端根据value值判断显示"去设置"还是"已完成"

    @NoArgsConstructor
    @AllArgsConstructor
    @Getter
    public static enum HealthCheckResult {
        excellent("优秀，请继续保持"),
        common("一般，请加油改进"),
        terrible("不佳，请尽快改进");

        private String name;
    }

    @NoArgsConstructor
    @AllArgsConstructor
    @Getter
    public static enum HealthCheckReason {
        foodNumCheck("菜品数量≥10"),
        businessTimeCheck("店铺营业时间不为0"),
        shopLogo("店铺有非默认的LOGO"),
        businessLicenseCheck("店铺已通过营业执照认证"),
        minDeliveryOrderFeeCheck("起送价≤50元"),
        deliveryFeeCheck("配送费≤10元");

        private String name;
    }
}
```

```
public class ShopNameId {
    private String shopName;
    private int shopId;
}
```

```
public class TodayBusinessData {
    private Map<BusinessShowType, OrderInfo> todayBusinessData;

    @Data
    class OrderInfo{
        private double todayOrders;
        private double yestdayOrders;
        private String url;
    }
    @NoArgsConstructor
    @AllArgsConstructor
    @Getter
    public static enum BusinessShowType {
        urgeOrder("订单"),
        cancleOrder("营业额");

        private String name;

    }
}
```

```
public class UnHandleFeedback {
    private Map<FeedbackType, ValueAndUrl> orderNotice;

    @NoArgsConstructor
    @AllArgsConstructor
    @Getter
    public static enum FeedbackType {
        urgeOrder("未回复差评"),
        cancleOrder("未回复评价");

        private String name;
    }
}
```

```
class ValueAndUrl{
    private int value;
    private String url;
}
```

