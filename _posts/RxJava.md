# ReactiveX For Android Project

[TOC]
## Documents
* [101 ReactiveX Samples](http://rxwiki.wikidot.com/101samples#toc34)
* [RxJava Github](https://github.com/ReactiveX/RxJava)
* [RxJava Wiki](https://github.com/ReactiveX/RxJava/wiki)
* [RxJava 中文文档](https://mcxiaoke.gitbooks.io/rxdocs/content/)
* [RxKotlin](https://github.com/ReactiveX/RxKotlin)
* [RxAndroid](https://github.com/ReactiveX/RxAndroid)

## Release Version
|Respository|Release Version|
|:-|:-:|
|[RxJva Maven 3.1.6](https://central.sonatype.com/artifact/io.reactivex.rxjava3/rxjava/3.1.6)|[3.1.6](https://github.com/ReactiveX/RxJava/tags)|
|[RxAndroid Maven 3.0.2](https://central.sonatype.com/artifact/io.reactivex.rxjava3/rxandroid/3.0.2)|[3.0.2](https://github.com/ReactiveX/RxAndroid/releases)|
|[RxKotlin Maven 3.0.1](https://central.sonatype.com/artifact/io.reactivex.rxjava3/rxkotlin/3.0.1)|[3.0.1](https://github.com/ReactiveX/RxKotlin/releases)|

## Binaries
Binaries and dependency information for Maven, Ivy, Gradle and others can be found at [ReactX Maven](https://central.sonatype.com/artifact/io.reactivex.rxjava3/rxjava/3.1.6).

Example for Gradle:
```groovy
implementation 'io.reactivex.rxjava3:rxjava:3.1.6'
implementation 'io.reactivex.rxjava3:rxandroid:3.0.2'
implementation 'io.reactivex.rxjava3:rxkotlin:3.0.1'
```
## The Operators of ReactiveX

### Create Observables

#### Create - [create an Observable from scratch by calling observer methods programmatically](https://reactivex.io/documentation/operators/create.html)

> You can create an Observable from scratch by using the Create operator. You pass this operator a function that accepts the observer as its parameter. Write this function so that it behaves as an Observable — by calling the observer’s onNext, onError, and onCompleted methods appropriately.<br>
> A well-formed finite Observable must attempt to call either the observer’s onCompleted method exactly once or its onError method exactly once, and must not thereafter attempt to call any of the observer’s other methods.

> 您可以使用 Create 运算符从头开始创建一个 Observable。您向此运算符传递一个接受观察者作为其参数的函数。编写这个函数，使其表现得像一个 Observable — 通过适当地调用观察者的 onNext、onError 和 onCompleted 方法。
>
> 一个结构良好的有限 Observable 必须尝试调用观察者的 onCompleted 方法恰好一次或它的 onError 方法恰好一次，并且此后不得尝试调用观察者的任何其他方法。

![create](https://reactivex.io/documentation/operators/images/create.c.png)

```kotlin
    Observable.create { emitter ->
            val message = "Hello Word!"
            emitter.onNext(message)
            emitter.onComplete()
            log("Create function emit-> $message")
        }
            .subscribe(
                /* onNext = */ { result ->
                    log("onNext-> $result")
                },
                /* onError = */ {
                    log("onError")
                },
                /* onComplete = */ {
                    log("onComplete-> complete.")
                })
```

> 17:59:40.300 RxJava-main                          D  onNext-> Hello Word!
> 17:59:40.300 RxJava-main                          D  onComplete-> complete.
> 17:59:40.300 RxJava-main                          D  Create function emit-> Hello Word!


#### Defer — [do not create the Observable until the observer subscribes, and create a fresh Observable for each observer](https://reactivex.io/documentation/operators/defer.html)
  
> The Defer operator waits until an observer subscribes to it, and then it generates an Observable, typically with an Observable factory function. It does this afresh for each subscriber, so although each subscriber may think it is subscribing to the same Observable, in fact each subscriber gets its own individual sequence.<br>
> In some circumstances, waiting until the last minute (that is, until subscription time) to generate the Observable can ensure that this Observable contains the freshest data.
>
> Defer 运算符等待观察者订阅它，然后它生成一个 Observable，通常带有一个 Observable 工厂函数。它为每个订阅者重新执行此操作，因此尽管每个订阅者可能认为它订阅了相同的 Observable，但实际上每个订阅者都获得了自己的单独序列。
>
> 在某些情况下，等到最后一分钟（也就是订阅时间）再生成 Observable 可以保证这个 Observable 包含的是最新的数据。

![defer](https://reactivex.io/documentation/operators/images/defer.c.png)

   

#### Empty - [create an Observable that emits no items but terminates normally, the subscriber invoke `onComplete()`.](https://reactivex.io/documentation/operators/empty-never-throw.html)

> The Empty, Never, and Throw operators generate Observables with very specific and limited behavior. These are useful for testing purposes, and sometimes also for combining with other Observables or as parameters to operators that expect other Observables as parameters.
>
> Empty、Never 和 Throw 运算符生成具有非常具体和有限行为的 Observable。这些对于测试目的很有用，有时也用于与其他 Observable 结合或作为期望其他 Observable 作为参数的运算符的参数。

![empty](https://reactivex.io/documentation/operators/images/empty.c.png)




#### Never - [create an Observable that emits no items and does not terminate, the subscriber will not invoke any method.](https://reactivex.io/documentation/operators/empty-never-throw.html)

![never](https://reactivex.io/documentation/operators/images/never.c.png)


#### Throw - [create an Observable that emits no items and terminates with an error. the subscriber invoke `onError()`](https://reactivex.io/documentation/operators/empty-never-throw.html)

![throw](https://reactivex.io/documentation/operators/images/throw.c.png)

#### From — [convert various other objects and data types into Observables.](https://reactivex.io/documentation/operators/from.html)

> When you work with Observables, it can be more convenient if all of the data you mean to work with can be represented as Observables, rather than as a mixture of Observables and other types. This allows you to use a single set of operators to govern the entire lifespan of the data stream.
>
> Iterables, for example, can be thought of as a sort of synchronous Observable; Futures, as a sort of Observable that always emits only a single item. By explicitly converting such objects to Observables, you allow them to interact as peers with other Observables.
>
> For this reason, most ReactiveX implementations have methods that allow you to convert certain language-specific objects and data structures into Observables.
>
>当您使用 Observables 时，如果您要使用的所有数据都可以表示为 Observables，而不是 Observables 和其他类型的混合体，那么会更方便。这允许您使用一组运算符来管理数据流的整个生命周期。
>
>例如，Iterables 可以被认为是一种同步的 Observable； Futures，作为一种 Observable，总是只发出一个项目。通过明确地将此类对象转换为 Observables，您允许它们作为对等体与其他 Observables 进行交互。
>
>出于这个原因，大多数 ReactiveX 实现都有允许您将特定语言的对象和数据结构转换为 Observables 的方法。

![from](https://reactivex.io/documentation/operators/images/from.c.png)



#### Interval — [create an Observable that emits a sequence of integers spaced by a particular time interval](https://reactivex.io/documentation/operators/interval.html)

> The Interval operator returns an Observable that emits an infinite sequence of ascending integers, with a constant interval of time of your choosing between emissions.
>
> Interval 运算符返回一个 Observable，该 Observable 发出无限的升序整数序列，在发射之间具有您选择的恒定时间间隔。

![interval](https://reactivex.io/documentation/operators/images/interval.c.png)



#### Just — [convert an object or a set of objects into an Observable that emits that or those objects.](https://reactivex.io/documentation/operators/just.html)

> The Just operator converts an item into an Observable that emits that item.
>
> Just is similar to From, but note that From will dive into an array or an iterable or something of that sort to pull out items to emit, while Just will simply emit the array or iterable or what-have-you as it is, unchanged, as a single item.
>
> Note that if you pass null to Just, it will return an Observable that emits null as an item. Do not make the mistake of assuming that this will return an empty Observable (one that emits no items at all). For that, you will need the Empty operator.
>
> Just 运算符将一个项目转换为一个发射该项目的 Observable。
>
> Just 与 From 类似，但请注意 From 将深入到数组或可迭代对象或类似的东西中以提取要发射的项目，而 Just 将简单地发射数组或可迭代对象或你拥有的东西，不变, 作为单个项目。
>
> 请注意，如果您将 null 传递给 Just，它将返回一个将 null 作为项目发出的 Observable。不要错误地假设这将返回一个空的 Observable（一个根本不发射任何项目的对象）。为此，您将需要 Empty 运算符。

![just](https://reactivex.io/documentation/operators/images/just.c.png)



#### Range — [create an Observable that emits a range of sequential integers](https://reactivex.io/documentation/operators/range.html)

> The Range operator emits a range of sequential integers, in order, where you select the start of the range and its length.
>
> Range 运算符按顺序发出一系列连续整数，您可以在其中选择范围的起点及其长度。

![rang](https://reactivex.io/documentation/operators/images/range.c.png)


#### Repeat — [create an Observable that emits a particular item or sequence of items repeatedly](https://reactivex.io/documentation/operators/repeat.html)

> The Repeat operator emits an item repeatedly. Some implementations of this operator allow you to repeat a sequence of items, and some permit you to limit the number of repetitions.
>
> Repeat 运算符重复发出一个项目。此运算符的某些实现允许您重复一系列项目，而某些实现允许您限制重复的次数。


![repeat](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/repeatCount.o.png)

```kotlin
  // The repeat methos will repeat the whole subscription. 
  var times = 0
  Observable.just(times)
      .doOnNext { log("up stream.") }
      .repeat(2)
      .doOnNext { log("down stream") }
      .map { ++times }
      .subscribe { log("Current times is $it") }
```

> 17:21:06.070 RxJava-main                D  up stream.
> 17:21:06.070 RxJava-main                D  down stream
> 17:21:06.070 RxJava-main                D  Current times is 1
> 17:21:06.070 RxJava-main                D  up stream.
> 17:21:06.070 RxJava-main                D  down stream
> 17:21:06.070 RxJava-main                D  Current times is 2

#### ~~Start — [create an Observable that emits the return value of a function](https://reactivex.io/documentation/operators/start.html)~~

> NOTE: this create oprator NOT in RxJava.
>
> [There are a number of ways that programming languages have for obtaining values as the result of calculations, with names like functions, futures, actions, callables, runnables, and so forth. The operators grouped here under the Start operator category make these things behave like Observables so that they can be chained with other Observables in an Observable cascade](https://reactivex.io/documentation/operators/start.html)
>
> 编程语言有多种方法来获取计算结果的值，名称包括函数、期货、动作、可调用对象、可运行对象等。这里分组在 Start 运算符类别下的运算符使这些东西表现得像 Observables，以便它们可以与 Observable 级联中的其他 Observables 链接

![start](https://reactivex.io/documentation/operators/images/start.c.png)

#### Timer — [create an Observable that emits a single item after a given delay](https://reactivex.io/documentation/operators/timer.html)
> The Timer operator creates an Observable that emits one particular item after a span of time that you specify.
>
> Timer 运算符创建一个 Observable，它在您指定的时间跨度后发出一个特定的项目。

![timer](https://reactivex.io/documentation/operators/images/timer.c.png)

```kotlin
        Observable.timer(/* delay = */ 5, /* unit = */TimeUnit.SECONDS, /* scheduler = */Schedulers.computation())
            .doOnSubscribe {
                log("Timer start time is ${getCurrentTime()}")
            }
            .doFinally {
                log("Timer end time is ${getCurrentTime()}")
            }
            .subscribe { result ->
                log("onSubscribe-> result is $result, time is ${getCurrentTime()}")
            }
```

> 17:48:56.201 RxJava main D:  Timer start time is 17:48:56
> 17:49:01.207 RxJava RxComputationThreadPool-1   D:  onSubscribe-> result is 0, time is 17:49:01
> 17:49:01.211 RxJava RxComputationThreadPool-1   D:  Timer end time is 17:49:01


### Transforming Observables

#### Buffer — [periodically gather items from an Observable into bundles and emit these bundles rather than emitting the items one at a time.](https://reactivex.io/documentation/operators/buffer.html)
> The Buffer operator transforms an Observable that emits items into an Observable that emits buffered collections of those items. There are a number of variants in the various language-specific implementations of Buffer that differ in how they choose which items go in which buffers.
>
> Note that if the source Observable issues an onError notification, Buffer will pass on this notification immediately without first emitting the buffer it is in the process of assembling, even if that buffer contains items that were emitted by the source Observable before it issued the error notification.
>
> The Window operator is similar to Buffer but collects items into separate Observables rather than into data structures before reemitting them.

> Buffer 运算符将一个发射项目的 Observable 转换为一个发射这些项目的缓冲集合的 Observable。在 Buffer 的各种特定于语言的实现中有许多变体，它们在如何选择哪些项目进入哪些缓冲区方面有所不同。
>
> 请注意，如果源 Observable 发出 onError 通知，Buffer 将立即传递此通知，而不会首先发出它正在组装的缓冲区，即使该缓冲区包含源 Observable 在发出错误通知之前发出的项目.
>
>Window 运算符类似于 Buffer，但在重新发送它们之前将项目收集到单独的 Observables 而不是数据结构中。

![buffer](https://reactivex.io/documentation/operators/images/Buffer.png)

```kotlin
    private fun rxBufferTest() {
        Observable.range(0, 10)
            .buffer(5)
            .subscribe { result ->
                log("onNext-> $result")
            }
    }
```
> 10:19:01.452 RxJava-main                          D  onNext-> [0, 1, 2, 3, 4]
> 10:19:01.452 RxJava-main                          D  onNext-> [5, 6, 7, 8, 9]   

```kotlin
    private fun rxBufferSourceErrorTest() {
        Observable.range(0, 10)
            .switchMap { value ->
                if (value == 6) {
                    Observable.error(Throwable("source error!"))
                } else {
                    Observable.just(value)
                }
            }
            .buffer(5)
            .subscribe({ result ->
                log("onNext-> $result")
            }, { error ->
                log("Buffer error, ${error.message}")
            })
    }
```
> 10:24:03.454 RxJava-main                          D  onNext-> [0, 1, 2, 3, 4]
> 10:24:03.455 RxJava-main                          D  Buffer error, source error!

#### FlatMap — [transform the items emitted by an Observable into Observables, then flatten the emissions from those into a single Observable](https://reactivex.io/documentation/operators/flatmap.html)

> The FlatMap operator transforms an Observable by applying a function that you specify to each item emitted by the source Observable, where that function returns an Observable that itself emits items. FlatMap then merges the emissions of these resulting Observables, emitting these merged results as its own sequence.
>
> This method is useful, for example, when you have an Observable that emits a series of items that themselves have Observable members or are in other ways transformable into Observables, so that you can create a new Observable that emits the complete collection of items emitted by the sub-Observables of these items.
>
> Note that FlatMap merges the emissions of these Observables, so that they may interleave.
>
> In several of the language-specific implementations there is also an operator that does not interleave the emissions from the transformed Observables, but instead emits these emissions in strict order, often called ConcatMap or something similar.

> FlatMap 运算符通过将您指定的函数应用于源 Observable 发射的每个项目来转换 Observable，其中该函数返回一个本身发射项目的 Observable。 FlatMap 然后合并这些生成的 Observable 的发射，将这些合并的结果作为它自己的序列发射。
>
> 这个方法很有用，例如，当你有一个 Observable 发射一系列本身具有 Observable 成员或以其他方式转换为 Observables 的项目时，这样你就可以创建一个新的 Observable 发射由发射的项目的完整集合这些项目的子 Observables。
>
> 请注意，FlatMap 合并了这些 Observable 的发射，因此它们可以交错。
>
> 在一些特定于语言的实现中，还有一个运算符不会交错转换后的 Observable 的发射，而是按照严格的顺序发射这些发射，通常称为 ConcatMap 或类似的东西。

![flatmap](https://reactivex.io/documentation/operators/images/flatMap.c.png)

```kotlin
    // 这个例子不好， 没有体现 flat map convert 后数据有交错现象。
    private fun rxFlatmapTest() {
        Observable.range(0, 10)
            .flatMap { source ->
                Observable.just("merge function result $source")
                    .delay((10 - source).toLong(), TimeUnit.SECONDS)
            }
            .subscribe {
                log(it)
            }
    }
```

#### ConcatMap
> Returns a new {@code Observable} that emits items resulting from applying a function that you supply to each item emitted by the current {@code Observable}, where that function returns an {@link ObservableSource}, and then emitting the items that result from concatenating those returned {@code ObservableSource}s.
>
> Note that there is no guarantee where the given {@code mapper} function will be executed; it could be on the subscribing thread,on the upstream thread signaling the new item to be mapped or on the thread where the inner source terminates. To ensure the {@code mapper} function is confined to a known thread, use the {@link concatMap(Function, int, Scheduler)} overload.

> 返回一个新的 {@code Observable}，它发出的项目是将您提供的函数应用于当前 {@code Observable} 发出的每个项目，其中该函数返回一个 {@link ObservableSource}，然后发出的项目连接那些返回的 {@code ObservableSource} 的结果。
>
> 请注意，无法保证给定的 {@code mapper} 函数将在何处执行；它可以在订阅线程上，在上游线程上发出要映射的新项目的信号，或者在内部源终止的线程上。为确保 {@code mapper} 函数仅限于已知线程，请使用 {@link concatMap(Function, int, Scheduler)} 重载。

![concatMap](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/concatMap.v3.png)

```kotlin
    /*
     * @param mapper
     *            a function that, when applied to an item emitted by the current {@code Observable}, returns an
     *            {@code ObservableSource}
     * @param bufferSize
     *            the number of elements expected from the current {@code Observable} to be buffered
     * @param scheduler
     *            the scheduler where the {@code mapper} function will be executed
     */ 
    private fun concatMap() {
        Observable.range(1, 10)
            .concatMap(
                /* mapper = */ { source ->
                    Observable.just("mapper value $source")
                },
                /* bufferSize = */ 3,
                /* scheduler = */ Schedulers.computation()
            )
            .subscribe { result ->
                log("concatMap-> $result")
            }
    }
```
> D/RxJava-RxComputationThreadPool-1: concatMap-> mapper value 1
> D/RxJava-RxComputationThreadPool-1: concatMap-> mapper value 2
> D/RxJava-RxComputationThreadPool-1: concatMap-> mapper value 3
> D/RxJava-RxComputationThreadPool-1: concatMap-> mapper value 4
> D/RxJava-RxComputationThreadPool-1: concatMap-> mapper value 5
> D/RxJava-RxComputationThreadPool-1: concatMap-> mapper value 6
> D/RxJava-RxComputationThreadPool-1: concatMap-> mapper value 7
> D/RxJava-RxComputationThreadPool-1: concatMap-> mapper value 8
> D/RxJava-RxComputationThreadPool-1: concatMap-> mapper value 9
> D/RxJava-RxComputationThreadPool-1: concatMap-> mapper value 10

#### GroupJoin
TODO

#### GroupBy — [divide an Observable into a set of Observables that each emit a different group of items from the original Observable, organized by key](https://reactivex.io/documentation/operators/groupby.html)

> The GroupBy operator divides an Observable that emits items into an Observable that emits Observables, each one of which emits some subset of the items from the original source Observable. Which items end up on which Observable is typically decided by a discriminating function that evaluates each item and assigns it a key. All items with the same key are emitted by the same Observable.

> GroupBy 运算符将一个发射项目的 Observable 分成一个发射 Observable 的 Observable，每个 Observable 发射来自原始源 Observable 的项目的一些子集。哪些项目最终出现在哪个 Observable 上通常由一个判别函数决定，该函数评估每个项目并为其分配一个键。所有具有相同键的项目都由同一个 Observable 发出。

![groupby](https://raw.githubusercontent.com/wiki/ReactiveX/RxJava/images/rx-operators/collect.2.v3.png)

```kotlin
    private fun groupBy() {
        val circle = "circle"
        val triangle = "triangle"

        val purple = "purple"
        val blue = "blue"
        val yellow = "yellow"
        
        Observable.just(
            circle to purple,
            circle to blue,
            triangle to yellow,
            circle to yellow,
            triangle to purple,
            triangle to blue
        )
            .groupBy { shapeToColor -> shapeToColor.first }
            .flatMapSingle { grouped ->
                grouped.collect(Collectors.groupingBy { it.first })
            }
            .subscribe { result ->
                log("result is: "+result.values.toString())
            }
    }
```

> 11:53:30.071 RxJava-main                          D  result is: [[(circle, purple), (circle, blue), (circle, yellow)]]
> 11:53:30.072 RxJava-main                          D  result is: [[(triangle, yellow), (triangle, purple), (triangle, blue)]]


#### Map — [transform the items emitted by an Observable by applying a function to each item](https://reactivex.io/documentation/operators/map.html)

> The Map operator applies a function of your choosing to each item emitted by the source Observable, and returns an Observable that emits the results of these function applications.

> Map 运算符将您选择的函数应用于源 Observable 发出的每个项目，并返回一个发出这些函数应用程序结果的 Observable。

![map](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/map.v3.png)

```kotlin
    private fun map() {
        Observable.just(1)
            .map { source -> "map $source" }
            .subscribe {
                log("result is $it")
            }
    }
```
> 11:58:48.735 RxJava-main                          D  result is map 1
    
#### Scan — [apply a function to each item emitted by an Observable, sequentially, and emit each successive value](https://reactivex.io/documentation/operators/scan.html)

> The Scan operator applies a function to the first item emitted by the source Observable and then emits the result of that function as its own first emission. It also feeds the result of the function back into the function along with the second item emitted by the source Observable in order to generate its second emission. It continues to feed back its own subsequent emissions along with the subsequent emissions from the source Observable in order to create the rest of its sequence.
>
> This sort of operator is sometimes called an “accumulator” in other contexts.

> Scan 运算符将一个函数应用于源 Observable 发射的第一个项目，然后发射该函数的结果作为它自己的第一个发射。它还将函数的结果连同源 Observable 发射的第二项一起反馈给函数，以生成其第二个发射。它继续反馈自己的后续发射以及来自源 Observable 的后续发射，以创建其序列的其余部分。

> 在其他情况下，这种运算符有时被称为“累加器”。

![scan](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/scan.v3.png)

```kotlin
    private fun scan() {
        Observable.just(1 to "first", 2 to "second", 3 to "third")
            .scan { t1, t2 ->
                (t1.first + t2.first) to ("${t1.second} + ${t2.second}")
            }
            .subscribe { result ->
                log("scan-> $result")
            }
    }
```

> 12:05:39.260 RxJava-main                          D  scan-> (1, first)
> 12:05:39.260 RxJava-main                          D  scan-> (3, first + second)
> 12:05:39.260 RxJava-main                          D  scan-> (6, first + second + third)


#### Window — [periodically subdivide items from an Observable into Observable windows and emit these windows rather than emitting the items one at a time](https://reactivex.io/documentation/operators/window.html)

> Window is similar to Buffer, but rather than emitting packets of items from the source Observable, it emits Observables, each one of which emits a subset of items from the source Observable and then terminates with an onCompleted notification.
> 
> Like Buffer, Window has many varieties, each with its own way of subdividing the original Observable into the resulting Observable emissions, each one of which contains a “window” onto the original emitted items. In the terminology of the Window operator, when a window “opens,” this means that a new Observable is emitted and that Observable will begin emitting items emitted by the source Observable. When a window “closes,” this means that the emitted Observable stops emitting items from the source Observable and terminates with an onCompleted notification to its observers.

> Window 类似于 Buffer，但它不是从源 Observable 发射项目包，而是发射 Observables，每个 Observables 从源 Observable 发射项目的子集，然后以 onCompleted 通知终止。
>
> 像 Buffer 一样，Window 有很多变体，每一种都有自己的方式将原始 Observable 细分为结果 Observable 发射，每一个都包含一个到原始发射项目的“窗口”。在 Window 操作符的术语中，当一个窗口“打开”时，这意味着一个新的 Observable 被发射并且 Observable 将开始发射源 Observable 发射的项目。当窗口“关闭”时，这意味着发出的 Observable 停止从源 Observable 发出项目，并以向其观察者发出 onCompleted 通知而终止。

![window](https://reactivex.io/documentation/operators/images/window.C.png)

```kotlin
    private fun window() {
        Observable.range(0, 9)
            .window(/* count = */ 3, /* skip = */ 2, /* bufferSize = */ 4)
            .flatMap { observables -> observables }
            .subscribe { result ->
                log("window-> $result")
            }
    }
```
> 13:07:15.383 RxJava-main                          D  window-> 0
> 13:07:15.383 RxJava-main                          D  window-> 1
> 13:07:15.383 RxJava-main                          D  window-> 2
> 13:07:15.383 RxJava-main                          D  window-> 2
> 13:07:15.383 RxJava-main                          D  window-> 3
> 13:07:15.383 RxJava-main                          D  window-> 4
> 13:07:15.383 RxJava-main                          D  window-> 4
> 13:07:15.383 RxJava-main                          D  window-> 5
> 13:07:15.383 RxJava-main                          D  window-> 6
> 13:07:15.383 RxJava-main                          D  window-> 6
> 13:07:15.383 RxJava-main                          D  window-> 7
> 13:07:15.383 RxJava-main                          D  window-> 8
> 13:07:15.384 RxJava-main                          D  window-> 8

### Filtering Observables
#### Debounce — [only emit an item from an Observable if a particular timespan has passed without it emitting another item](https://reactivex.io/documentation/operators/debounce.html)

> The Debounce operator filters out items emitted by the source Observable that are rapidly followed by another emitted item.

> Debounce 运算符过滤掉由源 Observable 发射的、紧随其后的另一个发射项目的项目(主要用于数据防抖，比如避免点击事件连续触发)。

![debounce](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/debounce.v3.png)

```kotlin
    private fun debounce() {
        Observable.create { emitter ->
            emitter.onNext(1) // skip
            Thread.sleep(300L)
            emitter.onNext(2) // skip
            Thread.sleep(600L)
            emitter.onNext(3) // deliver
        }
            .debounce(500L, TimeUnit.MICROSECONDS, AndroidSchedulers.mainThread())
            .subscribe { result ->
                log("Debounce-> $result")
            }
    }
```
> 19:03:21.903 RxJava-main                          D  Debounce-> 3

#### Distinct — [suppress duplicate items emitted by an Observable](https://reactivex.io/documentation/operators/distinct.html)

> The Distinct operator filters an Observable by only allowing items through that have not already been emitted.
>
> In some implementations there are variants that allow you to adjust the criteria by which two items are considered “distinct.” In some, there is a variant of the operator that only compares an item against its immediate predecessor for distinctness, thereby filtering only consecutive duplicate items from the sequence.

> Distinct 运算符通过仅允许尚未发射的项目通过来过滤 Observable。
>
> 在某些实现中，有一些变体允许您调整将两个项目视为“不同”的标准。在某些情况下，运算符有一个变体，它只比较一个项目与其直接前身的区别，从而只从序列中过滤连续的重复项目。

![distinct](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/distinct.v3.png)

```kotlin
    // @see distinctUntilChanged()
    private fun distinct() {
        Observable.just(1, 1, 2, 3, 3, 4)
            .distinct()
            .subscribe {
                log("Distinct-> $it")
            }
    }
```
> 10:03:18.847 RxJava-main                          D  Distinct-> 1
> 10:03:18.847 RxJava-main                          D  Distinct-> 2
> 10:03:18.847 RxJava-main                          D  Distinct-> 3
> 10:03:18.847 RxJava-main                          D  Distinct-> 4

#### ElementAt — [emit only item n emitted by an Observable](https://reactivex.io/documentation/operators/elementat.html)

> The ElementAt operator pulls an item located at a specified index location in the sequence of items emitted by the source Observable and emits that item as its own sole emission.

> ElementAt 运算符拉取位于源 Observable 发射的项目序列中指定索引位置的项目，并将该项目作为其自己的唯一发射发射。

![elementAt](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/elementAt.o.png)

```kotlin
    private fun elementAt() {
        Observable.fromIterable(listOf(1, 2, 3, 4, 5))
            .elementAt(2)
            .subscribe {
                log("elementAt-> $it")
            }
    }
```
> 10:07:49.988 RxJava-main                          D  elementAt-> 3

#### Filter — [emit only those items from an Observable that pass a predicate test](https://reactivex.io/documentation/operators/filter.html)

> The Filter operator filters an Observable by only allowing items through that pass a test that you specify in the form of a predicate function.

> Filter 运算符通过仅允许通过您以谓词函数形式指定的测试的项目过滤 Observable。

![filter](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/filter.v3.png)

```kotlin
    private fun filter() {
        Observable.fromIterable(listOf(1, 2, 3, 4))
            .filter { source -> 3 == source }
            .subscribe {
                log("Filter-> $it")
            }
    }
```

> 10:12:20.072 RxJava-main   D  Filter-> 3

#### First — [emit only the first item, or the first item that meets a condition, from an Observable](https://reactivex.io/documentation/operators/first.html)

> If you are only interested in the first item emitted by an Observable, or the first item that meets some criteria, you can filter the Observable with the First operator.
>
> In some implementations, First is not implemented as a filtering operator that returns an Observable, but as a blocking function that returns a particular item at such time as the source Observable emits that item. In those implementations, if you instead want a filtering operator, you may have better luck with Take(1) or ElementAt(0).
> 
> In some implementations there is also a Single operator. It behaves similarly to First except that it waits until the source Observable terminates in order to guarantee that it only emits a single item (otherwise, rather than emitting that item, it terminates with an error). You can use this to not only take the first item from the source Observable but to also guarantee that there was only one item.

> 如果您只对 Observable 发出的第一项感兴趣，或者满足某些条件的第一项，您可以使用 First 运算符过滤 Observable。
>
> 在某些实现中，First 不是作为返回 Observable 的过滤运算符实现的，而是作为在源 Observable 发出该项目时返回特定项目的阻塞函数来实现的。在这些实现中，如果您想要一个过滤运算符，您可能会更幸运地使用 Take(1) 或 ElementAt(0)。
>
> 在某些实现中，还有一个 Single 运算符。它的行为与 First 类似，只是它会一直等到源 Observable 终止，以保证它只发出一个项目（否则，它不会发出那个项目，而是以错误终止）。您不仅可以使用它从源 Observable 中获取第一项，还可以保证只有一项。

![firstElement](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/firstElement.m.png)

```kotlin
    // @see first(defaultItem)
    // @see firstElement()
    // @see firstOrError()
    private fun firstElement() {
        Observable.fromIterable(listOf(1, 2, 3, 4))
            .firstElement()
            .subscribe {
                log("FirstElement-> $it")
            }
    }
```

#### IgnoreElements — [do not emit any items from an Observable but mirror its termination notification](https://reactivex.io/documentation/operators/ignoreelements.html)

> The IgnoreElements operator suppresses all of the items emitted by the source Observable, but allows its termination notification (either onError or onCompleted) to pass through unchanged.
>
> If you do not care about the items being emitted by an Observable, but you do want to be notified when it completes or when it terminates with an error, you can apply the ignoreElements operator to the Observable, which will ensure that it will never call its observers’ onNext handlers.

> IgnoreElements 运算符抑制源 Observable 发出的所有项目，但允许其终止通知（onError 或 onCompleted）不加改变地通过。
>
> 如果您不关心 Observable 发出的项目，但您确实希望在它完成或因错误而终止时得到通知，您可以将 ignoreElements 运算符应用于 Observable，这将确保它永远不会调用其观察者的 onNext 处理程序。

![ignoreElements](https://reactivex.io/documentation/operators/images/ignoreElements.c.png)

```kotlin
    private fun ignoreElements() {
        Observable.fromIterable(listOf(1, 2, 3))
            .ignoreElements()
            .subscribe {
                log("IgnoreElements-> onComplete")
            }
    }
```

> 10:22:49.552 RxJava-main  D  IgnoreElements-> onComplete
