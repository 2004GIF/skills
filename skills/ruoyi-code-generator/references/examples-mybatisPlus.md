# 代码生成示例（MyBatis-Plus 版）

本文档提供了基于 MyBatis-Plus 模板的代码生成完整示例，供 Agent 参考学习。

---

## 示例一：简单 CRUD（产品管理）

### 输入信息

```yaml
tableName: biz_product
tableComment: 产品管理
packageName: com.ruoyi.business
moduleName: business
businessName: product
author: ruoyi
tplCategory: crud

columns:
  - name: product_id
    type: bigint
    comment: 产品ID
    isPk: true
    isIncrement: true

  - name: product_name
    type: varchar(100)
    comment: 产品名称
    isRequired: true
    isQuery: true
    htmlType: input

  - name: product_code
    type: varchar(50)
    comment: 产品编码
    isRequired: true

  - name: price
    type: decimal(10,2)
    comment: 价格

  - name: status
    type: char(1)
    comment: 状态（0正常 1停用）
    dictType: sys_normal_disable
    htmlType: radio

  - name: create_time
    type: datetime
    comment: 创建时间
```

### 生成结果

#### 1. Product.java（实体类 — 带 MP 注解）

```java
package com.ruoyi.business.domain;

import java.math.BigDecimal;
import org.apache.commons.lang3.builder.ToStringBuilder;
import org.apache.commons.lang3.builder.ToStringStyle;
import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.annotation.TableId;
import com.baomidou.mybatisplus.annotation.TableName;
import com.ruoyi.common.annotation.Excel;
import com.ruoyi.common.core.domain.BaseEntity;

/**
 * 产品管理对象 biz_product
 *
 * @author ruoyi
 * @date 2026-01-27
 */
@TableName("biz_product")
public class Product extends BaseEntity
{
    private static final long serialVersionUID = 1L;

    /** 产品ID */
    @TableId(value = "product_id", type = IdType.AUTO)
    private Long productId;

    /** 产品名称 */
    @Excel(name = "产品名称")
    private String productName;

    /** 产品编码 */
    @Excel(name = "产品编码")
    private String productCode;

    /** 价格 */
    @Excel(name = "价格")
    private BigDecimal price;

    /** 状态（0正常 1停用） */
    @Excel(name = "状态", readConverterExp = "0=正常,1=停用")
    private String status;

    public void setProductId(Long productId)
    {
        this.productId = productId;
    }

    public Long getProductId()
    {
        return productId;
    }

    public void setProductName(String productName)
    {
        this.productName = productName;
    }

    public String getProductName()
    {
        return productName;
    }

    public void setProductCode(String productCode)
    {
        this.productCode = productCode;
    }

    public String getProductCode()
    {
        return productCode;
    }

    public void setPrice(BigDecimal price)
    {
        this.price = price;
    }

    public BigDecimal getPrice()
    {
        return price;
    }

    public void setStatus(String status)
    {
        this.status = status;
    }

    public String getStatus()
    {
        return status;
    }

    @Override
    public String toString() {
        return new ToStringBuilder(this,ToStringStyle.MULTI_LINE_STYLE)
            .append("productId", getProductId())
            .append("productName", getProductName())
            .append("productCode", getProductCode())
            .append("price", getPrice())
            .append("status", getStatus())
            .append("createTime", getCreateTime())
            .toString();
    }
}
```

#### 2. ProductMapper.java（Mapper 接口 — 继承 BaseMapper）

```java
package com.ruoyi.business.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.ruoyi.business.domain.Product;

/**
 * 产品管理Mapper接口
 *
 * @author ruoyi
 * @date 2026-01-27
 */
public interface ProductMapper extends BaseMapper<Product>
{
}
```

#### 3. IProductService.java（Service 接口 — 继承 IService）

```java
package com.ruoyi.business.service;

import java.util.List;
import com.baomidou.mybatisplus.extension.service.IService;
import com.ruoyi.business.domain.Product;

/**
 * 产品管理Service接口
 *
 * @author ruoyi
 * @date 2026-01-27
 */
public interface IProductService extends IService<Product>
{
    /**
     * 查询产品管理
     *
     * @param productId 产品管理主键
     * @return 产品管理
     */
    public Product selectProductByProductId(Long productId);

    /**
     * 查询产品管理列表
     *
     * @param product 产品管理
     * @return 产品管理集合
     */
    public List<Product> selectProductList(Product product);

    /**
     * 新增产品管理
     *
     * @param product 产品管理
     * @return 结果
     */
    public boolean insertProduct(Product product);

    /**
     * 修改产品管理
     *
     * @param product 产品管理
     * @return 结果
     */
    public boolean updateProduct(Product product);

    /**
     * 批量删除产品管理
     *
     * @param productIds 需要删除的产品管理主键集合
     * @return 结果
     */
    public boolean deleteProductByProductIds(Long[] productIds);

    /**
     * 删除产品管理信息
     *
     * @param productId 产品管理主键
     * @return 结果
     */
    public boolean deleteProductByProductId(Long productId);
}
```

#### 4. ProductServiceImpl.java（Service 实现 — 继承 ServiceImpl，内部调用 MP 方法）

```java
package com.ruoyi.business.service.impl;

import java.util.Arrays;
import java.util.List;
import com.ruoyi.common.utils.DateUtils;
import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;
import org.springframework.stereotype.Service;
import com.ruoyi.business.mapper.ProductMapper;
import com.ruoyi.business.domain.Product;
import com.ruoyi.business.service.IProductService;

/**
 * 产品管理Service业务层处理
 *
 * @author ruoyi
 * @date 2026-01-27
 */
@Service
public class ProductServiceImpl extends ServiceImpl<ProductMapper, Product> implements IProductService
{

    /**
     * 查询产品管理
     *
     * @param productId 产品管理主键
     * @return 产品管理
     */
    @Override
    public Product selectProductByProductId(Long productId)
    {
        return this.getById(productId);
    }

    /**
     * 查询产品管理列表
     *
     * @param product 产品管理
     * @return 产品管理
     */
    @Override
    public List<Product> selectProductList(Product product)
    {
        LambdaQueryWrapper<Product> wrapper = new LambdaQueryWrapper<>();
        wrapper.eq(product.getProductName() != null, Product::getProductName, product.getProductName());
        return this.list(wrapper);
    }

    /**
     * 新增产品管理
     *
     * @param product 产品管理
     * @return 结果
     */
    @Override
    public boolean insertProduct(Product product)
    {
        product.setCreateTime(DateUtils.getNowDate());
        return super.save(product);
    }

    /**
     * 修改产品管理
     *
     * @param product 产品管理
     * @return 结果
     */
    @Override
    public boolean updateProduct(Product product)
    {
        product.setUpdateTime(DateUtils.getNowDate());
        return super.updateById(product);
    }

    /**
     * 批量删除产品管理
     *
     * @param productIds 需要删除的产品管理主键
     * @return 结果
     */
    @Override
    public boolean deleteProductByProductIds(Long[] productIds)
    {
        return super.removeByIds(Arrays.asList(productIds));
    }

    /**
     * 删除产品管理信息
     *
     * @param productId 产品管理主键
     * @return 结果
     */
    @Override
    public boolean deleteProductByProductId(Long productId)
    {
        return super.removeById(productId);
    }
}
```

#### 5. ProductController.java（控制器 — 调用 Service 业务方法）

```java
package com.ruoyi.business.controller;

import java.util.List;
import javax.servlet.http.HttpServletResponse;
import org.springframework.security.access.prepost.PreAuthorize;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import com.ruoyi.common.annotation.Log;
import com.ruoyi.common.core.controller.BaseController;
import com.ruoyi.common.core.domain.AjaxResult;
import com.ruoyi.common.enums.BusinessType;
import com.ruoyi.business.domain.Product;
import com.ruoyi.business.service.IProductService;
import com.ruoyi.common.utils.poi.ExcelUtil;
import com.ruoyi.common.core.page.TableDataInfo;

/**
 * 产品管理Controller
 *
 * @author ruoyi
 * @date 2026-01-27
 */
@RestController
@RequestMapping("/business/product")
public class ProductController extends BaseController
{
    @Autowired
    private IProductService productService;

    /**
     * 查询产品管理列表
     */
    @PreAuthorize("@ss.hasPermi('business:product:list')")
    @GetMapping("/list")
    public TableDataInfo list(Product product)
    {
        startPage();
        List<Product> list = productService.selectProductList(product);
        return getDataTable(list);
    }

    /**
     * 导出产品管理列表
     */
    @PreAuthorize("@ss.hasPermi('business:product:export')")
    @Log(title = "产品管理", businessType = BusinessType.EXPORT)
    @PostMapping("/export")
    public void export(HttpServletResponse response, Product product)
    {
        List<Product> list = productService.selectProductList(product);
        ExcelUtil<Product> util = new ExcelUtil<Product>(Product.class);
        util.exportExcel(response, list, "产品管理数据");
    }

    /**
     * 获取产品管理详细信息
     */
    @PreAuthorize("@ss.hasPermi('business:product:query')")
    @GetMapping(value = "/{productId}")
    public AjaxResult getInfo(@PathVariable("productId") Long productId)
    {
        return success(productService.selectProductByProductId(productId));
    }

    /**
     * 新增产品管理
     */
    @PreAuthorize("@ss.hasPermi('business:product:add')")
    @Log(title = "产品管理", businessType = BusinessType.INSERT)
    @PostMapping
    public AjaxResult add(@RequestBody Product product)
    {
        return toAjax(productService.insertProduct(product));
    }

    /**
     * 修改产品管理
     */
    @PreAuthorize("@ss.hasPermi('business:product:edit')")
    @Log(title = "产品管理", businessType = BusinessType.UPDATE)
    @PutMapping
    public AjaxResult edit(@RequestBody Product product)
    {
        return toAjax(productService.updateProduct(product));
    }

    /**
     * 删除产品管理
     */
    @PreAuthorize("@ss.hasPermi('business:product:remove')")
    @Log(title = "产品管理", businessType = BusinessType.DELETE)
    @DeleteMapping("/{productIds}")
    public AjaxResult remove(@PathVariable Long[] productIds)
    {
        return toAjax(productService.deleteProductByProductIds(productIds));
    }
}
```

---

## 示例二：主子表（订单 + 订单明细）

### 输入信息

```yaml
tableName: biz_order
tableComment: 订单管理
packageName: com.ruoyi.business
moduleName: business
businessName: order
author: ruoyi
tplCategory: sub

columns:
  - name: order_id
    type: bigint
    comment: 订单ID
    isPk: true
    isIncrement: true

  - name: order_no
    type: varchar(50)
    comment: 订单号
    isRequired: true
    isQuery: true

  - name: customer_name
    type: varchar(100)
    comment: 客户名称
    isQuery: true

  - name: total_amount
    type: decimal(10,2)
    comment: 订单金额

  - name: status
    type: char(1)
    comment: 订单状态
    dictType: order_status

  - name: create_time
    type: datetime
    comment: 创建时间

subTable:
  tableName: biz_order_item
  functionName: 订单明细

  columns:
    - name: item_id
      type: bigint
      comment: 明细ID
      isPk: true
      isIncrement: true

    - name: order_id
      type: bigint
      comment: 订单ID

    - name: product_name
      type: varchar(100)
      comment: 产品名称

    - name: quantity
      type: int
      comment: 数量

    - name: unit_price
      type: decimal(10,2)
      comment: 单价
```

### 生成结果

#### 1. Order.java（主表实体）

```java
package com.ruoyi.business.domain;

import java.math.BigDecimal;
import java.util.List;
import org.apache.commons.lang3.builder.ToStringBuilder;
import org.apache.commons.lang3.builder.ToStringStyle;
import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.annotation.TableId;
import com.baomidou.mybatisplus.annotation.TableName;
import com.ruoyi.common.annotation.Excel;
import com.ruoyi.common.core.domain.BaseEntity;

/**
 * 订单管理对象 biz_order
 *
 * @author ruoyi
 * @date 2026-01-27
 */
@TableName("biz_order")
public class Order extends BaseEntity
{
    private static final long serialVersionUID = 1L;

    /** 订单ID */
    @TableId(value = "order_id", type = IdType.AUTO)
    private Long orderId;

    /** 订单号 */
    @Excel(name = "订单号")
    private String orderNo;

    /** 客户名称 */
    @Excel(name = "客户名称")
    private String customerName;

    /** 订单金额 */
    @Excel(name = "订单金额")
    private BigDecimal totalAmount;

    /** 订单状态 */
    @Excel(name = "订单状态", readConverterExp = "0=待支付,1=已支付,2=已发货,3=已完成,4=已取消")
    private String status;

    /** 订单明细信息 */
    private List<OrderItem> orderItemList;

    // getters and setters ...
}
```

#### 2. OrderMapper.java（主表 Mapper — 声明子表自定义方法）

```java
package com.ruoyi.business.mapper;

import java.util.List;
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.ruoyi.business.domain.Order;
import com.ruoyi.business.domain.OrderItem;

/**
 * 订单管理Mapper接口
 *
 * @author ruoyi
 * @date 2026-01-27
 */
public interface OrderMapper extends BaseMapper<Order>
{
    /**
     * 批量删除订单明细
     */
    public int deleteOrderItemByOrderIds(Long[] orderIds);

    /**
         * 批量新增订单明细
     */
    public int batchOrderItem(List<OrderItem> orderItemList);

    /**
     * 通过订单主键删除订单明细信息
     */
    public int deleteOrderItemByOrderId(Long orderId);
}
```

#### 3. IOrderService.java（Service 接口）

```java
package com.ruoyi.business.service;

import java.util.List;
import com.baomidou.mybatisplus.extension.service.IService;
import com.ruoyi.business.domain.Order;

public interface IOrderService extends IService<Order>
{
    public Order selectOrderByOrderId(Long orderId);

    public List<Order> selectOrderList(Order order);

    public boolean insertOrder(Order order);

    public boolean updateOrder(Order order);

    public boolean deleteOrderByOrderIds(Long[] orderIds);

    public boolean deleteOrderByOrderId(Long orderId);
}
```

#### 4. OrderServiceImpl.java（Service 实现 — 子表操作使用 baseMapper，主表操作使用 super）

```java
package com.ruoyi.business.service.impl;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import com.ruoyi.common.utils.DateUtils;
import com.ruoyi.common.utils.StringUtils;
import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import com.ruoyi.business.mapper.OrderMapper;
import com.ruoyi.business.domain.Order;
import com.ruoyi.business.domain.OrderItem;
import com.ruoyi.business.service.IOrderService;

@Service
public class OrderServiceImpl extends ServiceImpl<OrderMapper, Order> implements IOrderService
{

    @Override
    public Order selectOrderByOrderId(Long orderId)
    {
        return this.getById(orderId);
    }

    @Override
    public List<Order> selectOrderList(Order order)
    {
        LambdaQueryWrapper<Order> wrapper = new LambdaQueryWrapper<>();
        wrapper.eq(order.getOrderNo() != null, Order::getOrderNo, order.getOrderNo());
        wrapper.eq(order.getCustomerName() != null, Order::getCustomerName, order.getCustomerName());
        return this.list(wrapper);
    }

    @Override
    @Transactional
    public boolean insertOrder(Order order)
    {
        order.setCreateTime(DateUtils.getNowDate());
        boolean result = super.save(order);
        insertOrderItem(order);
        return result;
    }

    @Override
    @Transactional
    public boolean updateOrder(Order order)
    {
        order.setUpdateTime(DateUtils.getNowDate());
        this.baseMapper.deleteOrderItemByOrderId(order.getOrderId());
        insertOrderItem(order);
        return super.updateById(order);
    }

    @Override
    @Transactional
    public boolean deleteOrderByOrderIds(Long[] orderIds)
    {
        this.baseMapper.deleteOrderItemByOrderIds(orderIds);
        return super.removeByIds(Arrays.asList(orderIds));
    }

    @Override
    @Transactional
    public boolean deleteOrderByOrderId(Long orderId)
    {
        this.baseMapper.deleteOrderItemByOrderId(orderId);
        return super.removeById(orderId);
    }

    /**
     * 新增订单明细信息
     */
    public void insertOrderItem(Order order)
    {
        List<OrderItem> orderItemList = order.getOrderItemList();
        Long orderId = order.getOrderId();
        if (StringUtils.isNotNull(orderItemList))
        {
            List<OrderItem> list = new ArrayList<OrderItem>();
            for (OrderItem orderItem : orderItemList)
            {
                orderItem.setOrderId(orderId);
                list.add(orderItem);
            }
            if (list.size() > 0)
            {
                this.baseMapper.batchOrderItem(list);
            }
        }
    }
}
```

---

## 与标准模板的关键差异对比

| 层级 | 标准模板 | MyBatis-Plus 模板 |
|------|---------|-------------------|
| **Domain** | 无 MP 注解 | `@TableName` + `@TableId` |
| **Mapper** | 手动声明所有 CRUD 方法 | `extends BaseMapper<T>`，仅保留子表自定义方法 |
| **Service 接口** | 手动声明所有方法，返回 `int` | `extends IService<T>`，返回 `boolean` |
| **ServiceImpl** | 手动实现所有方法，调用 `mapper.xxx()` | `extends ServiceImpl<M, T>`，调用 `super.xxx()` |
| **Controller** | 调用 service 方法 | 一致（不直接依赖 MP API） |

### ServiceImpl 方法对照

| 业务方法 | 标准模板实现 | MyBatis-Plus 模板实现 |
|---------|-------------|----------------------|
| 查询单条 | `${className}Mapper.selectXxxById(id)` | `this.getById(id)` |
| 查询列表 | `${className}Mapper.selectXxxList(xxx)` | `LambdaQueryWrapper` + `this.list(wrapper)` |
| 新增 | `${className}Mapper.insertXxx(xxx)` 返回 int | `super.save(xxx)` 返回 boolean |
| 修改 | `${className}Mapper.updateXxx(xxx)` 返回 int | `super.updateById(xxx)` 返回 boolean |
| 批量删除 | `${className}Mapper.deleteXxxByIds(ids)` 返回 int | `super.removeByIds(Arrays.asList(ids))` 返回 boolean |
| 单条删除 | `${className}Mapper.deleteXxxById(id)` 返回 int | `super.removeById(id)` 返回 boolean |
| 子表批量新增 | `${className}Mapper.batchXxx(list)` | `this.baseMapper.batchXxx(list)`（一致，走自定义 XML） |
| 子表删除 | `${className}Mapper.deleteXxxByXxx(id)` | `this.baseMapper.deleteXxxByXxx(id)`（一致，走自定义 XML） |

---

## 注意事项

1. **分页兼容**：Controller 中 `startPage()` 来自若依 `BaseController`，若使用 MP 分页插件需将 `PageHelper` 替换为 `PaginationInnerInterceptor`
2. **主键策略**：自增主键使用 `IdType.AUTO`，非自增主键默认使用雪花算法（`IdType.ASSIGN_ID`）
3. **子表 Mapper**：子表的批量新增/删除仍走自定义 XML（MP 不提供跨表级联操作），因此在 Mapper 接口中保留声明并在 XML 中实现
4. **返回值**：Service 增删改方法统一返回 `boolean`，Controller 通过 `toAjax(boolean)` 适配
5. **`super.save()` 回填主键**：MyBatis-Plus 的 `save()` 执行后自动将生成的主键回填到实体对象，子表插入时可直接通过 getter 获取
