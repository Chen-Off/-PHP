转载：https://www.onlyke.com/html/677.html

#
这个Writer->save错误可能由于很多原因导致，其中有一部分是因为header和缓冲区的错误导致的

这部分具体讨论可以看这里http://stackoverflow.com/questions/8566196/phpexcel-to-download

 

然而还有一个不容易发现的问题，在高版本PHP7下，出现ERR_INVALID_RESPONSE的错误还可能由于下面的原因导致

```
Fatal error: 'break' not in the 'loop' or 'switch' context in <mypath>\PHPExcel\PHPExcel\Calculation\Functions.php on line 581

Fatal error: 'break' not in the 'loop' or 'switch' context in <mypath>\PHPExcel\PHPExcel\Calculation\Functions.php on line 581
```
请打开PHPExcel\Calculation\Functions.php文件，删除掉581行的break即可
