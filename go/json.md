### json
* json Encode, Marshal == struct to json
* json Decode, Unmarshal == json to struct
* https://golang.org/pkg/encoding/json/

```go
package main

import (
    "bytes"
    "encoding/json"
    "errors"
    "fmt"
    "os"
    "strconv"
    "time"
)

// 필드명은 대문자여야 json encoding이 가능하다.
type Person struct {
    Name     string    `json:"name"`
    Age      int       `json:",omitempty"` // 0이면 무시, `json:"-"` <- 무시
    Id       int       `json:",string"`
    Status   Status    `json:"status"`
    BirthDay time.Time `json:"birthday"`
}

type Status int

// JSON 직렬화 사용자 정의
func (s Status) MarshalJSON() ([]byte, error) {
    statusNum := int(s)
    switch statusNum {
    case 1:
        return []byte("111"), nil
    case 2:
        return []byte("222"), nil
    case 3:
        return []byte("333"), nil
    default:
        return nil, errors.New("invalid status")
}
}

func (s *Status) UnmarshalJSON(b []byte) error {
    no, err := strconv.Atoi(string(b))
    if err != nil {
        return err
    }
    *s = Status(no)
    return nil
}

type Simple struct {
    Data int
}

func main() {
    // 직렬화. struct to json([]byte)
    b, err := json.Marshal(Person{Name: "tu", Age: 0, Id: 777, Status: 3, BirthDay: time.Now()})
    if err != nil {
        panic(err)
    }
    buf := bytes.NewBuffer(b)
    fmt.Fprintln(os.Stdout, buf)
    fmt.Println(string(b))
    
    // 역직렬화. json([]byte) to struct
    p := Person{}
    err = json.Unmarshal(b, &p)
    if err != nil {
        panic(err)
    }
    fmt.Printf("%#v\n", p)
    
    // 직렬화
    var buf2 bytes.Buffer
    env := json.NewEncoder(&buf2)
    err = env.Encode(struct {
        Name string
    }{Name: "tuyy"})
    if err != nil {
        panic(err)
    }
    fmt.Printf("%s\n", buf2.String())
    
    // 역직렬화
    buf3 := bytes.NewBuffer([]byte(`{"Data":111}{"Data":222}{"Data":333}`))
    dec := json.NewDecoder(buf3)
    for i := 0; i < 3; i++ {
        var obj Simple
        err = dec.Decode(&obj)
        if err != nil {
            panic(err)
        }
        fmt.Printf("%#v\n", obj)
    }
    
    fmt.Printf("%#v\n", json.Valid([]byte(`{}`))) // true
    fmt.Printf("%#v\n", json.Valid([]byte(`[123]`))) // true
    fmt.Printf("%#v\n", json.Valid([]byte(`{123}`))) // false
    
    var out bytes.Buffer
    json.HTMLEscape(&out, []byte(`{"Name":"<b>HTML content</b>"}`))
    fmt.Fprintln(os.Stdout, out.String())
}

# Result
{"name":"tu","Id":"777","status":333,"birthday":"2021-02-06T00:30:17.136854+09:00"}
{"name":"tu","Id":"777","status":333,"birthday":"2021-02-06T00:30:17.136854+09:00"}
main.Person{Name:"tu", Age:0, Id:777, Status:333, BirthDay:time.Time{wall:0x82839f0, ext:63748135817, loc:(*time.Location)(0x11c2480)}}
{"Name":"tuyy"}

main.Simple{Data:111}
main.Simple{Data:222}
main.Simple{Data:333}
true
true
false
{"Name":"\u003cb\u003eHTML content\u003c/b\u003e"}
```
