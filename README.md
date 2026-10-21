# Studio 14

## Overloading and Templates

In this studio, you will explore how overloading of template and non-template functions and operators can be managed to gain the flexibility and specificity that overloading offers while avoiding ambiguity. You will also see how type conversions and scoping affect overloading.

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

   Put that declaration and definition into a new namespace. In `main`, prepend the namespace name and the scope resolution operator (`::`) to one of the calls that prints an object of the class type.

   Compile and run your program. In your answers, show the output it produced. It should show both C-style strings, indicating that the non-template function was used for one call and the function template was used for the others.

8. In this exercise you will predict which overloaded function the compiler selects, then check your predictions. In the steps below, `NS` stands for the namespace you created in the previous exercise and `fn` for the name you gave the function template and the non-template function.

   At the top of `main`, add the using-directive `using namespace NS;`. **Before compiling**, write down in your answers which function (the function template or the non-template function) you predict each of the following calls will use, and briefly explain why. Here, `obj` is one of your objects of the class type and `str` is a `std::string` variable you declare in `main`:

   1. `fn(std::cout, obj);`
   2. `NS::fn(std::cout, obj);`
   3. `fn(std::cout, str);`
   4. `fn(std::cout, "hello");`
   5. `fn<ClassName>(std::cout, obj);`, where `ClassName` is the name of your class
   6. `fn<ClassName>(std::cout, str);`

   Add those calls to `main`, then compile and run your program. In your answers, show the output, and for each call say whether your prediction was correct. For any call you predicted incorrectly, explain what actually determined the choice. Think about exact matches versus conversions (including the implicit conversion your class's constructor allows), and about how the compiler chooses between a template and a non-template function that match equally well.

9. Replace the using-directive `using namespace NS;` in `main` with the using-declaration `using NS::fn;`.

   **Before compiling**, predict in your answers which of the six calls from the previous exercise will still compile, and which function each call that compiles will use. Then compile your program. For each call that does not compile, show the error message and comment that call out. Run the program and show its output.

   In your answers, explain why the using-declaration changed the outcome compared with the using-directive, especially for call 3. Consider which names an unqualified function name finds at block scope, and which additional functions argument-dependent lookup finds based on the types of the arguments.

## Deliverables

Commit and push all modified and added files, including `ANSWERS.md`, to the repo.
