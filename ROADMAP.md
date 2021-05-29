 # TODO
 
### imports     

- [ ] from Crypto.PublicKey import RSA => load("@vendor//Crypto/PublicKey/RSA", "RSA")
- [x] relative imports: 
  1. from .base import y => load("@vendor//pkgname/base/y", y="y")  

- [ ] warn on unknown symbols
  1. potential corner case: self.xx in dedented functions not in an enclosing function

### desugaring

- [ ] decorators should be desugared
```python
  # https://github.com/pydron/pydron/blob/a7b484dec8bcc2730ba9bd76bc63bf3362c05e4d/pydron/translation/dedecorator.py
    def visit_FunctionDef(self, t):
        t = self.generic_visit(t)
        fn = Function(t.name, t.args, t.body)
        for d in reversed(t.decorator_list):
            fn = Call(d, [fn])
        result = ast.Assign([ast.Name(t.name, store)], fn)
        return ast.copy_location(result, t)
```
- [ ] set literals should be desugared from {} to sets.make()
  

  
### complex translations

- rewrite try/except statements

- Rewrite lib2to3 fixers to libcst
 - Particularly the test generation stuff should be easy to port tests

- integrate lib3to6?
  
- rewrite:
    - a = b = "xyz" to:
      - a = "xyz"
      - b = a
    
- decode()/encode() should be translated to codecs.encode()/codecs.decode()

### exceptions

- [x] Exceptions => fail(), fix up the strings
  - [x] test string formatting cases as well
    
- [ ] ~~fail("TypeError(\"xxxxxx\")") => fail("TypeError: xxxxxx")~~
  - [x] with introduction of `Error` `Ok` object, we no longer need to fail()

### operators

- ** to pow
- X @ Y = operator.matmul(x,y)..


### bytes:
  
- [x] starlark bytes support was merged into starlarky (upstream!) 
  
- if byte literals are in ascii range, do not escape them by converting them to 
  hex digits 
  
  i.e. bytes([0x70, 0x61, 0x73, 0x73, 0x77, 0x6F, 0x72, 0x64]) == bytes("password", encoding="utf-8") == b"password"
  

### misc:

- remove if __name__ == '__main__'..

-  if methods are referenced in the class, then they should be ordered so that 
   they are defined first before invoking them in init? test this!