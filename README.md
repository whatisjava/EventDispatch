### View的事件传递机制

```flow
st=>start: Start
op=>operation: Your Operation
cond=>condition: Yes or No?
e=>end
st->op->cond
cond(yes)->e
cond(no)->op
```
从上面的流程图可以得出如下结论：
* 触摸事件的传递流程是从dispatchTouchEvent开始的，如果不进行人为干预（也就是默认返回父类的同名函数），则事件将会
依照嵌套层次从外层向内层传递，到达最内层的View时，就由它的onTouchEvent方法处理，该方法如果能够消费该事件，则返回
true，如果处理不了，则返回false，此时事件将会重新向外层传递，并由外层View的onTouchEvent方法进行处理，依此类推。
* 如果事件在向内层传递的过程中由于人为干预，事件处理函数返回true，则会导致事件提前被消费掉，内层View将不会收到此事件。
* View控件的事件触发顺序是先执行onTouch方法，在最后才执行onClick方法。如果onTouch返回true，则事件不会继续传递，
最后也不会调用onClick方法；如果onTouch返回false，则事件继续传递，最后调用onClick方法。

### ViewGroup的事件传递机制

```flow
st=>start: Start
op=>operation: Your Operation
cond=>condition: Yes or No?
e=>end
st->op->cond
cond(yes)->e
cond(no)->op
```
从上面的流程图可以得出如下结论：
* 触摸事件的传递顺序是由Activity到ViewGroup，再由ViewGroup递归传递给它的子View。
* ViewGroup通过onInterceptTouchEvent方法对事件进行拦截，如果该方法返回true，则事件不会继续传递给它的子View，
如果返回false或者super.onInterceptTouchEvent，则事件会继续传递给它的子View。
* 在子View中对事件进行消费后，ViewGroup将接收不到任何事件。