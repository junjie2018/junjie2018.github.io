案例代码如下：

~~~ java

Set<String> set1 = new HashSet<>();
Set<String> set2 = new HashSet<>();

# 交集
set1.retailAll(set2);

# 差集
set.removeAll(set2);

# 并集
set1.addAll(set2);

~~~

在项目中应用的代码一：

~~~ java

    @Override
    public void updateUserInfoInEnterprise(String tenantId, UpdateUserInfoInEnterpriseRequest request) {

        // 判断用户是否存在
        selectUserById(request.getUserId());

        // 判断传递的用户是否已和当前企业建立关联（获取关联表信息）
        List<AuthGroupUserRole> authGroupUserRoles =
                selectAuthGroupUserRoleByTenantIdAndUserId(tenantId, request.getUserId());

        // 处理企业内部组织架构更新
        boolean organizeUpdateFlag = StringUtils.isNotBlank(request.getOrganizeId())
                && !authGroupUserRoles.get(0).getOrganizeId().equals(request.getOrganizeId());
        String organizeIdValue = organizeUpdateFlag ?
                request.getOrganizeId() : authGroupUserRoles.get(0).getOrganizeId();

        // 处理企业内部角色信息更新
        Set<String> roleIdsInRequest = new HashSet<>(request.getRoleIds());
        Set<String> roleIdsInDb = authGroupUserRoles.stream()
                .map(AuthGroupUserRole::getRoleId)
                .collect(Collectors.toSet());

        Set<String> toInsert = new HashSet<>(roleIdsInRequest);
        Set<String> toUpdate = new HashSet<>(roleIdsInRequest);
        Set<String> toDelete = new HashSet<>(roleIdsInDb);

        // 请求中包含的roleId，而数据库中不包含的roleId，则为需要插入的roleId
        toInsert.removeAll(roleIdsInDb);

        // 数据库中包含的roleId，而请求中不包含的roleId，则为需要删除的roleId
        toDelete.removeAll(roleIdsInRequest);

        // 请求中和数据库中同时包含的roleId则为需要更新的Id
        toUpdate.retainAll(roleIdsInDb);

        if (toDelete.size() != 0) {
            deleteBatchIdsLogic(new ArrayList<>(toDelete));
        }

        if (toInsert.size() != 0) {
            insertAuthGroupUserRole(tenantId, request.getUserId(), organizeIdValue, new ArrayList<>(toInsert));
        }

        if (toUpdate.size() != 0 && organizeUpdateFlag) {

            LambdaQueryWrapper<AuthGroupUserRole> queryWrapperForAuthAppRole = new LambdaQueryWrapper<AuthGroupUserRole>()
                    .eq(AuthGroupUserRole::getOrgId, tenantId)
                    .eq(AuthGroupUserRole::getUserId, request.getUserId())
                    .eq(AuthGroupUserRole::getDelete, IsDelete.NOT_DELETE.getValue());

            AuthGroupUserRole authGroupUserRoleUpdate = AuthGroupUserRole.builder()
                    .organizeId(request.getOrganizeId())
                    .build();

            authGroupUserRoleMapper.update(authGroupUserRoleUpdate, queryWrapperForAuthAppRole);

        }

        // 通知到网关
        informGatewayUserRoleChange(Collections.singletonList(request.getUserId()), tenantId);
    }

~~~

在项目中应用的代码二：

~~~ java

    public void updatePdmProjMemberGroups(
            String userId, String tenantId, UpdatePdmProjMemberGroupsRequest request) {

        // todo 检查projectId是否存在

        Map<String, PdmProjMemberGroup> projectGroupIdToProjectMemberGroupMap =
                getProjectGroupIdToProjectMemberGroupMap(request.getProjectId(), tenantId);

        List<CreatePdmProjMemberGroupsRequest.PdmProjMemberGroupDTO> toAdd = new ArrayList<>();
        List<UpdatePdmProjMemberGroupsRequest.PdmProjMemberGroupDTO> toUpdate = new ArrayList<>();
        List<String> toDelete = new ArrayList<>(projectGroupIdToProjectMemberGroupMap.keySet());

        // 如果传递的请求中包含Id则进行更新， 否则进行添加
        for (UpdatePdmProjMemberGroupsRequest.PdmProjMemberGroupDTO groupInfo : request.getGroupInfos()) {
            if (StringUtils.isBlank(groupInfo.getProjectGroupId())) {
                // 完成参数转换，方便接下来复用已有方法
                toAdd.add(BeanUtil.copyProperties(groupInfo,
                        CreatePdmProjMemberGroupsRequest.PdmProjMemberGroupDTO.class));
            } else {
                // 查看用户传递的Id是否已经在数据库中存在
                if (!projectGroupIdToProjectMemberGroupMap.containsKey(groupInfo.getProjectGroupId())) {
                    throw new RuntimeException("Wrong");
                }
                toUpdate.add(groupInfo);
                toDelete.remove(groupInfo.getProjectGroupId());
            }
        }

        // 新增
        if (toAdd.size() != 0) {
            createPdmProjMemberGroups(userId, tenantId, request.getProjectId(), toAdd);
        }

        // 删除
        if (toDelete.size() != 0) {
            // 删除成员信息
            deleteProjectMembersByProjectIdAndTenantId(
                    request.getProjectId(), toDelete, tenantId);
            // 删除分组信息
            deleteProjectMemberGroupsByProjectIdAndTenantId(
                    request.getProjectId(), toDelete, tenantId);
        }

        // 修改（成员信息直接删除重建，分组信息进行更新）
        if (toUpdate.size() != 0) {
            deleteProjectMembersByProjectIdAndTenantId(
                    request.getProjectId(),
                    toUpdate.stream()
                            .map(UpdatePdmProjMemberGroupsRequest.PdmProjMemberGroupDTO::getProjectGroupId)
                            .collect(Collectors.toList()),
                    tenantId);

            for (UpdatePdmProjMemberGroupsRequest.PdmProjMemberGroupDTO groupInfo : toUpdate) {
                PdmProjMemberGroup pdmProjMemberGroup =
                        projectGroupIdToProjectMemberGroupMap.get(groupInfo.getProjectGroupId());

                PdmProjMemberGroup pdmProjMemberGroupUpdate = PdmProjMemberGroup.builder()
                        .id(pdmProjMemberGroup.getId())
                        .modifier(userId)
                        .gmtModifyTime(LocalDateTime.now())
                        .name((JSONObject) JSONObject.toJSON(groupInfo.getName()))
                        .build();

                pdmProjMemberGroupMapper.updateById(pdmProjMemberGroupUpdate);

                // 重新插入成员信息
                createProjectMembersBatch(userId, tenantId,
                        request.getProjectId(), groupInfo.getProjectGroupId(),
                        groupInfo.getUserIds());
            }
        }
    }

~~~

## 参考资料

1. [Java ArrayList retainAll() 方法](https://www.runoob.com/java/java-arraylist-retainall.html)