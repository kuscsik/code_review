
## Commit messages

```
 [Jira reference] Short log / Statement of what needed to be changed.
  
  (Pointers to external resources, such as defect tracking in Jira)
  
  The intent of your change.
  
  (Optional, if it's not clear from above) how your change resolves the
  issues in the first part.
 ```
## Code review

* The code is well-designed.
* The functionality is good for the users of the code.
* Any parallel programming is done safely.
* The code isn’t more complex than it needs to be.
* The developer isn’t implementing things they might need in the future but don’t know they need now.
* Tests are well-designed.
* The developer used clear naming.
* Comments are clear and useful, and mostly explain why instead of what.
Code is appropriately documented.
* Code does not violate any licenses and copyrights.
* Do not use sleep function unless there is absolutely no other way to achieve a functionality.
* Avoid creating services that serve only one hardware configuration. Hardware specific interfaces should be abstracted * away with hardware abstraction layers.
* Always assume that RPC services are called from multiple processes. Handling of RPCs must be always thread safe.
* Code must not break other system functionalities.
* Keep functions short. Ideally functions are max 25 lines and should not exceed 50 lines.
* In general, reviewers should favor approving a patch set once it is in a state where it definitely improves the overall code health of the system being worked on, even if the patch set isn’t perfect.

## C++ Guidelines
* Use C++ standards in .cpp files and C standards in .c files.
* Avoid using global variables whenever it is possible.
* Prefer compile time checking to run-time checking
* Do not leak any resources, memory, file descriptors.
* Use scoped objects, don't allocate local variables on heap.
* Avoid malloc/free, use new and delete.
* Use unique_ptr or smart_ptr to represent ownership.
* Avoid redundant code.
* Do not comment out large sections of code except the code is used to enable debug features/logging. Git history will keep track of the removed code.
* Use std:thread for threading instead of pthread.
* Use switch/case instead of if/then whenever it is possible.
* Best practices learned in C do not apply for C++. Seek for switching perspective when moving from C to C++.
* Use singletons where it is absolutely necessary.
* Don't use memset for zeroing C++ structs. Use constructor or default values in the struct.Consider the following bad example:
```
struct dataType
{
    int a;
    std::string b;
};
{
    dataType x;
    memset(&x, 0, sizeof dataType); // <- b string type gets corrupted
}
```

* Use #pragma once  include guards for protecting headers against multiple inclusions.
* Handle error return values gracefully. For example, it is not acceptable to log an error and keep the application running in a state where it can no longer perform the task designed for.
* Avoid magic constants. 
Example. This is bad:
```
    enable_bluetooth_function(0x434493)
```
This is Good:
```
    #define BLUETOOTH_FUNCTION_PAIRING 0x434493
    
    ... 
    {
       enable_bluetooth_function(BLUETOOTH_FUNCTION_PAIRING)
    }
```
## String guidelines
* User strncpy instead of strcpy. strcpy is not safe.
* Use std::string whenever it is possible.

## Parallels and multi-process programming
* Avoid state synchronizations between processes. RPC clients should never make assumption about a service being in a given internal state.
* Make all IPC calls thread safe.
* Any RPC/Service interface that is exposed to applications shall be documented. Other people should not spend time reading your code to figure out what values function values valid or what are the potential return values.

## Mentoring
Code review can have an important function of teaching developers something new about a language, a framework, or general software design principles. It’s always fine to leave comments that help a developer learn something new. Sharing knowledge is part of improving the code health of a system over time. If your comment is purely educational, but not critical to meeting the standards described in this document, prefix it with “Nit: “ or otherwise indicate that it’s not mandatory for the author to resolve it in this CL.
