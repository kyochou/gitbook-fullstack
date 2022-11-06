# 测试
提高测试覆盖率的最好方法, 是识别并删除不必要的代码.  
简单的说, 测试是一个可重复的过程, 是一种帮助确定某件事情在特定情况下是否按预期工作的方法.  
越是人们关注和喜爱的程序, 它的测试(尤其是自动化测试)做得就越充分, 测试流程就越规范.
我们说测试的时候一般是指自动化测试, 也就是写一些小的程序来检测被测试代码(产品代码)的行为和预期的是否一致.  

## TDT
Table Driven Test, 表格驱动测试.
它将创建测试程序的步骤分为规划及实现二个阶段. 即将测试过程和其输入/输出数据分开来定义.

```go
func TestNewSuits52(t *testing.T) {
	var countAsserts = []struct {
		total uint16
		suits uint8
	}{
		{52, 1},
		{416, 8},
	}

	for _, data := range countAsserts {
		if data.total == uint16(NewSuits52(data.suits, true).Count()) {
			t.Log(`pass`)
		} else {
			t.Error(`faild`)
		}
	}
}

```


## Fuzzing 
Fuzzinng 为模糊测试, 是一种自动化测试技术, 可以随机生成测试数据集, 然后调用要测试的功能代码来检查功能是否符合预期.
Fuzz 测试的存在, 并不是为了替代单元测试, 而是为单元测试提供更好的保障.  































