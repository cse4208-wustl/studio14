# Studio 14

## Overloading and Templates

In this studio, you will explore how overloading of template and non-template functions and operators can be managed to gain the flexibility and specificity that overloading offers while avoiding ambiguity. You will also see how type conversions and the placement of declarations affect overloading. This studio supplements the assigned reading for this module (section 16.3).

## Collaboration

You may complete this studio individually or in a small group.

## Exercises

Record your answers in `ANSWERS.md` as you work. Include the names of everyone who worked on the studio in your first answer, and number your responses so they are easy to match to the exercises.

1. List the names of the people who worked together on this studio.

2. SSH into `shell.cec.wustl.edu` using your WUSTL Key credentials, then use `qlogin` to connect to one of the Linux Lab machines and confirm that the version of `g++` there is correct, as you did in [Studio 0](https://github.com/cse4208-wustl/studio0).

   Clone your `studio14` repo using SSH:

   ```bash
   git clone git@github.com:cse4208-wustl/studio14.git
   cd studio14
   ```

   The repo already includes a `Makefile` that builds an executable named `studio14`. As you develop your code for this studio, update the `Makefile` so it lists all of your source and header files.

   Add a source file and a header file in which you declare and define a class that has:

   1. a private member variable of type `std::string`
   2. a public constructor that takes a reference to a const `std::string` and uses it to initialize the private member variable in its base/member initialization list

   In those same files, declare and define a non-member left shift operator (`operator<<`) that takes a non-const reference to a `std::ostream` and a reference to a const object of your class type, inserts the object into the `std::ostream`, and returns a non-const reference to the `std::ostream`. In your class declaration, declare that operator to be a friend of the class.

   Add another source file (for example, `studio14.cpp`) and in it define a `main` function that:

   1. declares two objects of the class type, each initialized with a different string
   2. prints their contents, separated by a space, using `std::cout`
   3. returns a descriptively named symbol with value 0 to indicate success

   Compile and run your program. In your answers, show the output that your program produced.

3. Add a public **const** member `operator<` to your class that takes a reference to a const object of the class type and returns `true` if and only if the object's string member variable is less than the string member variable of the object that was passed to it.

   In `main`, use that operator to determine which of the two objects has the lesser string, and output the result of that comparison to the standard output stream. Compile and run your program. In your answers, show the output it produced.

4. In your class's source and header files, comment out the declaration and definition of the member `operator<` you added in the previous exercise. Instead, declare and define a non-member `operator<` that is a friend of the class, takes two references to const objects of the class type, and performs the same comparison.

   Compile and run your program and confirm that the output is the same as in the previous exercise. In your answers, show the declaration and definition of the non-member `operator<`.

5. Uncomment the declaration and definition of the member version of `operator<` and compile your program. Note the warning or error that occurs. If you only got a warning (that is, the code compiled), run your program and confirm that the output is the same as in the previous exercise.

   In your answers:

   1. show the warning or error you got
   2. briefly explain why it occurred

6. Comment out the declaration and definition of the member version of `operator<` again.

   Add a template header file and a template source file to this repo, with the template header file including the template source file, and update the `Makefile` accordingly. In them, declare and define a function template with a single type parameter that:

   1. has a `void` return type
   2. takes a reference to a non-const `std::ostream` and a reference to a const variable of the parameterized type
   3. inserts the variable into the `std::ostream`

   Include the template header file in the source file for your `main` function (or in its header file, if you created one), and in `main` replace every place that inserts an object of the class type into an `std::ostream` with a call to the function template.

   Compile and run your program and confirm that it produces the same output as in the previous exercise (or the one before it, if the previous one did not compile). In your answers, show the declaration and definition of the function template.

7. Modify the function template from the previous exercise so that it also inserts a short C-style string into the `std::ostream` to indicate that printing happened from the template function.

   Add a header file for your `main` function's source file if you do not already have one. In that header and source file, declare and define a non-template function that:

   1. has the same name as the function template
   2. has a `void` return type
   3. takes a reference to a non-const `std::ostream` and a reference to a const object of your class type
   4. inserts the object into the `std::ostream`, and also inserts a short C-style string to indicate that printing happened from the non-template function

   **Before compiling**, predict in your answers which function (the function template or the non-template function) each call in `main` that prints an object of the class type will use. Then compile and run your program, and show the output it produced.

   In your answers, explain why the non-template function was called, even though the function template could also be instantiated to match those calls exactly.

   Finally, change one of those calls so that it provides an explicit template argument (for example, `fn<ClassName>(std::cout, obj)`, where `fn` is the name of your function template and `ClassName` is the name of your class). Compile and run your program, show the output, and explain why that call now uses the function template.

8. In this exercise you will overload your function template with a second, more specialized function template, as in the `debug_rep` examples in the assigned reading for this module (section 16.3). In the steps below, `fn` stands for the name of your function template, `ClassName` for the name of your class, and `obj` for one of your objects of the class type.

   In your template header file, add a declaration of a second function template named `fn` with a single type parameter `T` that has a `void` return type and takes a reference to a non-const `std::ostream` and a parameter of type `T*`. Make sure both function templates are declared in the template header file before either one is defined. In your template source file, define it so that it:

   1. inserts the pointer's own value into the `std::ostream`, along with a short C-style string indicating that printing happened from the pointer template
   2. if the pointer is not null, calls `fn` to print the object it points to; otherwise inserts a message indicating a null pointer

   In `main`, declare a `const` pointer to one of your objects (of type `const ClassName*`), a null pointer of type `ClassName*`, and a `std::string` variable `str`. **Before compiling**, predict in your answers which function (the `const T&` template, the `T*` template, or your non-template function from the previous exercise) each of the following calls will use, and what `T` will be deduced as for each template call. For calls that use the `T*` template, also predict which function prints the object that the pointer points to. Briefly explain why, using the function-matching rules from section 16.3:

   1. `fn(std::cout, obj);`
   2. `fn(std::cout, &obj);`
   3. `fn(std::cout, cp);`, where `cp` is your `const ClassName*`
   4. `fn(std::cout, str);`
   5. `fn(std::cout, "hello");`
   6. `fn(std::cout, np);`, where `np` is your null `ClassName*`

   Add those calls to `main`, then compile and run your program. In your answers, show the output, and for each call say whether your prediction was correct. For any call you predicted incorrectly, explain what actually determined the choice. In particular, explain the output of call 5: why is the pointer's value not printed as an address, and why does only one character of the string get printed after it?

9. In this exercise you will add non-template overloads for strings and see how a missing declaration can silently change which function gets called.

   In the source file for your `main` function, above `main` and outside any namespace (do not declare these functions in a header file), define two non-template functions named `fn`, each with a `void` return type and taking a reference to a non-const `std::ostream` as the first parameter. **Define them in this order:**

   1. a function whose second parameter is a `const char*`, which inserts a short C-style string indicating that printing happened from the `const char*` non-template function, and then calls `fn` on a `std::string` constructed from the pointer
   2. a function whose second parameter is a reference to a const `std::string`, which inserts the string surrounded by double quotes, along with a short C-style string indicating that printing happened from the `std::string` non-template function

   **Before compiling**, predict in your answers which functions calls 4 and 5 from the previous exercise will now use. For call 5, also predict which function the `const char*` function will call. Then compile and run your program, show the output, and say whether your predictions were correct.

   Next, add a declaration of the `std::string` version of `fn` above the definition of the `const char*` version. **Before compiling**, predict how the output of call 5 will change. Then compile and run your program and show the output.

   In your answers, explain why adding that declaration changed the output, even though the program compiled and ran without it. Also explain how the advice in section 16.3 to declare every function in an overload set before defining any of them would have prevented the problem.

## Deliverables

Commit and push all modified and added files, including `ANSWERS.md`, to the repo.
