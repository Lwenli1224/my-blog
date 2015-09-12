# 警惕javascript的console.log

javascript的console.log()一个很方便调试代码的函数，我们经常在程序里使用它来打印变量或错误。许多程序员在代码中加入log函数后，在上线后并未注释或删除该语句，尤其是nodejs的项目，认为这个东西放在那里也无伤大雅，反正客户端也看不到。其实，殊不知这个语句有时正是耗费性能的关键所在。

## 拖慢循环速度
场景代码如下：
	
````javascript
var duration = 300000,
	isRun = true,
	count = 0,
	startTime = 0,
	endTime = 0;
	
startTime = new Date().getTime();
run();
endTime = new Date().getTime();
// 打印执行时间
console.log(endTime - startTime);

function run() {
    while (isRun) {
    	// 打印当前次数
        console.log(count);
        if (count > 100000) {
            console.log('complete!');
            isRun = false;
        }
        count++;
    }
    return false;
}
		
````
在命令行里用node运行了一下该脚本，时间高达1.8s，而注释打印次数的语句后运行时间降低到18ms！由此可见，log对性能的损耗是多么巨大，如果某个程序员在一次庞大的计算里加入了log函数，那么后果将会是十分可怕的。

## 大量消耗内存

console.log()同样也会大量占用内存，并且即使log语句所在的函数执行完后，该内存也不会得到释放。场景代码如下：

````javascript
var duration = 300000,
    isRun = true,
    count = 0;
    
// 使用setTimeout保持node进程不退出
setTimeout(function () {
    isRun = false;
}, duration);
run();

function run() {
    while (isRun) {
    	// 打印当前次数
        console.log(count);
        if (count > 100000) {
            console.log('complete!');
            isRun = false;
        }
        count++;
    }
    return false;
}


````
该脚本执行完后，使用top命令查看node进程的占用内存，发现高达67M。注释log函数，再次执行该脚本后，查看node进程内存使用情况，发现仅占用了8000多K。


通过实际的代码测试，我们发现log函数对性能的影响是较大的，并且会随着执行次数增多，不断占用内存，建议在nodejs应用上线前跑一个脚本检测console.log()语句是否存在。












