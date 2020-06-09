# sipcounter
This module implements a simple, stateless SIP message counter with optional message flow direction, IP address, protocol and port tracking. When provided with the IP address/protocol/port information in addition to the mandatory SIP message body it counts the SIP requests and responses for each communication link separately. A link thus is comprised of the SIP UA server and client IP address, the server and client side ports and the transport protocol type (TLS, TCP, UDP) which can also be inferred from the SIP message body if not supplied. It's 'data' dictionary holds the links as keys and a nested dictionary as values which contain another layer of dictionaries for each direction and Counters that keep track of the number of SIP message types sent or received on the link.
For quick visualization it comes with a 'pprint' convenience method. In addition the SIPCounter object has numerous methods similar to those available for the Counter class from the collections module in the standard library and more.

### Example ###
To count the number of INVITE and ReINVITE messages and any error responses (4xx, 5xx, 6xx) received in these dialogs:

```
>>> from sipcounter import SIPCounter
>>> reader = sip_msg_reader()
>>> c = SIPCounter(name='SBC Cone-A', sip_filter=set(['INVITE', '4', '5', '6']))
>>> while True:
...     try:
...         # the 'reader' is SIP log parser generator
...         timestamp, sipmsg, msgdir, srcip, srcport, dstip, dstport = next(reader)
...         c.add(sipmsg, msgdir, srcip, srcport, dstip, dstport)
...     except:
...         print(c.tostring(title='2020-0101 01:01:00'))
...         break
KeyboardInterrupt

    2020-0101 01:01:00          INVITE   ReINVITE    503       600
    SBC Cone-A                ---> <--- ---> <--- ---> <--- ---> <---
    1.1.1.1-TCP-5060-2.2.2.1    13   10   40   40    0    0    0    0
    1.1.1.1-TLS-5061-2.2.2.1    13   10   36   42    1    0    1    0
    SUMMARY                     26   20   76   82    1    0    1    0
```
## Requirements

- Python 2.7-3.x

## License

MIT, see: LICENSE.txt

## Author

Szabolcs Szokoly <a href="mailto:sszokoly@pm.me">sszokoly@pm.me</a>
