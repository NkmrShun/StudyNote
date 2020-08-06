# 冒泡排序 #
比较当前数与下一个的大小，如果下一个大，则互换位置，一趟排序大的放最后。需要（n-1）趟排序。
优化1 如果这一趟排序没有变化，证明排序完成，返回结果（$isChange = false）
优化2 后面优化好的数据再遍历就会变得多余，记录下上一次最后交换的位置index，交换范围变成0~index
```
//基本逻辑
function bubbleSort(&$arr)
{
    $length = count($arr) - 1;
    // 外循环
    for ($j = 0; $j < $length; ++$j) {
        // 内循环
        for ($i = 0; $i < $length; ++$i) {
            // 如果后一个值小于前一个值，则互换位置
            if ($arr[$i + 1] < $arr[$i]) {
                $tmp = $arr[$i + 1];
                $arr[$i + 1] = $arr[$i];
                $arr[$i] = $tmp;
            }
        }
    }
}
//优化版本
function bubbleSortBetter(&$arr)
{
    $flag   = true; // 标示 排序未完成
    $length = count($arr)-1; // 数组最后一个元素的索引
    $index  = $length; // 最后一次交换的索引位置 初始值为最后一位
    while ($flag) {
        $flag = false; // 假设排序已完成
        for ($i=0; $i < $index; $i++) {
            if ($arr[$i] > $arr[$i+1]) {
                $flag  = true; // 如果还有交换发生，则排序未完成
                $last  = $i; // 记录最后一次发生交换的索引位置
                $tmp   = $arr[$i];
                $arr[$i] = $arr[$i+1];
                $arr[$i+1] = $tmp;
            }
        }
        $index = !$flag ? : $last;
    }
}
//变种
function bubbleSort(&$arr)
{
	$length = count($arr);
	if ($length <= 1) return $arr;
	for ($i = 0; $i < $length - 1; $i++) {
        $isChange = false;
		//每一趟排好一个数，循环次数-1
        for ($j = 0; $j < $length - $i - 1; $j++) {
			//互换位置，大的放后面
            if ($arr[$j] > $arr[$j + 1]) {
                $tmp = $arr[$j];
                $arr[$j] = $arr[$j + 1];
                $arr[$j + 1] = $tmp;
				//发生了互换
                $isChange = true;
            }
        }
		//如果比较完一趟没有发生互换，说明已经完成排序
        if (!$isChange) {
            break;
        }
    }
	return $arr;
}
```