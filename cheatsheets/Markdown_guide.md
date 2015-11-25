#Markdown/GitHub Cheat Sheet 

Based on [Mastering Markdown](https://guides.github.com/pdfs/markdown-cheatsheet-online.pdf)

##Syntax guide

###Headers and Emphasis

C'mon, you *already* know it! __Jump__ this step!

###Lists

####Unordered

* Item 1
* Item 2
  * Item 2a
  * Item 2b

####Ordered

1. Item 1
2. Item 2
3. Item 3
   * Item 3a
   * Item 3b
  

###Links

E.g. if you haven't, you should subscribe to [SIO BUGS](https://siomail.ucsd.edu/mailman/listinfo/bioinf) asap!

###Blockquotes

BUGS, don't be afraid to be childish, as Pablo Picasso said:

>Every child is an artist. The problem is how to remain an artist once we grow up

###Inline code

If you have some free time, I think you should `grep` an interesting topic and present at next BUGS meeting!

##"Flavored" Markdown

###Syntax highlighting

You can have a javascript:

```javascript
function fancyAlert(arg) {
  if(arg) {
    $.facebox({div:'#foo'})
  }
}
```

Or python:

```python
def foo():
    if not bar:
        return True
```

Or R:

```R
for (beer in 1:inf){
   if (beer==cold)
    new_alcohol_blood_level <- sum(alcohol_blood_level, beer*0.05)
    } else {
     print("I dont want this beer")
   }
 }
```

###Task Lists

- [x] @mentions, #refs, [links](), **formatting**, and <del>tags</del> supported
- [x] list syntax required (any unordered or ordered list supported)
- [x] this is a complete item
- [ ] this is an incomplete item
 
###Tables

First Header | Second Header
------------ | -------------
Content from cell 1 | Content from cell 2
Content in the first column | Content in the second column

