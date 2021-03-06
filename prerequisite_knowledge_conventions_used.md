# Prerequisite Knowledge / Conventions Used

### Prerequisite Knowledge
* You should know basic Ruby syntax.

* Bourne shell knowledge will be useful, but not required.

* You do not need to know C, though I'll refer to and map Ruby methods to corresponding or analogous C functions.

* You must know that that two (or more) variables (in Ruby) can refer to the same underlying object.  This concept is present in Unix, so it is /very/ important one understand this:

        a = "this is a string"
        b = a      # 'b' refers to the same String object as 'a'
        a << "!"   # modifies the String object 'a' points to

        "this is a string!" == a

        # You should understand why the following statements are true:
        "this is a string!" == b
        "this is a string" != b

### Notations/Conventions
* Ruby method documentation references (should be the same as Ruby documentation)

 - IO.pipe -- class/singleton method
 - IO#stat -- instance method

* C function documentation references

 - pipe(2) -- "#{function_name}(#{section})"
               The C function manpage can be accessed as:
                    man #{section} #{function_name}
               so you can open documentation for pipe(2) using
               the command: man 2 pipe


That's all I can think of for now...


## License: GPLv3 (or later, at the discretion of Eric Wong) http://www.gnu.org/licenses/gpl-3.0.txt
