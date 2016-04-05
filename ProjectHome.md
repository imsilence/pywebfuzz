# pywebfuzz #
pywebfuzz is a Python module to assist in the identification of vulnerabilities in web applications through brute force methods. The module does this by providing common testing values along with generators and other utilities that would be helpful when fuzzing web applications.

The project is in its beginning phases so more features will be added in the future.

You can browse the API documentation at: http://hexsec.com/apidocs/pywebfuzz

Also check out the Wiki for more documentation and usage examples.

### fuzzdb Integration ###

pywebfuzz also contains a Python implementation of the fuzzdb project created by Adam Munter http://code.google.com/p/fuzzdb/

fuzzdb is just a collection of values for testing. The point is to provide many of the values of the fuzzdb project cleaned up and available through Python classes and namespaces. This makes it easier to use these values in your own test cases. Effort was made to match the names up similarly to the folders and values files from the fuzzdb project. This effort can sometimes make for some ugly looking namespaces. This balance was struck so that familiarity with the fuzzdb project would cross over in to the Python code. The exceptions come in with the replacement of hyphens with underscores.

The files are read from a binary perspective. This may cause problems with certain libraries since the default encoding scheme for many Python 2.x items is ASCII. So while you are using the values from the fuzzdb portion you may get an error such as:

```
UnicodeDecodeError: 'ascii' codec can't decode byte 0xe2 in position 0: ordinal not in range(128)
```

If you get this all you have to do is provide a decode method for your item and specify utf-8 as the encoding type. Such as:

```
item.decode("utf-8")
```

Namespaces for fuzzdb objects will change as the fuzzdb changes. It's important to check the documentation for these changes.

## About ##

pywebfuzz (C) copyright Nathan Hamiel, 2010

Twitter: https://twitter.com/nathanhamiel

The items in the data directory come from the fuzzdb project:

fuzzdb (C) copyright Adam Munter, 2010