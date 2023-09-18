### fill-available,min-content,max-content,fit-content的作用机制

fill-available:宽度由外部元素决定（div）<br>
min-content:宽度由内部元素宽度缩小到最小的最大内部元素宽度决定<br>
max-content:宽度由内部元素宽度扩大到最大后的最大内部元素宽度决定<br>
fit-content:（inline-block）

先取max-content和min-content的宽度，再取父元素的宽度，设为pwidth。(max-content>=min-content)

若pwidth>max-content，则宽度是max-content。

若pwidth<min-content，则宽度是min-content。

若min-content<pwidth<max-content，则宽度是pwidth。
