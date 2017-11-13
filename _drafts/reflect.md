```go
// isZeroValue guesses whether the string represents the zero
// value for a flag. It is not accurate but in practice works OK.
func isZeroValue(v interface{}) bool {
	typ:= reflect.TypeOf(v)
    // 错误的用法 ？？
	if reflect.ValueOf(v) == reflect.Zero(typ) {
		return true
	}
	return false
}
```

```go
// hasValue determines if a reflect.Value is it's default value
// fasle if a reflect.Value is it's default value
func hasValue(field reflect.Value) bool {
	switch field.Kind() {
	case reflect.Slice, reflect.Map, reflect.Ptr, reflect.Interface, reflect.Chan, reflect.Func:
		return !field.IsNil()
	default:
		return field.IsValid() && field.Interface() != reflect.Zero(field.Type()).Interface()
	}
}

// false , 如果 interface{} 是它实际类型的默认值
func HasValue(v interface{}) bool {
	field := reflect.ValueOf(v)
	switch field.Kind() {
	case reflect.Slice, reflect.Map, reflect.Ptr, reflect.Interface, reflect.Chan, reflect.Func:
		return !field.IsNil()
	default:
		return field.IsValid() && field.Interface() != reflect.Zero(field.Type()).Interface()
	}
}
```
