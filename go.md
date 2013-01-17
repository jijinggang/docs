##array
#####循环:
	ary := []string{"a","b","c"}
	for i,v := range ary{
	}

##string
#####大量字符串累加:
	var buf bytes.Buffer
	for i := 0; i < 1000000; i++ {
		line := "aaaaaaaa\r\n"
		buf.WriteString(line)
	}
	return buf.String()
		