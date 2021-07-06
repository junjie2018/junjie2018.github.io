## 布尔类型

FreeMarker中不可以直接在渲染出布尔值，需要使用如下的语法：

~~~ tpl

${flag?c}

<#assign foo = true>
${foo?then('Y', 'N')}

<#assign foo2 = false>
${foo2?then('Y', 'N')}

<#assign x = 10>
<#assign y = 20>
${100 + (x > y)?then(x, y)}

~~~

## 日期类型

FreeMarker中不可以直接输出日期类型，需要使用如下的语法：

~~~ java

renderData.put("openingTime", new Date());

~~~

~~~ tpl

<#assign x = openingTime>
${openingTime?time}

<#assign openingTimeTime = openingTime?time>
${openingTimeTime}

<#-- 将datetime类型转成date和time -->
<#assign openingTimeDateTime = openingTime?datetime>
${openingTimeDateTime}
${openingTimeDateTime?date}
${openingTimeDateTime?time}

~~~

如果问号左边是字符串，那么这些内建函数将字符串转换成日期/时间/日期事件（实验中datetime并不好使）：

~~~ java

renderData.put("openingTimeTimeStr", "19:27:58");
renderData.put("openingTimeDateStr", "2021-6-23");
renderData.put("openingTimeDatetimeStr", "2021-6-23 19:27:58");

~~~

~~~ tpl

${openingTimeTimeStr?time}
${openingTimeDateStr?date}
${openingTimeDateTimeStr?datetime}

~~~

还可以使用?string：

~~~ java

renderData.put("openingTime", new java.sql.Time(123456789));
renderData.put("nextDiscountDay", new java.sql.Date(123456789));
renderData.put("lastUpdated", new java.sql.Timestamp(123456789));
renderData.put("lastUpdated2", new java.util.Date(123456789));

~~~

~~~ tpl

${openingTime?string.short}
${openingTime?string.medium}
${openingTime?string.long}
${openingTime?string.full}
${openingTime?string.xs}
${openingTime?string.iso}

${nextDiscountDay?string.short}
${nextDiscountDay?string.medium}
${nextDiscountDay?string.long}
${nextDiscountDay?string.full}
${nextDiscountDay?string.xs}
${nextDiscountDay?string.iso}

${lastUpdated?string.short}
${lastUpdated?string.medium}
${lastUpdated?string.long}
${lastUpdated?string.full}
${lastUpdated?string.medium_short} <#-- medium date, short time -->
${lastUpdated?string.xs}
${lastUpdated?string.iso}

<#-- SimpleDateFormat patterns: -->
${lastUpdated?string["dd.MM.yyyy, HH:mm"]}
${lastUpdated?string["EEEE, MMMM dd, yyyy, hh:mm a '('zzz')'"]}
${lastUpdated?string["EEE, MMM d, ''yy"]}
${lastUpdated?string.yyyy} <#-- Same as ${lastUpdated?string["yyyy"]} -->

<#-- Advanced ISO 8601-related formats: -->
${lastUpdated?string.iso_m_u}
${lastUpdated?string.xs_ms_nz}

~~~