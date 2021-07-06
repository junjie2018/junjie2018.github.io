代码如下：

~~~ java

/**
 * 硬件校准信息上报
 */
@PostMapping("/client/calibration/report")
@PassToken
public ResponseVo<Object> clientCalibrationReport(
        @RequestHeader(value = "terminal") Integer terminal,
        @Valid @RequestBody ClientCalibrationReportRequest request) {

    clientCalibrationReportService.saveClientCalibrationReport(terminal, request);

    return ResponseVo.createSuccess();
}

~~~

## 参考资料

1. [java 获取HttpRequest Header 的几种方法](https://blog.csdn.net/AlbertFly/article/details/52679797)