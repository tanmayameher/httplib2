# Introduction #

This design document is preliminary and should be discussed on the
httplib2-dev mailing list.


# Details #

```
class HttpAsync:
  def __init__(self, cache, onerror_cb, server_initiated_cb):
    pass

  def request(self, method, body, ...., response_cb)
    pass

  def selectfds():
    """
    returns (rlist, wlist, xlist) suitable for passing to select.select().
    """
    pass

  def selectedfds(rlist, wlist, xlist):
    """
    takes in 3 lists as would be returned from select.select().
    """
    pass
  
```