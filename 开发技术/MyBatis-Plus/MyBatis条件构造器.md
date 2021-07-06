我目前比较中意的写法：

~~~ java

        // 查询思路：table.id = fieldId and (table.orgId = orgId or table.orgId = 超管orgId)
        LambdaQueryWrapper<FieldPo> queryWrapper = new LambdaQueryWrapper<FieldPo>()
                .eq(FieldPo::getId, fieldId)
                .and(queryWrapperInner -> queryWrapperInner
                        .eq(FieldPo::getOrgId, orgId)
                        .or()
                        .eq(FieldPo::getOrgId, SystemAdmin.ORG_ID.getOrgId()));

        FieldPo fieldPo = fieldDao.selectOne(queryWrapper);

        if (fieldPo == null) {
            throw new ApiException(
                    ExceptionCode.FIELD_NOT_FOUND.getCode(),
                    ExceptionCode.FIELD_NOT_FOUND.getMsg());
        }

        return fieldPo;

~~~

## 参考资料

1. [Mybatis Plus中的lambdaQueryWrapper条件构造图介绍](https://www.cnblogs.com/javagg/p/12654305.html)