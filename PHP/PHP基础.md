- 变量前加&，改变变量会改变源变量。
```
$a = &$b;
$a = '123';
echo $b;//123
```