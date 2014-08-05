##酷站
- <http://www.golang.in> 对channel及协程讲得很到位
- <https://github.com/Unknwon/go-study-index> 资料索引
- <https://github.com/mmcgrana/gobyexample/tree/master/examples> 类库示例

##编译参数
    //减小编译文件尺寸
    go build -ldflags "-s -w"
    //不显示Win命令行窗口
    go build -ldflags -Hwindowsgui project.go

##array
###循环:
	ary := []string{"a","b","c"}
	for i,v := range ary{
	}

##string
###大量字符串累加:
	var buf bytes.Buffer
	for i := 0; i < 1000000; i++ {
		line := "aaaaaaaa\r\n"
		buf.WriteString(line)
	}
	return buf.String()
###遍历字符串
	s := "Hi中国"
	for i,v := range s {//此处按rune遍历
		println(i, v)
	}		
