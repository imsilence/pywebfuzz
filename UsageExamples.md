# Introduction #

The following are a couple of examples of using pywebfuzz. You can also browse the API documentation at:
http://hexsec.com/apidocs/pywebfuzz

# Using utils #

The utils portion of pywebfuzz contains functions that assist in making requests, encoding, as well as range generation.

### Making requests ###

pywebfuzz provides a convenience function to make requests. At the moment only GET and POST are supported but options for more HTTP methods are planned in the future. It takes advantages of Python's built-in urllib2 module to make these requests.

The make\_request function takes a few arguments and returns two values: headers and content.

The default method is HTTP GET. If we just wanted to make a request for the index page of python.org we could do the following:

```
from pywebfuzz import utils

location = "http://python.org"

headers, content = utils.make_request(location)
```

headers =  dictionary value with the returned headers

content = HTML response from the web server

make\_request also has some other options too. You can provide your own HTTP headers in a Python dictionary type as well as add POST data to the request.

```
from pywebfuzz import utils

location = "http://foo.com/submit.php"

headers = {"Host": "foo.com",
                   "User-Agent": "Mozilla/5.0 (X11; U; Linux i686; en-US; rv:1.9.2.3) Gecko/20100423 Firefox/3.6.3",
                   "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8",
                   "Accept-Language": "en-us,en;q=0.5",
                   "Keep-Alive": "115",
                   "Connection": "keep-alive",
                   "Referer": "http://foo.com",
                   "Cookie": ""}

postdata = "foo=bar&high=low"

headers, content = utils.make_request(location, method="POST", postdata=postdata, headers=headers)
```

### Generating a range of values ###

The generate\_range function of pywebfuzz allows you to create a list of values with a start, stop, and step values as well as the ability to prepend and append static values to your generated values. If we wanted to generate a range of values from 5000 to 6000, the following would return a list value with those values.

```
from pywebfuzz import utils

myrange = utils.generate_range(5000, 6000)
```

If we wanted to skip every other value, we could provide a step value of 2.

```
from pywebfuzz import utils

myrange = utils.generate_range(5000, 6000, 2)
```

Where this may really come in handy is having the ability to prepend or append values to the range that you are generating. You may be testing an application that takes order numbers as an input. You may want to iterate though order numbers to see if you can view someone else's order by merely specifying the value. So an example order number might look like:

` WQ6T7088 `

We want to generate a list of values from 6000 to 8000 that have  the characters WQ6T prepended.

```
from pywebfuzz import utils

myrange = utils.generate_range(6000, 8000, pre="WQ6T")
```

# Using fuzzdb #
The following section describes how to use the items outlined in the fuzzdb portion of pywebfuzz.

## Keep in mind ##

The files are read from a binary perspective. This may cause problems with certain libraries since the default encoding scheme for many Python 2.x items is ASCII. So while you are using the values from the fuzzdb portion you may get an error such as:

```
UnicodeDecodeError: 'ascii' codec can't decode byte 0xe2 in position 0: ordinal not in range(128)
```

If you get this all you have to do is provide a decode method for your item and specify utf-8 as the encoding type. Such as:

```
item.decode("utf-8")
```

## Simple Importing ##

As with many modules in Python you have options with regard to how you would like to set up your namespaces. Efforts are made to match these names up with fuzzdb. The exception is that when a hyphen (-) is encountered they are replaced with an underscore (_)._

If you would just like to import the entire namespace of the fuzzdb project you could do the following:

```
from pywebfuzz import fuzzdb
```

If you were only interested in importing only the attack payloads you could do the following:

```
from pywebfuzz.fuzzdb import attack_payloads
```

Some of these namespaces are a bit messy but that's due to the attempt to have them directly match up with the fuzzdb project.

## Simple Usage ##

If we were going to test for LDAP injection we are going to want to use all of the values for this injection from the appropriate namespace. Values in the project are Python list types, which means they are able to be iterated over the top of. The following example would take all of the LDAP injection values and assign them to a variable called ldap\_injection:

```
from pywebfuzz import fuzzdb

ldap_values = fuzzdb.attack_payloads.ldap.ldap_injection
```

## Multiple Values ##

Some of the items in fuzzdb contain multiple values per line. One situation is where username and password value pairs are encountered. When encountering these there is an empty space between the values. Such as "cisco cisco". These values need to be split at this empty space then they can be used the way they were intended. This can be done using standard Python functions. The following example pulls in some default HTTP username and password values and splits them in to user and password.

```
from pywebfuzz import fuzzdb

userpass = fuzzdb.wordlists_user_passwd.generic_listpairs.http_default_userpass

for item in userpass:
     up = item.split(" ")
     user = up[0]
     password = up[1]
```

## Replacing Values ##

There are items in fuzzdb that require replacing before they can be properly used. One example of this is a path traversal attack. The payload values in the traversals\_8\_deep\_exotic\_encoding are one such set that requires replacing.

The payload values are structured as follows:
```
/../{FILE}
/../../{FILE}
/../../../{FILE}
/../../../../{FILE}
```

These {FILE} items must be replaced with the file you are testing for in order for the attack to be successful. Luckily Python has easy string manipulation built in to it. This can be done using the .replace method for the value. The following will iterate through these values and replace the {FILE} items with Foo.bar and print them to the console.

```
from pywebfuzz import fuzzdb

pathvals = fuzzdb.attack_payloads.path_traversal.traversals_8_deep_exotic_encoding

for item in pathvals:
    print(item.replace("{FILE}", "Foo.bar"))
```