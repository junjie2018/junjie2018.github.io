## 操作步骤

1. File > Settings > Editor > CodeStyle

![2020-11-18-17-19-47](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2020-11-18-17-19-47.png)

2. 编写代码

~~~ java

// @formatter:on

this.sagaDefinition =
        step()
            .withCompensation(
                orderService.reject,
                CreateOrderSagaState::makeRejectOrderCommand)

        .step()
            .invokeParticipant(
                consumerService.validateOrder,
                CreateOrderSagaState::makeValidateOrderByConsumerCommand)
            .onReply(
                CreateTicketReply.class,
                CreateOrderSagaState::handleCreateTicketReply)
            .withCompensation(
                kitchenService.cancel,
                CreateOrderSagaState::makeConfirmCreateTicketCommand)

        .step()
            .invokeParticipant(
                    kitchenService.create,
                    CreateOrderSagaState::makeCreateTicketCommand)

        .step()
            .invokeParticipant(
                    accountingService.authorize,
                    CreateOrderSagaState::makeAuthorizeCommand)

        .step()
            .invokeParticipant(
                    kitchenService.confirmCreate,
                    CreateOrderSagaState::makeConfirmCreateTicketCommand)

        .step()
            .invokeParticipant(
                    orderService.approve,
                    CreateOrderSagaState::makeApproveOrderCommand)

        .build();

// @formatter:off

~~~

3. 如上代码，默认情况下使用`atrl + art + l`时，肯定会被格式化，但是启动了部分格式化后，则不会

## 参考资料

1. [IDEA(AS)代码格式化部分忽略](https://blog.csdn.net/Mislead/article/details/52130536)

2. [idea 代码部分格式化](https://blog.csdn.net/weixin_37194108/article/details/103893999)