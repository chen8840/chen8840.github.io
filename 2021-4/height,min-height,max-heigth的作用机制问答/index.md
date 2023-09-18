### height,min-height,max-heigth的作用机制问答
<span style="color: red">1.</span>min-height和height同时存在，子元素高度100%，以哪个高度为准？

答：min-height

<span style="color: red">2.</span>height存在，子元素高度100%，子元素内容高度大于100%，子元素高度为多少？

答：高度为父元素高度100%

<span style="color: red">3.</span>min-height 和 height 同时存在且相等，子元素高度大于min-height，子元素高度为多少？

答：为 min-height

<span style="color: red">4.</span>height不存在，min-height存在，子元素高度100%，此时子元素高度为多少？

答：0

<span style="color: red">5.</span>height不存在，min-height，max-height都存在，子元素高度100%，此时子元素高度为多少？

答：0

<span style="color: red">6.</span>上面的例子，怎么样能让子元素的高度和父元素min-height的高度相同？如果可以，子元素不设height而设min-height是什么效果？

答：第1个问题见[使min-height子元素height百分比生效的2种方式](../使min-height子元素height百分比生效的2种方式/)，第2个问题，子元素高度可以突破父元素高度

<span style="color: red">7.</span>height 100%有作用的前提是什么？

答：其父元素的高度是确定的。

<span style="color: red">8.</span>height，min-height，max-height的作用顺序是什么？

答：若min-height大于height，以min-height为准，若max-height小于height，以max-height为准，若min-height大于max-height，以min-height为准，否则以height为准。min-height > max-height > height。



width和height设百分比的区别是什么？
width:fit-content相当于height:unset?
width:auto相当于height:100%?
