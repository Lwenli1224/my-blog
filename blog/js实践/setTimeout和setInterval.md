# setTimeout和setInterval


大部分javascript程序员都认为setTimeout和setInterval的区别仅仅是调用次数不同，其实它们的区别非常之大，如果不通过实践了解其内部机制，那么在编写一些复杂需求或动画时，肯定会发生奇葩错误而掉入坑中不能自拔。

首先要明确的一个基础概念，javascript是单线程，任何延迟执行或是异步事件，并没有其它线程处理，在一个时间片内只有一个任务在执行。

那么问题来了，当计时器的回调方法是一个计算量很大的任务，其执行时间已经超出了计时器的间隔时间，javascript的计时器怎样处理这种情况呢？

当然，查阅谷歌百度有很多这样那样的讲解，而作为一名合格的前端工程师，应该通过数据和实践得出结论，废话不提，先上代码。

	/**
	 * 测试方法，一亿次循环加开平方
	 */
	function testFunc() {
	    for (var i = 0; i < 100000000; i++) {
	        Math.sqrt(10);
	    }
	}
	
	/**
	 * 测试setInterval方法
	 */
	var testInterval = function () {
	    var nowTime = new Date();
	    var timer = setInterval(function () {
	        var nowTime = new Date();
	        console.log("开始执行时间" + nowTime.getTime());
	        testFunc();
	        console.log("本次回调执行时间为" + (new Date().getTime() - nowTime.getTime()));
	    }, 100);
	};
	
	/**
	 * 测试setTimeout方法
	 */
	var testTimeout = function () {
	    var nowTime = new Date();
	    var timer = setTimeout(function () {
	        var nowTime = new Date();
	        console.log("开始执行时间" + nowTime.getTime());
	        testFunc();
	        console.log("本次回调执行时间为" + (new Date().getTime() - nowTime.getTime()));
	        setTimeout(arguments.callee, 100);
	    }, 100);
	};


简述测试思路，获得每次回调方法执行的开始时间时间戳，及测试方法的执行时间。举个例子，例如一次计时器回调处理中，开始执行时间为1423841003097，而测试方法执行时间为176，已经超出了100毫秒的计时器间隔，那么再结合下次回调的执行时间，比如是1423841003274，我们便可判断出，在这种情况下，回调方法几乎是连续执行的。


setInterval：第一次的开始时间为1423841357648，而回调执行时间为167，第二次的开始时间为1423841357816，近似等于第一次的开始时间+回调执行时间。我们可以很容易得出结论，**setInterval在执行时间超出计时器间隔时，会将后一次的回调方法立即加到执行队列中去，并尽快的执行它。**换句话说，当只有计时器在该时间段执行时，在执行时间大于计时器间隔时，其没有在预期时间点执行的方法会连续执行。
对比setTimeout的部分执行结果

setTimeout：第一次的开始时间为1423841846102，回调执行时间为187，第二次的开始时间为1423841846391，与第一次回调执行完毕的时间近似差了100毫秒，差异出现了！**原来setTimeout在连续调用时，不论回调方法的执行时间如何，它都能严格地控制回调方法的间隔时间。**

jquery的作者John Resig大神曾分析过两种的计时器，他认为，在使用setInterval遇到执行时间超出计时器间隔的情况时，本应该在预期时间点执行的方法会取消执行，他是通过javascript的单线程来讲述这个问题的。
不论javascript的内部机制具体是怎样的，但我们已通过真实的数据阐述了这两种计时器的最大区别，总结一下：
**setInterval计时器将“间隔”视为从两次回调方法开始时间之差，当执行回调时间过长时，会忽略掉间隔。
setTimeout计时器将“间隔”视为经过多久开始执行回调方法，回调方法之间的间隔在理论上是相等的。**
当执行回调的方法的间隔需要很精确或是制作js动画时，我们尽量选用setTimeout计时器。