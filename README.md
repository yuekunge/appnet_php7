<p># appnet介绍<br />appnet是一个基于linux epoll的多线程的高性能异步网络事件库，目标是用高性能的PHP版本<br />搭载appnet取代C/lua或C/python模式,快速构建强有力的长连接服务器，以弥补PHP固有的缺陷。<br />使其可广泛用于聊天系统，游戏服务器，消息通知服务器等实时通信场景。可对网络IO密集性场景<br />或CPU密集性场景配置reactor数量和woker数量的比例，使硬件运行于最佳状态。<br /><br />其特点有<br />[1],高性能,纯C语言开发,epoll异步非阻塞事件通知机制.<br />[2],易用性,使用方式简单,并提供PHP7.0版本扩展。<br />[3],高并发,多线程网络IO,Per Thread One Loop并发模型，多个worker进程并行处理业务。<br />[4],多协议,可混合TCP协议，websocket协议和简单http协议与服务器通信。<br />[5],内存优化，进程间通信使用共享内存，兼容jemalloc和tcmalloc内存优化技术。<br />[6],缓冲区优化，自适应缓冲区，根据数据包大小自动增长缩小，避免内存浪费和缓冲区溢出<br />与其它网络库粗略比较<br />[1], 相比node.js单进程模式，appnet可充分利用多核CPU，不致于服务器资源浪费。<br />[2], 相比nginx多进程+fpm多进程，支持TCP协议，websocket协议等长连接。<br />[3], 相比libevent，原生支持多线程无需再次封装,支持混合协议。<br />[4], 相比memcache多线程+libevent模型，appnet支持多任务处理业务，具有更少的封装。<br />[5], 相比redis单进程网络模型，具体有更强的网络IO能力，appnet也就是redis网络层的增强版本<br /><br />以上是一个美好的愿望，目前websocket协议和http协议尚未实现，还存已知的在进程终止时有可能会<br />内存泄露BUG，目前是一个人开发，也缺少更多的测试，希望有兴趣的同学一起参与进来测试,开发和提建议。<br />让PHP真正成为世界上最好的语言，没有之一。。<br /><br />安装方法:<br />1,源码安装php_7.0.x<br />2,下载扩展到任意目录appnet_php7<br />3,执行如下指令:<br />&nbsp;&gt;cd appnet_php7<br />&nbsp;&gt;/usr/local/php7/bin/phpize<br />&nbsp;&gt;./configure --with-php-config=/usr/local/php7/bin/php-config<br />&nbsp;&gt;make<br />&nbsp;&gt;make install<br />&nbsp;&gt;php test/test.php<br />&nbsp;&gt;telnet 127.0.0.1 3011<br />&nbsp;开始测试吧。<br />&nbsp;<br />&lt;?php<br />dl( "appnet.so" );<br />$server = new appTcpServer( "0.0.0.0" , 3011 );<br />$server-&gt;on('connect', function( $serv , $fd )<br />{<br />&nbsp;&nbsp;&nbsp; echo "Client:Connect:{$fd} \n";<br />});<br /><br />$server-&gt;on('receive', function( $serv , $fd , $buffer )<br />{&nbsp;&nbsp; &nbsp;<br />&nbsp;&nbsp;&nbsp; echo "Client Recv:[{$buffer}][{$fd}] \n";<br />&nbsp;&nbsp;&nbsp; $server-&gt;send( $fd , "hellow world!" );<br />&nbsp;&nbsp;&nbsp; //close method<br />&nbsp;&nbsp;&nbsp; //$server-&gt;close( $fd );<br />});<br /><br />$server-&gt;on( 'close' , function( $serv , $fd )<br />{<br />&nbsp;&nbsp;&nbsp; echo "Client Close:$fd";<br />});<br />$server-&gt;run();<br />?&gt;</p>
<p>author:bona<br />QQ交流群:379084776</p>