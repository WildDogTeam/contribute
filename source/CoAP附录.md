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

##12.	互联网地址分配注意事项（IANA Considerations）

###12.1	CoAP代码注册（CoAP Code Registries）
这篇文档定义了两个sub-registries，给CoAP头部代码字段的值包含“Constrained RESTful Environments (CoRE) Parameters”注册表。将来参考“CoRE参数”注册表。

这两个sub-registries都是8位值，可以用三个十进制的符号表示为“c.dd”,用一个句号将第一位和第二位数字分离开；第一位数字c为0~7，表示代码种类；第二个和第三个数字dd为00~31的十进制数，表示细节。

所有的代码值都按照sub-registries，按照如下范围安排：

    0.00 		表示一个空的消息（见4.1章）
	0.01-0.31 	表示一个请求。这个范围的值是根据“CoAP Method Codes”的sub-registry分配的（见12.1.1节）
	1.00-1.31 	保留
	2.00-5.31 	表示一个响应。这个范围的值是根据“CoAP Response Codes”的sub-registry分配的（见12.1.2节）
	6.00-7.31 	保留




####12.1.1	 方法码(Method Codes)
这个sub-registry是“CoAP Method Codes”

每次进入这个sub-registry都必须包含在0.01-0.31范围内的Method Code，方法的名字，方法文档的参考。

初始化进入这个sub-registry如下：

		     +------+--------+-----------+                       
			 | Code | Name   | Reference |                        
			 +------+--------+-----------+                        
			 | 0.01 | GET    | [RFC7252] |                        
			 | 0.02 | POST   | [RFC7252] |                        
			 | 0.03 | PUT    | [RFC7252] |                        
			 | 0.04 | DELETE | [RFC7252] |                        
			 +------+--------+-----------+  
 				表 5: CoAP Method Codes

其他Method Codes没有安排。

互联网号码分配政策在未来为这个sub-registry额外定义描述在“IETF    Review or IESG Approval” [[RFC5226]](http://tools.ietf.org/pdf/rfc5226).

方法代码的文档需要指定这个请求的语义，包含如下属性：

	这个方法的响应码成功返回。
	这个方法是否幂等，安全或两者都满足。

####12.1.2	响应码

这个sub-registry的名字是“CoAP Response Codes”

每个sub-registry必须包含在2.00-5.31范围内的响应码，响应码的描述，响应码的文档参考。

初始化进入这个sub-registry如下：

    
            +------+------------------------------+-----------+ 
            | Code | Description                  | Reference |
            +------+------------------------------+-----------+
            | 2.01 | Created                      | [RFC7252] | 
            | 2.02 | Deleted                      | [RFC7252] |
            | 2.03 | Valid                        | [RFC7252] |
            | 2.04 | Changed                      | [RFC7252] |
            | 2.05 | Content                      | [RFC7252] |
            | 4.00 | Bad Request                  | [RFC7252] |
            | 4.01 | Unauthorized                 | [RFC7252] | 
            | 4.02 | Bad Option                   | [RFC7252] |
            | 4.03 | Forbidden                    | [RFC7252] | 
            | 4.04 | Not Found                    | [RFC7252] | 
            | 4.05 | Method Not Allowed           | [RFC7252] | 
            | 4.06 | Not Acceptable               | [RFC7252] | 
            | 4.12 | Precondition Failed          | [RFC7252] | 
            | 4.13 | Request Entity Too Large     | [RFC7252] |  
            | 4.15 | Unsupported Content-Format   | [RFC7252] |  
            | 5.00 | Internal Server Error        | [RFC7252] |
            | 5.01 | Not Implemented              | [RFC7252] | 
            | 5.02 | Bad Gateway                  | [RFC7252] |  
            | 5.03 | Service Unavailable          | [RFC7252] | 
            | 5.04 | Gateway Timeout              | [RFC7252] | 
            | 5.05 | Proxying Not Supported       | [RFC7252] | 
            +------+------------------------------+-----------+ 
				 表 6: CoAP Response Codes

响应码3.00-3.31是预留给将来使用。所有其他响应码都没有被安排。

互联网号码分配政策为这个sub-registry以后额外的定义描述在“IETF 
   Review or IESG Approval”[[RFC5226]](tools.ietf.org/pdf/rfc5226).

响应码的文档需要指定这个响应的语义，包含如下属性：

*	响应码应用方式
*	是否需要携带payload，option。
*	payload的语义。举个例子，2.05（内容）响应的payload是目标资源的展示；payload在错误的响应中是可读和诊断的。
*	payload的格式。举个例子，这个格式在2.05（内容）响应是通过内容格式选项表示；payload的格式在一个错误的响应中总是Net-Unicode文本
*	响应是否可以缓冲，取决于freshness model
*	响应是否通过合法性检查，取决于validation model
*	响应是否导致一个cache来标志响应已经存储，表明这个请求的URI不是最新的。


###12.2	CoAP Option Number Registry
这篇文档为CoAP options中“CoRE Parameters”注册表中的option编号定义了一个sub-registry。这个sub-registry的名字是“CoAP Option Number”。

每个sub-registry必须包含这个option编号，option的名称，还有option文档参考。

初始化进入这个sub-registry如下：

    
                 +--------+------------------+-----------+ 
                 | Number | Name             | Reference | 
                 +--------+------------------+-----------+ 
                 |      0 | (Reserved)       | [RFC7252] | 
                 |      1 | If-Match         | [RFC7252] |
                 |      3 | Uri-Host         | [RFC7252] | 
                 |      4 | ETag             | [RFC7252] |  
                 |      5 | If-None-Match    | [RFC7252] |  
                 |      7 | Uri-Port         | [RFC7252] |  
                 |      8 | Location-Path    | [RFC7252] | 
                 |     11 | Uri-Path         | [RFC7252] |
                 |     12 | Content-Format   | [RFC7252] |
                 |     14 | Max-Age          | [RFC7252] | 
                 |     15 | Uri-Query        | [RFC7252] |  
                 |     17 | Accept           | [RFC7252] |   
                 |     20 | Location-Query   | [RFC7252] |   
                 |     35 | Proxy-Uri        | [RFC7252] |  
                 |     39 | Proxy-Scheme     | [RFC7252] | 
                 |     60 | Size1            | [RFC7252] |
                 |    128 | (Reserved)       | [RFC7252] | 
                 |    132 | (Reserved)       | [RFC7252] |  
                 |    136 | (Reserved)       | [RFC7252] |   
                 |    140 | (Reserved)       | [RFC7252] |    
                 +--------+------------------+-----------+ 
                       表 7: CoAP Option Numbers

互联网号码分配政策为这个sub-registry以后额外的定义分为了如下3层。0..255是为option保留，在IETF有被定义（IETF Review or IESG Approval）。256..2047是为普通使用的包含公开规格（Specification Required）的options保留的。2048..64999是为了所有其他options，包含私人的或者赞助商规格的，这些需要经过一个特定的专家审核来确定这个option语法是定义正确的。在6500和65535之间的Option编号是保留下来用于实验。他们不是给赞助商使用，而且是禁止用于操作部署。

    	+-------------+---------------------------------------+        
   		|       Range | Registration Procedures               |        
   		+-------------+---------------------------------------+        
  	 	|       0-255 | IETF Review or IESG Approval          |     
   		|    256-2047 | Specification Required                |   
        |  2048-64999 | Expert Review                         | 
        | 65000-65535 | Experimental use (no operational use) |
        +-------------+---------------------------------------+ 
           表 8: CoAP Option Numbers: Registration Procedures

这个option编号的文档应该指定这个option和它的编号，包含以下属性：

*	option在请求中的意义。
*	option在响应中的意义。
*	这个option是critical还是elective，由option编号决定。
*	这个option是否是safe-to-forward，如果是，是否是cache-key的一部分，这些由option编号决定（见5.4.2节）
*	option值的格式和长度。
*	是否option只一次出现，还是它能出现很多次。
*	默认值。对于一个有默认值的critical option，存在这样的讨论，默认值如何能处理通过实施，而这个实施又不支持critical option（见5.4.4节）	

###12.3,  CoAP Cotent-Formats Registry
互联网媒体类型被定义成一个字符串，例如“application/xml”[RFC2046]。为了最小化使用这些媒体类型表示格式所带来的payload开销，这篇文档为互联网媒体类型子集定义了一个sub-registry，这些子集在CoAP中使用，并且每个都分配好，和content-coding合并，有数字标识。这个sub-registry的名字是“CoAP Content-Formats”，在“CoRE Parameters”表里面。

每次进入到sub-registry必须包含这些IANA注册过的媒体类型，用于定义CoAP中的媒体类型数字的范围是0-65535，cotent-coding和这些定义相关，一个文档参考描述了payload和媒体类型的表达语义。

CoAP不包括用分离的方式来转移content-encoding信息和请求或响应，因为content-encoding每一个标识符也是特定的。如果多种content-encodings和媒体类型一起使用，然后每一个分离的content-format标识符将会被注册。相似的，其他参数和互联网媒体类型关联，比如level，也能被定义为CoAP Content-Format条目。

初始化进入这个sub-registry如下：


	+--------------------------+----------+----+------------------------+ 
    | Media type               | Encoding | ID | Reference              |
    +--------------------------+----------+----+------------------------+
    | text/plain;              | -        |  0 | [RFC2046] [RFC3676]    |  
  	| charset=utf-8            |          |    | [RFC5147]              |   
 	| application/link-format  | -        | 40 | [RFC6690]              |   
 	| application/xml          | -        | 41 | [RFC3023]              |  
  	| application/octet-stream | -        | 42 | [RFC2045] [RFC2046]    | 
   	| application/exi          | -        | 47 | [REC-exi-20140211]     | 
   	| application/json         | -        | 50 | [RFC7159]              |
    +--------------------------+----------+----+------------------------+
                       表 9: CoAP Content-Formats
 
65000~65535之间所包含的标识符是预留用于实验。他们不是给赞助商使用，也禁止用于操作部署。256~9999之间的标识符预留是用于未来IETF规格（IETF Review or IESG Approval）。所有其他标识符都尚未安排。

由于一个字节标识符的命名空间非常小，IANA政策对于sub-registry未来额外的定义在0~255范围，是“Expert Review”描述在[[RFC5226]](tools.ietf.org/pdf/rfc5226)。IANA政策对于额外的定义在1000-64999包含了“First Come First Served”，描述在[[RFC5226]](tools.ietf.org/pdf/rfc5226)。总结在下表中。

      +-------------+---------------------------------------+     
      |       Range | Registration Procedures               |      
      +-------------+---------------------------------------+         
  	  |       0-255 | Expert Review                         |        
      |    256-9999 | IETF Review or IESG Approval          |       
      | 10000-64999 | First Come First Served               |    
      | 65000-65535 | Experimental use (no operational use) |          
	  +-------------+---------------------------------------+ 
          表 10: CoAP Content-Formats: Registration Procedures

在M2M的应用中，不希望这些常用的媒体类型，比如text/plain,application/xml或者application/octet-stream在实际应用中长期有效。推荐M2M的应用中使用CoAP请求新的互联网媒体类型，这些媒体类型来自于IANA表明的语义，该语义信息规定了如何创建和理解一个payload。举个例子，一个智能的payload携带的一个XML，可能请求一个更特定的类型，比如application/se+xml或者application/se-exi。

###12.4, URI Scheme Registration
这篇文档中包含注册符合CoAP规范的URI请求。这个请求遵从于[[RFC4395]](tools.ietf.org/pdf/rfc4395)
    
*	URI 名字.       
			coap     

*	Status.      
 	  	Permanent.
     
*	URI 规范语法.       
		在[[RFC7252]](tools.ietf.org/pdf/rfc7252)的6.1 中定义.  
   
*	URI 规范的语义.      
		CoAP URI规范提供了一种能在CoAP协议上定位和访问资源的的方法。通过CoAP服务器可以定义和操作资源。该规范是参考Http的URI见[RFC2616](tools.ietf.org/pdf/rfc2616)
    
*	编码注意项(Encoding considerations).       
		该规范的编码规则遵循已近制定的[[RFC3986]](tools.ietf.org/pdf/rfc)，例如其互联网和资源章节表明UTF-8-based percent-encoding.
	
*	Applications/protocols that use this URI scheme name.       
		CoAP端点利用该规范访问CoAP资源 
*	互操作注意项(Interoperability considerations).      
		None.     
*	安全注意项(Security considerations).       
		See Section 11.1 of [[RFC7252]](http://tools.ietf.org/pdf/rfc7252).  
   
*	联系方式.       
		IETF Chair <chair@ietf.org>     
		Author/Change controller.       IESG <iesg@ietf.org>   
  
*	References.       
		[[RFC7252]](http://tools.ietf.org/pdf/rfc7252)

###12.5, 安全URI规范注册表
该文档包含:

*	URI 规范名称.       
		coaps     Status.       

*	Permanent.     
	URI scheme syntax.       
		Defined in Section 6.2 of [[RFC7252]](http://tools.ietf.org/pdf/rfc7252).    
*	URI 规范语义.       
		CoAP URI规范提供了一种能在CoAP协议上标识资源的的方法，该方法把DTLS作为其安全传输层。通过CoAP服务器可以定义和操作资源。该规范是参考Http的URI [[RFC2616]](http://tools.ietf.org/pdf/rfc2616) 和 [[RFC7252]](http://tools.ietf.org/pdf/rfc7252)。

*	编码注意项(Encoding considerations).       
		该规范的编码规则遵循已近制定的[[RFC3986]](http://tools.ietf.org/pdf/rfc3986)，例如其互联网和资源章节表明UTF-8-based percent-encoding.

*	Applications/protocols that use this URI scheme name.       
		CoAP端点利用该规范通过DTLS访问CoAP资源 

*	互操作注意项(Interoperability considerations).      
		None.     
*	安全注意项(Security considerations).       
		See Section 11.1 of [[RFC7252]](http://tools.ietf.org/pdf/rfc7252).  
   
*	联系方式.       
		IETF Chair <chair@ietf.org>     
		Author/Change controller.       IESG <iesg@ietf.org>   

###12.7, 安全服务名称和端口号表

*	服务名称.       
		coaps     

*	传输协议.       
		udp     

*	代理.       
		IESG <iesg@ietf.org>     

*	联系方式.       
		IETF Chair <chair@ietf.org>     
*	描述.       
		DTLS-secured CoAP     

*	参考.       
		[[RFC7252]](http://tools.ietf.org/pdf/rfc7252)     
*	端口号.       5684

###12.8, 多播地址表
第8章，“多播CoAP”，定义了多播的使用。IANA已经安排了如下被用于CoAP节点多播地址：
    
*	IPv4  --“All CoAP Nodes”地址为224.0.1.187，源自“IPv4 Multicast Address Space Registry”。当这个地址用于发现

*	IPv6  -- "All CoAP Nodes" address FF0X::FD, from the "IPv6 Multicast
      Address Space Registry", in the "Variable Scope Multicast
      Addresses" space (RFC 3307).  Note that there is a distinct
      multicast address for each scope that interested CoAP nodes should
      listen to; CoAP needs the Link-Local and Site-Local scopes only.

##13.	感谢
Brian Frank was a contributor to and coauthor of early versions of
   this specification.

   Special thanks to Peter Bigot, Esko Dijk, and Cullen Jennings for
   substantial contributions to the ideas and text in the document,
   along with countless detailed reviews and discussions.

   Thanks to Floris Van den Abeele, Anthony Baire, Ed Beroset, Berta
   Carballido, Angelo P. Castellani, Gilbert Clark, Robert Cragie,
   Pierre David, Esko Dijk, Lisa Dusseault, Mehmet Ersue, Thomas
   Fossati, Tobias Gondrom, Bert Greevenbosch, Tom Herbst, Jeroen
   Hoebeke, Richard Kelsey, Sye Loong Keoh, Ari Keranen, Matthias
   Kovatsch, Avi Lior, Stephan Lohse, Salvatore Loreto, Kerry Lynn,
   Andrew McGregor, Alexey Melnikov, Guido Moritz, Petri Mutka, Colin
   O'Flynn, Charles Palmer, Adriano Pezzuto, Thomas Poetsch, Robert
   Quattlebaum, Akbar Rahman, Eric Rescorla, Dan Romascanu, David Ryan,
   Peter Saint-Andre, Szymon Sasin, Michael Scharf, Dale Seed, Robby
   Simpson, Peter van der Stok, Michael Stuber, Linyi Tian, Gilman
   Tolle, Matthieu Vial, Maciej Wasilak, Fan Xianyou, and Alper Yegin
   for helpful comments and discussions that have shaped the document.
   Special thanks also to the responsible IETF area director at the time
   of completion, Barry Leiba, and the IESG reviewers, Adrian Farrel,
   Martin Stiemerling, Pete Resnick, Richard Barnes, Sean Turner,
   Spencer Dawkins, Stephen Farrell, and Ted Lemon, who contributed in-
   depth reviews.

   Some of the text has been borrowed from the working documents of the
   IETF HTTPBIS working group.

##14， 参考

###14.1	标准参考

  [RFC0768]  Postel, J., "User Datagram Protocol", STD 6, RFC 768,
              August 1980.

   [RFC2045]  Freed, N. and N. Borenstein, "Multipurpose Internet Mail
              Extensions (MIME) Part One: Format of Internet Message
              Bodies", RFC 2045, November 1996.

   [RFC2046]  Freed, N. and N. Borenstein, "Multipurpose Internet Mail
              Extensions (MIME) Part Two: Media Types", RFC 2046,
              November 1996.

   [RFC2119]  Bradner, S., "Key words for use in RFCs to Indicate
              Requirement Levels", BCP 14, RFC 2119, March 1997.

   [RFC2616]  Fielding, R., Gettys, J., Mogul, J., Frystyk, H.,
              Masinter, L., Leach, P., and T. Berners-Lee, "Hypertext
              Transfer Protocol -- HTTP/1.1", RFC 2616, June 1999.

   [RFC3023]  Murata, M., St. Laurent, S., and D. Kohn, "XML Media
              Types", RFC 3023, January 2001.

   [RFC3629]  Yergeau, F., "UTF-8, a transformation format of ISO
              10646", STD 63, RFC 3629, November 2003.

   [RFC3676]  Gellens, R., "The Text/Plain Format and DelSp Parameters",
              RFC 3676, February 2004.

   [RFC3986]  Berners-Lee, T., Fielding, R., and L. Masinter, "Uniform
              Resource Identifier (URI): Generic Syntax", STD 66, RFC
              3986, January 2005.

   [RFC4279]  Eronen, P. and H. Tschofenig, "Pre-Shared Key Ciphersuites
              for Transport Layer Security (TLS)", RFC 4279, December
              2005.

   [RFC4395]  Hansen, T., Hardie, T., and L. Masinter, "Guidelines and
              Registration Procedures for New URI Schemes", BCP 35, RFC
              4395, February 2006.

   [RFC5147]  Wilde, E. and M. Duerst, "URI Fragment Identifiers for the
              text/plain Media Type", RFC 5147, April 2008.

   [RFC5198]  Klensin, J. and M. Padlipsky, "Unicode Format for Network
              Interchange", RFC 5198, March 2008.
 [RFC5226]  Narten, T. and H. Alvestrand, "Guidelines for Writing an
              IANA Considerations Section in RFCs", BCP 26, RFC 5226,
              May 2008.

   [RFC5234]  Crocker, D. and P. Overell, "Augmented BNF for Syntax
              Specifications: ABNF", STD 68, RFC 5234, January 2008.

   [RFC5246]  Dierks, T. and E. Rescorla, "The Transport Layer Security
              (TLS) Protocol Version 1.2", RFC 5246, August 2008.

   [RFC5280]  Cooper, D., Santesson, S., Farrell, S., Boeyen, S.,
              Housley, R., and W. Polk, "Internet X.509 Public Key
              Infrastructure Certificate and Certificate Revocation List
              (CRL) Profile", RFC 5280, May 2008.

   [RFC5480]  Turner, S., Brown, D., Yiu, K., Housley, R., and T. Polk,
              "Elliptic Curve Cryptography Subject Public Key
              Information", RFC 5480, March 2009.

   [RFC5785]  Nottingham, M. and E. Hammer-Lahav, "Defining Well-Known
              Uniform Resource Identifiers (URIs)", RFC 5785, April
              2010.

   [RFC5952]  Kawamura, S. and M. Kawashima, "A Recommendation for IPv6
              Address Text Representation", RFC 5952, August 2010.

   [RFC5988]  Nottingham, M., "Web Linking", RFC 5988, October 2010.

   [RFC6066]  Eastlake, D., "Transport Layer Security (TLS) Extensions:
              Extension Definitions", RFC 6066, January 2011.

   [RFC6347]  Rescorla, E. and N. Modadugu, "Datagram Transport Layer
              Security Version 1.2", RFC 6347, January 2012.

   [RFC6690]  Shelby, Z., "Constrained RESTful Environments (CoRE) Link
              Format", RFC 6690, August 2012.

   [RFC6920]  Farrell, S., Kutscher, D., Dannewitz, C., Ohlman, B.,
              Keranen, A., and P. Hallam-Baker, "Naming Things with
              Hashes", RFC 6920, April 2013.

   [RFC7250]  Wouters, P., Tschofenig, H., Gilmore, J., Weiler, S., and
              T. Kivinen, "Using Raw Public Keys in Transport Layer
              Security (TLS) and Datagram Transport Layer Security
              (DTLS)", RFC 7250, June 2014.

 [RFC7251]  McGrew, D., Bailey, D., Campagna, M., and R. Dugal, "AES-
              CCM Elliptic Curve Cryptography (ECC) Cipher Suites for
              Transport Layer Security (TLS)", RFC 7251, June 2014.


###14.2, Informative References

 [BLOCK]    Bormann, C. and Z. Shelby, "Blockwise transfers in CoAP",
              Work in Progress, October 2013.

   [CoAP-MISC]
              Bormann, C. and K. Hartke, "Miscellaneous additions to
              CoAP", Work in Progress, December 2013.

   [EUI64]    IEEE Standards Association, "Guidelines for 64-bit Global
              Identifier (EUI-64 (TM))", Registration Authority
              Tutorials, April 2010, <http://standards.ieee.org/regauth/
              oui/tutorials/EUI64.html>.

   [GROUPCOMM]
              Rahman, A. and E. Dijk, "Group Communication for CoAP",
              Work in Progress, December 2013.

   [HHGTTG]   Adams, D., "The Hitchhiker's Guide to the Galaxy", Pan
              Books ISBN 3320258648, 1979.

   [IEEE1003.1]
              IEEE and The Open Group, "Portable Operating System
              Interface (POSIX)", The Open Group Base Specifications
              Issue 7, IEEE 1003.1, 2013 Edition,
              <http://pubs.opengroup.org/onlinepubs/9699919799/>.

   [IPsec-CoAP]
              Bormann, C., "Using CoAP with IPsec", Work in Progress,
              December 2012.

   [MAPPING]  Castellani, A., Loreto, S., Rahman, A., Fossati, T., and
              E. Dijk, "Guidelines for HTTP-CoAP Mapping
              Implementations", Work in Progress, February 2014.

   [OBSERVE]  Hartke, K., "Observing Resources in CoAP", Work in
              Progress, April 2014.

   [REC-exi-20140211]
              Schneider, J., Kamiya, T., Peintner, D., and R. Kyusakov,
              "Efficient XML Interchange (EXI) Format 1.0 (Second
              Edition)", W3C Recommendation REC-exi-20140211, February
              2014, <http://www.w3.org/TR/2014/REC-exi-20140211/>.

   [REST]     Fielding, R., "Architectural Styles and the Design of
              Network-based Software Architectures", Ph.D. Dissertation,
              University of California, Irvine, 2000,
              <http://www.ics.uci.edu/~fielding/pubs/dissertation/
              fielding_dissertation.pdf>.

   [RFC0020]  Cerf, V., "ASCII format for network interchange", RFC 20,
              October 1969.

   [RFC0791]  Postel, J., "Internet Protocol", STD 5, RFC 791, September
              1981.

   [RFC0792]  Postel, J., "Internet Control Message Protocol", STD 5,
              RFC 792, September 1981.

   [RFC0793]  Postel, J., "Transmission Control Protocol", STD 7, RFC
              793, September 1981.

   [RFC1035]  Mockapetris, P., "Domain names - implementation and
              specification", STD 13, RFC 1035, November 1987.

   [RFC3264]  Rosenberg, J. and H. Schulzrinne, "An Offer/Answer Model
              with Session Description Protocol (SDP)", RFC 3264, June
              2002.

   [RFC3280]  Housley, R., Polk, W., Ford, W., and D. Solo, "Internet
              X.509 Public Key Infrastructure Certificate and
              Certificate Revocation List (CRL) Profile", RFC 3280,
              April 2002.

   [RFC3542]  Stevens, W., Thomas, M., Nordmark, E., and T. Jinmei,
              "Advanced Sockets Application Program Interface (API) for
              IPv6", RFC 3542, May 2003.

   [RFC3828]  Larzon, L-A., Degermark, M., Pink, S., Jonsson, L-E., and
              G. Fairhurst, "The Lightweight User Datagram Protocol
              (UDP-Lite)", RFC 3828, July 2004.

   [RFC4086]  Eastlake, D., Schiller, J., and S. Crocker, "Randomness
              Requirements for Security", BCP 106, RFC 4086, June 2005.

   [RFC4443]  Conta, A., Deering, S., and M. Gupta, "Internet Control
              Message Protocol (ICMPv6) for the Internet Protocol
              Version 6 (IPv6) Specification", RFC 4443, March 2006.

   [RFC4492]  Blake-Wilson, S., Bolyard, N., Gupta, V., Hawk, C., and B.
              Moeller, "Elliptic Curve Cryptography (ECC) Cipher Suites
              for Transport Layer Security (TLS)", RFC 4492, May 2006.
 
RFC 7252       The Constrained Application Protocol (CoAP)     June 2014


   [RFC4821]  Mathis, M. and J. Heffner, "Packetization Layer Path MTU
              Discovery", RFC 4821, March 2007.

   [RFC4944]  Montenegro, G., Kushalnagar, N., Hui, J., and D. Culler,
              "Transmission of IPv6 Packets over IEEE 802.15.4
              Networks", RFC 4944, September 2007.

   [RFC5405]  Eggert, L. and G. Fairhurst, "Unicast UDP Usage Guidelines
              for Application Designers", BCP 145, RFC 5405, November
              2008.

   [RFC5489]  Badra, M. and I. Hajjeh, "ECDHE_PSK Cipher Suites for
              Transport Layer Security (TLS)", RFC 5489, March 2009.

   [RFC6090]  McGrew, D., Igoe, K., and M. Salter, "Fundamental Elliptic
              Curve Cryptography Algorithms", RFC 6090, February 2011.

   [RFC6120]  Saint-Andre, P., "Extensible Messaging and Presence
              Protocol (XMPP): Core", RFC 6120, March 2011.

   [RFC6282]  Hui, J. and P. Thubert, "Compression Format for IPv6
              Datagrams over IEEE 802.15.4-Based Networks", RFC 6282,
              September 2011.

   [RFC6335]  Cotton, M., Eggert, L., Touch, J., Westerlund, M., and S.
              Cheshire, "Internet Assigned Numbers Authority (IANA)
              Procedures for the Management of the Service Name and
              Transport Protocol Port Number Registry", BCP 165, RFC
              6335, August 2011.

   [RFC6655]  McGrew, D. and D. Bailey, "AES-CCM Cipher Suites for
              Transport Layer Security (TLS)", RFC 6655, July 2012.

   [RFC6936]  Fairhurst, G. and M. Westerlund, "Applicability Statement
              for the Use of IPv6 UDP Datagrams with Zero Checksums",
              RFC 6936, April 2013.

   [RFC6960]  Santesson, S., Myers, M., Ankney, R., Malpani, A.,
              Galperin, S., and C. Adams, "X.509 Internet Public Key
              Infrastructure Online Certificate Status Protocol - OCSP",
              RFC 6960, June 2013.

   [RFC6961]  Pettersen, Y., "The Transport Layer Security (TLS)
              Multiple Certificate Status Request Extension", RFC 6961,
              June 2013.

   [RFC7159]  Bray, T., "The JavaScript Object Notation (JSON) Data
              Interchange Format", RFC 7159, March 2014.

RFC 7252       The Constrained Application Protocol (CoAP)     June 2014


   [RFC7228]  Bormann, C., Ersue, M., and A. Keranen, "Terminology for
              Constrained-Node Networks", RFC 7228, May 2014.

   [RTO-CONSIDER]
              Allman, M., "Retransmission Timeout Considerations", Work
              in Progress, May 2012.

   [W3CXMLSEC]
              Wenning, R., "Report of the XML Security PAG", W3C XML
              Security PAG, October 2012,
              <http://www.w3.org/2011/xmlsec-pag/pagreport.html>.