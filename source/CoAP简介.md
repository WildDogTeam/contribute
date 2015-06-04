<link rel="stylesheet" href="http://yandex.st/highlightjs/6.2/styles/googlecode.min.css">
 
<script src="http://code.jquery.com/jquery-1.7.2.min.js"></script>
<script src="http://yandex.st/highlightjs/6.2/highlight.min.js"></script>
 
<script>hljs.initHighlightingOnLoad();</script>
<script type="text/javascript">
 $(document).ready(function(){
      $("h2,h3,h4,h5,h6").each(function(i,item){
        var tag = $(item).get(0).localName;
        $(item).attr("id","wow"+i);
        $("#category").append('<a class="new'+tag+'" href="#wow'+i+'">'+$(this).text()+'</a></br>');
        $(".newh2").css("margin-left",0);
        $(".newh3").css("margin-left",20);
        $(".newh4").css("margin-left",40);
        $(".newh5").css("margin-left",60);
        $(".newh6").css("margin-left",80);
      });
 });
</script>
<div id="category"></div>

#受限应用协议（CoAP）
##1, 简介
在互联网上，使用web服务（web API）已经非常普遍，大多应用都在使用基于web的REST架构。

Constrained RESTful Environments（CoRE）的工作目标是采用恰当的方式在受限节点（如8位微控制器、较小RAM和ROM）和受限网络（例如6LoWPAN，[[RFC4944]](http://tools.ietf.org/pdf/rfc4944)）上实现REST架构。6LoWPAN等受限网络支持把IPv6数据包分片成为小的链路层数据帧。然而，这导致数据发送成功率的下降。CoAP协议的设计目标之一是使数据包开销尽可能小，以减少分片的发生。

CoAP目标是设计一个通用的网络协议，满足受限环境的特殊需求，特别考虑了能源、楼宇自动化和其它M2M应用。CoAP不是对HTTP[[RFC2616]](http://tools.ietf.org/pdf/rfc2616)协议的压缩，而是实现了REST的一个子集，并为M2M应用程序做了优化。尽管CoAP协议可以被当作一个HTTP压缩之后变得更紧凑的协议来使用，但更重要的是它还提供了M2M的一些特性，例如内置的资源发现、多播支持和异步消息交换。

本文档描述了CoAP协议，它可以很容易的与HTTP协议互相转换以集成到现有的web中，同时满足了许多特定的需求，如多播支持、非常小的开销和足够简单以适应受限环境和M2M应用。
###1.1, 特性
CoAP协议主要有如下特性：

*	它是一个满足受限环境下M2M需求的协议；

*	基于UDP[[RFC0768]](http://tools.ietf.org/pdf/rfc0768)，拥有可选的可靠性保障，支持单播和多播请求；

*	异步消息交换；

*	轻量级的头部，且解析复杂度低；

*	支持URI和Content-Type；

*	能实现简单的数据缓存和代理；

*	无状态的HTTP映射，可以构建代理服务器，使CoAP资源可以用HTTP协议访问，也可以使HTTP接口实现于CoAP协议之上；

*	支持DTLS[[RFC6347]](http://tools.ietf.org/pdf/rfc6347)协议。

###1.2, 术语解释

出现在本文档中的关键字“**必须**”、“**必须不**”、“**要求**”、“**应该**”、“**应该不**”、“**应当**”、“**应当不**”、“**建议**”、“**不建议**”、“**或许**”和“**可选**”，当加粗时，都在[[RFC2119]](http://tools.ietf.org/pdf/rfc2119)中定义。这些词语也可以普通的形式出现，则不具备预先定义的含义。

本文档要求读者熟悉[[RFC2616]](http://tools.ietf.org/pdf/rfc2616)中讨论的所有术语，包括resource , representation，cache和fresh。（对HTTP协议更新的RFC有7230到7235，但由于本文档的完成早于这些对HTTP协议更新的rfc，故本文档引用的是先前的HTTP协议版本：[[RFC2616]](http://tools.ietf.org/pdf/rfc2616)）。此外，本文档定义了以下术语：

* 端(Endpoint)

	参与CoAP协议的一个实体。通俗的说，一个端指的是一个节点(node)，尽管Host这个词更符合互联网标准，与传输层的多路复用技术联系起来，可以包括一个UDP端口号。

*	发送者(Sender)

	消息的发起端。从交互角度来说，也称为“源端”（source endpoint）。

*	接收者(Recipient)

	消息的目的端。从交互角度来说，也称为“目的端”（destination endpoint）。

*	客户端(Client)

	消息请求的源端，消息响应的目的端。

*	服务端(Server)

	消息请求的目的端，消息响应的源端。

*	原始服务端(Origin Server)

	存储或创建一个给定的资源的服务端。

*	中间人(Intermediary)

	对于一个原始服务端（也可能是其它的中间人）来说，一个即是服务端又是客户端的CoAP端。常见的场景是一个代理。本文档中讨论了几种这样类型的代理。

*	代理(Proxy)

	代理是主要作用为转发请求和响应消息的中间人，有可能起到缓存，命名空间转换，或者协议转换的作用。与一般的中间人不同的是，代理通常不实现任何语义。在转发请求的场景下，根据定位的不同，主要分为两种：正向代理和反向代理。在某些情况下，一个端有可能根据每一个请求的特性转换自己的角色，可以作为原始服务端、正向代理或反向代理。

*	正向代理(Forward-Proxy)

	即客户端的代理，通常是通过本地配置，用来代表客户端发出请求，必要时做一些转换。有些转换是很细微的，如代理“coap”开头的URI，然而有些请求则需要在其它应用层协议和coap协议间作转换。

*	反向代理(Reverse-Proxy)

	代替一个或多个服务端接收请求的端，必要时做一些转换。与正向代理不同的是，反向代理对客户端可能是完全透明的。反向代理接收请求，把它自己当成目标资源的原始服务端。

*	CoAP到CoAP代理(CoAP-to-CoAP Proxy)

	把一个CoAP请求映射到另一个CoAP请求的代理。也就是说它的服务端部分和客户端部分都使用CoAP协议。与“跨协议代理”形成对比。

*	跨协议代理(Cross-Proxy)

	跨协议代理指的是在不同协议之间做转换的代理，例如一个从CoAP到HTTP的代理，或者HTTP到CoAP的代理。相对于CoAP到CoAP代理，跨协议代理有很多种。

*	需应答消息(Confirmable Message)

	要求ACK的消息称为需应答消息。当没有发生数据包丢失的时候，每个需应答消息必定会有一个类型为ACK或Reset的响应。后面简写为CON。

*	不需应答消息(Non-confirmable Message)

	不要求ACK的的消息称为不需应答消息。通常用于某些应用中周期性的重复发送数据的情形，例如不断的读取一个传感器的数据。后面简写成NON。

*	ACK消息(Acknowledgement Message)

	ACK消息用于确认某个可靠消息已经到达。ACK消息自身并不代表这个请求处理的结果是成功还是失败。ACK消息有可能会同时为附带响应(Piggybacked Response)。

*	重置消息(Reset Message)

	Reset消息代表的是一个消息（需要应答或者不需要应答的消息）被收到了，但是由于缺少某些上下文信息而无法被正常的处理。这种情况通常是由于接收节点重启了，因而缺失了一些必要的信息，导致当前接收到的消息无法被处理。利用reset消息，也是一种低开销的检查端是否存活的方式（也称作CoAP ping，发送一个空的需应答消息）。后面简写成RST。

*	附带响应(Piggybacked Response)

	附带响应指的是，对于一个请求消息，它的ACK消息中包含了响应数据。

*	单独响应(Separate Response)

	当请求是一个需应答消息时，如果它的ACK是一个空消息（因为服务端对该请求产生对应结果需要一些时间），那么就需要一个单独的消息交换过程来完成对请求的响应（5.2.2节）。

*	空消息(Empty Message)

	空消息的code是0.00，有可能是请求，也可能是响应。空消息只有4个字节的header，没有body部分。

*	重要选项(Critical Option)

	指的是这样的选项：只有接收端正确的理解这个选项，那么这个请求才能被正确的处理（5.4.1节）。注意，这些选项的值通常都有一个范围，不支持的选项值会导致错误的响应消息或者拒绝这个消息。

*	非重要选项(Elective Option)

	非重要选项指的是如果接收端不理解这个选项，那么可以把它忽略。协议允许忽略这个选项而对消息进行处理（参见5.4.1节）。

*	非安全选项(Unsafe Option)

	非安全的选项指的是，代理必须理解这个选项才能正确的转发这个消息。并非所有的重要选项都是非安全选项。

*	转发安全选项(Safe-to-Forward Option)

	代理不理解这个选项，但也可以安全的转发这个消息。在不理解这个选项的情况下，也可以转发这个消息（参见5.4.2节）。

*	资源发现(Resource Discovery)

	资源发现指的是CoAP客户端获得服务端支持的所有资源列表的过程（参见第7章）。

*	内容格式(Content-Format)
	
	内容格式指的是互联网媒体类型和内容编码，用一个数值型标识符来标识。这个数值型标识符在“COAP 内容格式”中定义。当重点注意力不是在这个数值型的标识上，而是在资源表现本身时，就称作“表现格式”（REPRESENTATION FORMAT）。

更多关于受限节点和受限节点网络的术语，请参考[[RFC7228]](http://tools.ietf.org/pdf/rfc7228 "RFC7228")。

在本文档中，术语byte指的是通常的字节定义（即一个字节为8bit）。

本协议中所有长度超过一个字节的整型都采用网络字节序。

本文中使用的符号和C语言中类似，例外情况：运算符" ** "表示幂运算。
 