### My note about Clean Code

### [Meaningful Names](https://github.com/bluething/cleancode/tree/main/02%20Meaningful%20Names)

1. Your name must answer this question:  
 1.a. Why it exists?  
 1.b. What it does?  
 1.c. How it is used?  
2. Choose descriptive and unambiguous names.  
3. Make meaningful distinction.  
4. Use pronounceable names.  
5. Use searchable names.  
6. Don't use prefix.  
7. Use domain name.

### [Functions](https://github.com/bluething/cleancode/tree/main/03%20Functions)

1. Do one thing.  
2. Use descriptive names.
3. Zero or one argument.  
4. No output argument.  
5. No flag argument.  
6. Have no side effect.  
7. DRY.  
8. Fail fast.

### [Comments](https://github.com/bluething/cleancode/tree/main/04%20Comments)

1. Don't write comment to explain your code.  
2. Try to express our code.  
 2.a Give a meaningful name.  
   2.b Put in the right context.
3. Comments are always evolved to follow the code.

### [Formatting](https://github.com/bluething/cleancode/tree/main/05%20Formatting)

1. It's all about communication.  
2. Vertical formatting is like reading a newspaper.  
3. 80 chars width.  
4. If we work in a team, use team rules (create it if you don't have).

### [Object and Data Strcutures](https://github.com/bluething/cleancode/tree/main/06%20Object%20and%20Data%20Structure)

1. Everything is an object is a myth.  
2. Avoid hybrids structures (half object and half data).  
3. Keep class field private and final.  
4. Obey the Law of Demeter (LoD).  
5. Don't use setter or getter.

### [Error Handling](https://github.com/bluething/cleancode/tree/main/07%20Error%20Handling)

1. Avoid using exception for flow control.  
2. Use checked exception.  
3. Don't return NULL.  
4. Don't pass NULL.  
5. Use fail fast.  
6. Don't catch until you have to.  
7. Provide context with exception.

### [Boundaries](https://github.com/bluething/cleancode/tree/main/08%20Boundaries)

1. Try to encapsulate any boundary interface inside a class.  
 1.a. Avoid returning it or accepting it as an argument.  
2. Learn your boundaries by writing a tests.  
3. If your boundaries not yet available, create it.  
 3.a. You can use adapter pattern as a bridge when they are available to use.

Reference:  
_Martin, Robert C. 2009. Clean code: a handbook of agile software craftsmanship. Upper Saddle River, NJ: Prentice Hall._  
_Bugayenko, Yegor. 2017. Elegant Object Volume 1. CreateSpace Independent Publishing Platform; 1.0 edition (February 17, 2016)_  
_Bugayenko, Yegor. 2017. Elegant Object Volume 2. CreateSpace Independent Publishing Platform; 1.4 edition (April 18, 2017)_