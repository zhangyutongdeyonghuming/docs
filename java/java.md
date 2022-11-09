## BigDecimal

> 格式化固定两位

```java
BigDecimal value = BigDecimal.ZERO;
DecimalFormat decimalFormat = new DecimalFormat("0.00#");
String zeroVal = decimalFormat.format(value);
// -> 0.00
```