
### goroutine涉及的问题
goroutine是实现并发的手段，协程并发执行的过程中，还涉及到同步问题，同步就是多个协程之间如何传递消息，在此引入channel

### channel
channel 通道，常用于goroutine之间同步消息，可视为一个队列，
