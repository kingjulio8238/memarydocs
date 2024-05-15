# Welcome to memary 

For full documentation visit [mkdocs.org](https://www.mkdocs.org).

## Code Annotation Examples 

## Codeblocks 

some `code` goes here. <!-- to highlght code -->

### Plain codeblock 

A plain codeblock: 

<!-- to have entire codeblock -->
``` 
some code here 
def myfunction()
// some comment 
``` 

#### Code for a specific 

Some more code with the `py` at the start: 

``` py
import tensorflow as tf
def whatever()
``` 

#### Code with a title 
``` py title="bubble_sort.py"
def bubble_sort(items):
    for i in range(len(items)):
        for j in range (len(items) -1 - i):
            if items[j] > items[j + 1]:
                items[j], items[j + 1] = items[j + 1], items[j]
```

#### Code with line numbers  
``` py linenums="1"
def bubble_sort(items):
    for i in range(len(items)):
        for j in range (len(items) -1 - i):
            if items[j] > items[j + 1]:
                items[j], items[j + 1] = items[j + 1], items[j]
```

#### Highlight lines 
``` py hl_lines="2 3"
def bubble_sort(items):
    for i in range(len(items)):
        for j in range (len(items) -1 - i):
            if items[j] > items[j + 1]:
                items[j], items[j + 1] = items[j + 1], items[j]
```

## Icons and Emojis 

:smile: 

:fontawesome-regular-face-laugh-wink: 

:fontawesome-brands-twitter:{ .twitter }

:octicons-heart-fill-24:{ . heart }