我的代码如下：

Request:

~~~ java

@Data
public class ClientCalibrationReportRequest {

    /**
     * 上报内容详情
     */
    @NotNull
    private Detail detail;

    @Data
    public static class Detail {
        /**
         * 白平衡校准数据
         */
        @NotBlank
        private String wbs;

    }
}


~~~

Controller:

~~~ java

@PostMapping("/client/calibration/report")
public ResponseVo<Object> clientCalibrationReport(
        @RequestHeader(value = "terminal") Integer terminal,
        @Valid @RequestBody ClientCalibrationReportRequest request) {

    clientCalibrationReportService.saveClientCalibrationReport(terminal, request);

    return ResponseVo.createSuccess();
}

~~~

问题是这样的，我在测试时使用了如下的json：

~~~ json

{
    "detail": {
        // "wbs": "100",
    }
}

~~~

结果校验依旧通过了，这不符合我的设计，后来我在detail字段上加上了@Valid注解后，能够正常的进行校验。代码如下：

~~~ java

@Data
public class ClientCalibrationReportRequest {

    /**
     * 上报内容详情
     */
    @NotNull
    @Valid
    private Detail detail;

    @Data
    public static class Detail {
        /**
         * 白平衡校准数据
         */
        @NotBlank
        private String wbs;

    }
}


~~~

后来我尝试将controller方法中的@Valid移动到ClientCalibrationReportRequest类上，我期待实现统一的注解位置配置，但是很失望，这样是不能够达到我的目标的。这次实验中我使用的是`javax.validation.Valid`，但是我知道Spring有一个`org.springframework.validation.annotation.Validated`注解，我不确定该注解能否实现我的目标。

## 参考资料

1. [javax框架之@Valid对象嵌套的效验](https://blog.csdn.net/w15868676598/article/details/80930595)