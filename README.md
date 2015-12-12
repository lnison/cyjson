# cyjson
Fast and efficent JSON Parser for Python.

This is a wrapper class written in Cython. 
The Underlaying library is [cJSON](https://github.com/kbranigan/cJSON/) by [kbranigan](https://github.com/kbranigan) written in C.


    import cyj
    
    parser = cyj.cyj()
    
    #loading data into cyjson and parsing it internally
    #returns True on success and False on failure
    parser << '{"ab":"test","c":[1,2,{"x":["y","f","w",[6,7,8,10]]},4],"s":{"y":["llüöll",1,false,true]}}'
    
    #information such as types, keywords and item size
    print parser.root_info()
    
    #getting information on elements, raises cyJSON_* exception on error
    print parser >> ('c',2)
    
    #converting items into python objects
    for i in parser.root_info()["keys"]:
    	print parser( ( str(i), ) )
    	
## loading
    parser << "JSON" # will be converted -> .replace("\'","\"")

## converting to python object
This is not efficent, because it converts the whole structure into python objects.

    parser.get_root()
    
## retriving info
Instead of converting to python object, we can collect some information and extract the target value directly from C layer which is faster and more efficent.

    >>> parser.root_info()
    {'keys': [u'ab', u'c', u's'], 'types': ['str', 'list', 'dict'], 'size': 3}
    >>> parser >> ('c',)
    {'keys': [], 'types': ['num', 'num', 'dict', 'num'], 'size': 4}
    
## get value

    >>> parser( ('c',1) )
    2
    >>> parser( ('c',2 ) )
    {'x': [u'y', u'f', u'w', [6, 7, 8, 10]]}
    
    
#install
    git clone https://github.com/mitghi/cyjson/
    cd ./cyjson/cJSON ; make ; make install ; cd ..
    python setup.py build_ext --inplace

#requirements
Cython 0.23
