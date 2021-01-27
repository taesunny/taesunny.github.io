---
title:  "[Test] GoLang string type variable default value"
excerpt: ""

categories:
  - etc
tags:
  - [etc, test, pid, cgroup]

toc: true
toc_sticky: true
 
date: 2020-08-02
last_modified_at: 2020-08-02
---

<br>

# Conclusion

Default value is "".

<br>
---

# Test Code

```go
package main

import (
	"fmt"
)

type Student struct {
    Name string
}

func main() {
	var a Student
	fmt.Println("Hello, playground" + a.Name)
	
	if a.Name == "" {
		fmt.Println("Same")
	} else {
		fmt.Println("Not Same")
	}
	
	var testString string
	fmt.Println("Hello, playground" + testString )
	
	if testString == "" {
		fmt.Println("Same")
	} else {
		fmt.Println("Not Same")
	}
}
```

---

# Test Result

{:refdef: style="text-align: center;"}
![test result](/assets/img/etc/2021-01-27-19-11-42.png)
{: refdef}
