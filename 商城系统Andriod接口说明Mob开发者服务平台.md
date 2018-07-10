---
title: 商城系统Andriod接口说明
date: 2018-03-15 10:54:04
categories: "开发"
tags:
	- 技术
	- JSON

---

![商城系统Andriod接口说明][Andriod]

Mob的ShopSDK让用户2小时快速集成商城系统，商品管理 — 订单交易 — 售后退款一整套解决方案，丰富您APP的应用场景。

# 接口说明 #

OperationCallback.java

OperationCallback是ShopSDK的回调接口，用于返回结果。

搜索:

    | 接口名  | 接口定义                   | 参数说明                                    | 备注 |
    | ---- | ---------------------- | --------------------------------------- | -- |
    | 成功回调 | +onSuccess(T data)     | data：泛型，成功时返回的数据                        |    |
    | 失败回调 | +onFailed(Throwable t) | t：失败时返回的Throwable对象（一般是ShopException实例） |    |

MobSDK.java

MobSDK是Mob产品公共类，ShopSDK需要使用其中的一些方法。

搜索:

    | 接口名      | 接口定义                                                                              | 参数说明                                                    | 备注                                                           |
    | -------- | --------------------------------------------------------------------------------- | ------------------------------------------------------- | ------------------------------------------------------------ |
    | Mob产品初始化 | init(Context context, String appkey, String appsecret)                            | context：上下文  appkey：Mob-AppKey  appsecret：Mob-AppSecret | 初始化方法，若不使用manifest中的meta-data定义appKey和appSecret，则可通过该方法进行初始化 |
    | 设置用户信息   | setUser(String id, String nickname, String avatar, HashMap<String, Object> extra) | id：用户ID  nickname：昵称  avatar：用户头像URL  extra：用户附加信息键值対   | 设置登录用户的信息，开发者需要将用户信息传给ShopSDK，以便区分用户，否则当前用户为匿名用户，大部分接口将被限制使用 |
    | 清除用户信息   | clearUser()                                                                       |                                                         | 清除当前用户信息，相当于登出功能，用户将变为匿名用户                                   |

ShopSDK.java

ShopSDK是核心接口定义，所有的接口都在该类中定义。

搜索:

    | 接口名                | 接口定义                                                                                                                                               | 参数说明                                                                                                   | 备注                                                                                          |
    | ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------- |
    | 获取产品列表             | getProducts(ProductQuerier productQuerier, OperationCallback<List<Product>> callback)                                                              | productQuerier：产品列表查询器  callback：回调                                                                    | 根据指定搜索条件检索商品列表                                                                              |
    | 获取产品列表             | getProducts(List<Long> productIds, TimeSort timeSort, PriceSort priceSort, SalesSort salesSort, OperationCallback<List<Product>> callback)         | productIds：产品Id列表  timeSort：时间排序  priceSort：价格排序  salesSort：销量排序  callback：回调                          | 根据产品Id列表查询相应的产品列表                                                                           |
    | 获取配送策略             | getTransportStrategy(OperationCallback<List<TransportStrategy>> callback)                                                                          | callback：回调                                                                                            | 获取配送策略，用于商品筛选                                                                               |
    | 获取产品标签             | getLabels(int size, OperationCallback<List<Label>> callback)                                                                                       | size：请求的标签数量  callback：回调                                                                              | 获取产品标签，用于商品筛选                                                                               |
    | 获取商品详情             | getCommodityDetails(long productId, OperationCallback<Product> callback)                                                                           | productId：产品ID  callback：回调                                                                            | 根据产品ID获取该产品下所有商品信息                                                                          |
    | 获取产品详情             | getProductDetail(long productId, OperationCallback<Product> callback)                                                                              | productId：产品ID  callback：回调                                                                            | 根据产品ID获取产品详情（该接口返回的数据同时包含了getCommodityDetails接口的数据）                                         |
    | 获取评论列表             | getComments(CommentQuerier commentQuerier, OperationCallback<CommentList> callback)                                                                | commentQuerier：商品评论查询器  callback：回调                                                                    | 获取产品评论列表                                                                                    |
    | 添加购物车              | addIntoShoppingCart(long commodityId, int count, OperationCallback callback)                                                                       | commodityId：待加入购物车的商品ID  count：数量  callback：回调                                                         | 向购物车中添加商品  说明：接口已实现了购物车的缓存与合并功能，开发者无需考虑                                                     |
    | 删除购物车条目            | deleteFromShoppingCart(List<Long> cartCommodityIdList, OperationCallback<Void> callback)                                                           | cartCommodityIdList：待删除的购物车商品ID列表  callback：回调                                                         | 从购物车中删除商品  说明：接口已实现了购物车的缓存与合并功能，开发者无需考虑                                                     |
    | 获取购物车条目            | getShoppingCartItems(OperationCallback<List<CartCommodity>> callback)                                                                              | callback：回调                                                                                            | 获取购物车中所有商品  说明：接口已实现了购物车的缓存与合并功能，开发者无需考虑                                                    |
    | 修改购物车条目数量          | modifyShoppingCartItemAmount(List<CartCommodityItem> cartCommodityItemList, OperationCallback<Void> callback)                                      | cartCommodityItemList：待修改的购物车商品列表  callback：回调                                                         | 修改购物车中商品数量  说明：接口已实现了购物车的缓存与合并功能，开发者无需考虑                                                    |
    | 预下单                | preOrder(PreOrderInfoBuilder preOrderInfoBuilder, OperationCallback<PreOrder> callback)                                                            | preOrderInfoBuilder：预订单信息创建器  callback：回调                                                              | 创建订单前的预操作，用于通过当前下单商品信息、地址、使用优惠券等确认最终订单信息，一般用于“订单确认”页面的数据显示  说明：调用createOrder接口前必须先调用该接口     |
    | 获取当前订单的可选优惠券列表     | getOrderRelatedCoupons(List<BaseBuyingItem> buyingItemList, OperationCallback<List<OrderCoupon>> callback)                                         | buyingItemList：欲购买项目列表  callback：回调                                                                    | 获取当前订单的可选优惠券列表                                                                              |
    | 获取订单自定义属性 列表       | getOrderCustomFields(OperationCallback<List<String>> callback)                                                                                     | callback：回调                                                                                            | 获取商家自定义的订单附加属性列表                                                                            |
    | 创建订单               | createOrder(OrderInfoBuilder orderInfoBuilder, OperationCallback<Order> callback)                                                                  | orderInfoBuilder：订单信息创建器  callback：回调                                                                  | 创建订单  说明：调用该接口前必须先调用preOrder接口获取预订单信息                                                       |
    | 订单支付               | payOrder(OrderPayBuilder orderPayBuilder, OperationCallback<PayResult> callback)                                                                   | orderPayBuilder：订单支付构建器  callback：回调                                                                   | 订单支付                                                                                        |
    | 获取收货地址列表           | getShippingAddrs(OperationCallback<List<ShippingAddr>> callback)                                                                                   | callback：回调                                                                                            | 获取用户收货地址列表                                                                                  |
    | 新建收货地址             | createShippingAddr(ShippingAddrBuilder shippingAddrBuilder, OperationCallback<ShippingAddr> callback)                                              | shippingAddrBuilder：收货地址创建器  callback：回调                                                               | 新建收货地址                                                                                      |
    | 修改收货地址             | modifyShippingAddr(long shippingAddrId, ShippingAddrBuilder shippingAddrBuilder, OperationCallback<ShippingAddr> callback)                         | shippingAddrId：目标收货地址ID  shippingAddrBuilder：收货地址创建器  callback：回调                                      | 修改收货地址                                                                                      |
    | 删除收货地址             | deleteShippingAddr(long shippingAddrId, OperationCallback<Void> callback)                                                                          | shippingAddrId：收货地址ID  callback：回调                                                                     | 删除收货地址                                                                                      |
    | 获取省市区信息            | getArea(OperationCallback<List<Province>> callback)                                                                                                | callback：回调                                                                                            | 获取省市区信息  说明：SDK内部已经实现了省市区列表的本地缓存功能，开发者无需考虑                                                  |
    | 获取订单状态信息           | getOrdersStatisticInfo(OperationCallback<List<OrderStatistic>> callback)                                                                           | callback：回调                                                                                            | 获取用户全部订单的状态统计结果，用于个人中心显示                                                                    |
    | 查询订单               | getOrders(OrderListQuerier orderListQuerier, OperationCallback<List<Order>> callback)                                                              | orderListQuerier：订单列表查询器（自定义查询器）  callback：回调                                                          | 获取用户所有订单列表                                                                                  |
    | 获取订单详情             | getOrderDetail(long orderId, OperationCallback<OrderDetail> callback)                                                                              | orderId：订单ID  callback：回调                                                                              | 获取订单详情                                                                                      |
    | 修改订单状态             | modifyOrderStatus(long orderId, OrderOperator orderOperator, OperationCallback<Void> callback)                                                     | orderId：订单ID  orderOperator：订单操作（枚举）                                                                   | 修改订单状态（取消、删除、确认收货）                                                                          |
    | 预申请退款              | preRefund(long orderCommodityId, OperationCallback<PreRefund> callback)                                                                            | orderCommodityId：订单商品ID  callback：回调                                                                   | 退款预申请  说明：调用refund接口前须先调用该接口                                                                |
    | 申请退款               | refund(RefundBuilder refundBuilder, OperationCallback<Long> callback)                                                                              | refundBuilder：退款构建器（自定义接口参数构建器）  callback：回调                                                           | 申请退款  说明：调用该接口前须先调用preRefund接口                                                              |
    | 查询物流信息             | queryExpress(String transportId, OperationCallback<ExpressInfo> callback)                                                                          | transportId：订单物流ID  callback：回调                                                                        | 查询物流信息                                                                                      |
    | 添加评论               | addComment(CommentBuilder commentBuilder, OperationCallback<Void> callback)                                                                        | commentBuilder：评论构建器（自定义接口参数构建器）  callback：回调                                                          | 添加商品评论                                                                                      |
    | 获取优惠券列表            | getCoupons(CouponQuerier couponQuerier, OperationCallback<List<Coupon>> callback)                                                                  | couponQuerier：优惠券查询器（自定义查询器）  callback：回调                                                              | 获取用户的优惠券列表                                                                                  |
    | 获取可领取优惠券列表         | getReceivableCoupons(ReceivableCouponQuerier receivableCouponQuerier, OperationCallback<ReceivableCoupons> callback)                               | receivableCouponQuerier：可领取优惠券查询器（自定义查询器）  callback：回调                                                 | 获取用户可领取的优惠券列表                                                                               |
    | 领取优惠券              | receiveCoupon(long couponId, OperationCallback<Void> callback)                                                                                     | couponId：优惠券ID  callback：回调                                                                            | 领取优惠券                                                                                       |
    | 获取退款售后列表           | getRefunds(RefundQuerier refundQuerier, OperationCallback<List<Refund>> callback)                                                                  | refundQuerier：退款售后查询器  callback：回调                                                                     | 获取退款售后列表                                                                                    |
    | 获取退款详情             | getRefundDetail(long orderCommodityId, OperationCallback<RefundDetail> callback)                                                                   | orderCommodityId：订单商品ID  callback：回调                                                                   | 获取退款详情                                                                                      |
    | 取消退款               | cancelRefund(long orderCommodityId, OperationCallback<Void> callback)                                                                              | orderCommodityId：订单商品ID  callback：回调                                                                   | 取消退款申请                                                                                      |
    | 填写退款物流             | fillInExpressInfo(long orderCommodityId, String transportId, long expressCompanyId, DeliveryType deliveryType, OperationCallback<Void> callback)   | orderCommodityId：订单商品ID  transportId：物流单号  expressCompanyId：物流公司代码  deliveryType：配送类型（枚举）  callback：回调 | 填写退款物流信息                                                                                    |
    | 获取物流公司列表           | getExpressCompanies(OperationCallback<List<ExpressCompany>> callback)                                                                              | callback：回调                                                                                            | 获取物流公司列表  说明：SDK内部已实现本地缓存功能，开发者无需考虑                                                         |
    | 上传图片               | uploadImage(String filePath, OperationCallback<String> callback)                                                                                   | filePath：文件路径  callback：回调（图片URL）                                                                      | 上传图片至ShopSDK提供的文件服务器并获取图片URL，该URL可用于“添加评论”和“退款申请”接口，开发者也可以使用自己的文件服务器                        |
    | 获取可选属性列表（辅助接口）     | getSelectableProperties(Product product, List<SelectedProperty> selectedPropertyList, OperationCallback<List<? extends ProductProperty>> callback) | product：通过“获取产品详情”接口获取到的product对象  selectedPropertyList：已选属性列表  callback：回调                            | 辅助接口，用于根据传入的当前已选择属性值，获取其余属性的可选值列表  使用场景：购买商品或添加商品至购物车前，用户需选择商品各属性                           |
    | 通过所选属性确认唯一商品（辅助接口） | specifyCommodityBySelectedProperties(Product product, List<SelectedProperty> selectedPropertyList, OperationCallback<Commodity> callback)          | product：通过“获取产品详情”接口获取到的product对象  selectedPropertyList：已选属性列表（必须包含该产品的所有属性，否则无法确定一个唯一商品）  callback：回调 | 辅助接口，用于根据传入的当前已选择属性值，确定一个唯一的商品  使用场景：购买商品或添加商品至购物车前，用户需选择商品各属性，当用户选定了所有属性后，可通过该接口确定出用户选定的商品 |
    | 获取当前用户             | getUser()                                                                                                                                          |                                                                                                        | 获取当前用户，该用户存储了开发者通过MobSDK.setUser()接口设置给ShopSDK的用户信息  说明：非必需接口，一般开发者不需要使用                    |
    | 绑定用户状态监听器          | bindUserWatcher(UserWatcher userWatcher)                                                                                                           | userWatcher：用户状态监听器                                                                                    | 绑定用户状态监听器，以实时监听用户状态，一旦ShopSDK内部用户状态发生变化，将触发监听器回调  说明：非必需接口，一般开发者不需要使用                       |
    | 解绑用户状态监听器          | unbindUserWatcher()                                                                                                                                |                                                                                                        | 解绑用户状态监听器，停止监听用户状态，若绑定过UserWatcher，请在适当的时机解绑UserWatcher  说明：非必需接口，一般开发者不需要使用                |

#  #

错误码说明

除极个别情况外，ShopSDK抛出的异常均为ShopException实例，其中存储了code和message信息。

ShopSDK的错误码列表，4XXXXXX和5XXXXXX为服务端返回给SDK的错误码，6XXXXXX为SDK内部错误码

搜索:

    | Code    | 说明                      |
    | ------- | ----------------------- |
    | 4113002 | 请求不存在的服务/服务地址不存在        |
    | 4113003 | 请求参数不完整                 |
    | 4113004 | JSON格式错误                |
    | 4113005 | 用户未登录（未设置用户信息，即当前是匿名用户） |
    | 5113001 | 服务器未知异常                 |
    | 5113201 | 不可以重复发起退款               |
    | 5113202 | 退款金额超出限制                |
    | 5113203 | 待发货状态商品选择了退款退货          |
    | 5113204 | 订单已经支付，不能修改价格           |
    | 5113205 | 调用支付网关失败，退款不成功          |
    | 5113206 | 订单没有完成，不能操作(评论)         |
    | 5113207 | 非关闭订单不允许删除              |
    | 5113208 | 商品不可用，或已经下架             |
    | 5113209 | 没有发现地址信息                |
    | 5113210 | 存在不可下单的商品               |
    | 5113211 | 获取支付Ticket错误            |
    | 5113212 | 下单金额不一致(下单接口)           |
    | 5113213 | 调用第三方物流查询数据错误           |
    | 5113214 | 调用第三方物流查询网络错误           |
    | 5113215 | 找不到优惠券数据                |
    | 5113216 | 报表类型错误                  |
    | 5113217 | 调用用户详情失败                |
    | 5113218 | 取消已经付款订单                |
    | 5113219 | 订单未关闭                   |
    | 5113220 | 订单未支付                   |
    | 5113221 | 订单状态不允许退换货              |
    | 5113242 | 此订单免单,不需要支付             |
    | 5113243 | 异常商品                    |
    | 6113000 | 缺少必要的参数                 |
    | 6113001 | 参数不合法                   |
    | 6113002 | 用户未登录（一般为网络状态不佳）        |
    | 6113003 | 服务器返回数据异常               |
    | 6113004 | 无法找到Mob支付SDK            |
    | 6113005 | 不支持的支付平台                |
    | 6113006 | 支付过程发生内部异常              |
    | 6113099 | 未知错误                    |

其中，\[4113005：用户未登录\]较为重要，建议开发者接收到该code时跳转至登录页面，待用户登录后，再调用MobSDK.setUser()接口将用户信息设置给ShopSDK。


[Andriod]: /pro/os/crawler/6BVA-MJZB-IYJA.jpg
 *  **原文作者：** Mob开发者服务平台
 *  **原文链接：** https://www.toutiao.com/item/6532999353378800142/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。