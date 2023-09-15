### angular变化检测OnPush策略需要注意的几个问题
1. OnPush组件内部触发的事件（包括viewChild）会引起组件的一次markForCheck
2. Detached组件内部触发的事件不会引起组件的变化检测
3. OnPush组件的contentChild依然会在变化检测周期时进行自身的变化检测
4. OnPush进行markForCheck时，会以此对父组件进行markForCheck，一直到根组件
5. 最顶层的OnPush或detach组件会进行NgDoCheck，但是他们的viewChild组件不会进行NgDoCheck，contentChild组件反而会进行NgDoCheck
6. OnPush组件进行了markForCheck，那么他的viewChild就会进行NgDoCheck
7. 一个对angular变化检测进行尝试的网页链接如下：[EDU - Understand Angular Change Detection (Angular 8.x)](https://danielwiehl.github.io/edu-angular-change-detection/)
