# Programming journy

## Projects
- [ ] a hypervioser
- [ ] RPG game
- [ ] decentralize OS
- [ ] [rust os](https://os.phil-opp.com/)


## Languages
- [x] C
- [x] C++
- [ ] C#
- [x] Python
- [x] Go
- [x] Java
- [ ] Java script
- [ ] Rust
- [x] Assembly
- [ ] R
- [ ] Swift
- [ ] PHP
- [ ] Dart

## General language knowlagde
## operation time:
 - late / dynamic / virtual - on runtime
 - early / static - on compilation time

### name binding 
The process of associating data or code with an identifier (uid).<br>
name binding occures at linkage time.
```
hold up:
var = 5;
function = &func

uuid table
---------------------------
var_uuid      | bp + var_offset
var_uuid      | bp + var_offset
function_uuid | location in .text memory
---------------------------

the code after name binding

```

### dispatching
the process of chosing from the virtual table the apropriate identifier (via name bindings).<br>

how does it look? the BASE class object is replaced<br>
**before dispatching**
```C++
base = derive()
base_uid = derive_uid
```
**after dispatching**
```C++
derive = derive()
```
### double dispatch

### multiple dispatch
multiple dispatch is a language propertiy in which a data or code can be dispatch in runtime.

### linking code
The process of combining all given object file and shared libraries to one executable.<br>

