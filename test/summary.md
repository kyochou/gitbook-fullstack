# 测试

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