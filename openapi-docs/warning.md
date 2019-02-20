# 注意事项 #

## 级联删除 ##

当试图删除一些数据的时候，一定要事先了解此数据（实体）所被关联或引用的对象，因为在 WiseCRM365 系统中，所有的删除操作都会涉及到级联关系，例如当一条订单数据被删除时，订单下的明细记录也会被一并删除。

### 主记录明细记录
当一条主记录被删除时，其下的明细记录也会被一并删除。

### 关联或引用
在删除记录的同时，与之相关联的数据是否也一并删除。例如在订单中会有所属客户（字段），当订单所引用的客户被删除时，订单是否也需要一并删除。默认情况下，删除操作仅仅会删除记录本身，如果希望删除关联记录，则需在删除接口中指定 `cascade_delete` 参数。

### 如何使用 cascade_delete 参数
多数删除接口都支持 `cascade_delete` 参数，此参数类似 `map<string, array>` 结构。以下将通过一个示例来说明具体用法。
<pre>
<em>// cascade_delete 参数</em>
{ "SalesOrder": [ "accountId" ] }
</pre>

上例中，在删除客户的同时，指定删除引用此客户的订单记录，其中 accountId 是 SalesOrder 中的字段，此字段引用自客户。

## 主键/引用（ID）字段 ##

与一般系统中采用自增式数字主键不同，WiseCRM365 使用 40 位长度的哈希字符串作为 ID 值，如 `003-e7cb7ac6-9ccd-437c-a803-a0ff90bfd78d`，需要注意的是前 3 位为实体代码（可通过元数据接口获取），这也就意味着通过一个 ID 值，我们可以清晰的识别出其是属于哪一个实体。