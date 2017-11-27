### Mbatis批量插入代码
@Date 2016.11.13

> 使用Mbatis批量插入功能代码示例

* 在做批量插入时要注意细节、如有写错会有奇怪的异常抛出
* 有可能会出现异常 : Parameter ‘__frch_callRecord_0’ not found

```
@Insert('''<script>
        INSERT INTO xxx (
            prefix_number,
            serial_number,
            is_register,
            insert_date,
            update_date,
            all_number,
            merchant_id
        ) VALUES
        <foreach collection="list" item="item" index="index" separator="," >
            (
                #{item.prefixNumber},
                #{item.serialNumber},
                #{item.isRegister},
                now(),
                now(),
                #{item.allNumber},
                #{item.merchantId}
            )
        </foreach>
    </script>''')
```