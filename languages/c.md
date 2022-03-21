# C
## Strings allocation
strings in code are always saved in the exe data as a constant array with null at the end.

```C
char* str = "secret";
```
this code points to the constant array inside the exe.

```C
char[] str = "secret";
```
this code copies the array into the stack. meaning its editable.
```C
char *a = malloc(256);
strcpy(a, "This is a string");
```
this code copies the array into the heap. also editable.<br>

You cannot change the `"secret"` itself from the exe, because every time the `"secret"` string will be used again its value wouldn't be necessarily `"secret"`.<br>
for example:
```C
char* str = "secret";
str[0] = '5';
printf("secret");
```
this will output **`5ecret`**!<br>...<br>
but you can copy it to writable memory.
## Errors
1. dangling pointer. pointer that points to a variable that does not exsits anymore.
