#### 层叠规则

- 选择器优先级：`!important` > 内联样式 > id 选择器> 类选择器 > 元素选择器
- 当应用两条同级别的规则到一个元素的时候，写在后面的就是实际使用的规则

在计算选择器的权重时，不允许进行进位。也就是说，无论多少个类选择器的权重叠加，都不会超过一个 ID 选择器。

#### 属性选择器

| 选择器                | 描述                                                                                       |
| --------------------- | :----------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| `selector[attr]`      | 选择 selector 所匹配的元素，且该元素拥有 attr 属性。                                       |
| `selector[attr=val]`  | 选择 selector 所匹配的元素，且该元素 attr 属性为 val。                                     |
| `selector[attr~=val]` | 选择 selector 所匹配的元素，且该元素 attr 属性值具有多个空格分隔的值，其中一个值等于 val。 |
| `selector[attr*=val]` | 选择 selector 所匹配的元素，且该元素 attr 属性值的任意位置包含 val。                       |
| `selector[attr^=val]` | 选择 selector 所匹配的元素，且该元素 attr 属性值以 val 开头。                              |
| `selector[attr$=val]` | 选择 selector 所匹配的元素，且该元素 attr 属性值以 val 结尾。                              |
| `selector[attr        | =val]`                                                                                     | 选择 selector 所匹配的元素，且该元素 attr 属性值为 val 或 val-开头。 |

#### 关系选择器

子代选择器： `div > p`

后代选择器：`div p`

紧邻兄弟选择器： `p + img`

兄弟选择器： `p ~ img`

#### CSS 属性值：inherit initial unset revert

**inherit: ** 继承父元素的属性值。

**initial:** css 属性恢复为初始值，且去掉浏览器默认样式。

**unset:** 当前元素浏览器或用户设置的 CSS 忽略，然后如果是具有继承特性的 CSS，如`color`, 则使用继承值；如果是没有继承特性的 CSS 属性，如`background-color`, 则使用初始值。

**revert:** 可以重置 CSS 属性为浏览器默认的样式。
