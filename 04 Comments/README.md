```text
Don't comment bad code - rewrite it
```  
--Brian W. Kernighan and P. J. Plaugher

### Don't write comment to explain your code

Usually we write comments to compensate for our failure to express ourselves in code. We must have them because we cannot always figure out how to express ourselves without them.  
See the following sample code:  
```java
   // Check to see if the employee is eligible for full benefits
   if ((employee.flags & HOURLY_FLAG) && (employee.age > 65)) {}
```  
vs  
```java
 if (employee.isEligibleForFullBenefits()) {}
```  
Which one more expressive?

#### Comments are always evolved to follow the code

Code changes and evolves. What if the comments don't evolve with code? The older a comment is, and the farther away it is from the code it describes.

##### Before we write comments

1. Try to express our code  
    1.a. Give a meaningful name.  
    1.b. Put in the right context.
   
2. Think it's the best comment we can write.  
Don't let our reader to read other part of the code to understand the comment.

### Proper use of comment

#### Legal information

For example, copyright and authorship statements.

#### Informative comment

Provide basic information.  
Before we write this kind of comment, think about a descriptive name or move the code to other place to enrich the context.  
```java
   // format matched kk:mm:ss EEE, MMM dd, yyyy
   Pattern timeMatcher = Pattern.compile(“\\d*:\\d*:\\d* \\w*, \\w* \\d*, \\d*”);
```

#### Explanation of intent

Sometimes we need comment to explain the logic behind decision-making.  
```java
   public void testConcurrentAddWidgets() throws Exception {
       WidgetBuilder widgetBuilder = new WidgetBuilder(new Class[]{BoldWidget.class});
       
       String text = ”’’’bold text’’’”;
       ParentWidget parent = new BoldWidget(new MockWidgetRoot(), ”’’’bold text’’’”);
       
       AtomicBoolean failFlag = new AtomicBoolean();
       failFlag.set(false);
       
       //This is our best attempt to get a race condition
       //by creating large number of threads.
       for (int i = 0; i < 25000; i++) {
         WidgetBuilderThread widgetBuilderThread = new WidgetBuilderThread(widgetBuilder, text, parent, failFlag);
         Thread thread = new Thread(widgetBuilderThread);
         thread.start();
       }
       
       assertEquals(false, failFlag.get());
     }
```

#### Clarification

Sometimes this kind of comment use when want to clarify our code because of obscure argument or not readable return value. Usually this happens when we deal with 3rd party library.  
Is ok if there are no better ways. Careful, keep the comment accurate.  
```java
public void testCompareTo() throws Exception {

     WikiPagePath a = PathParser.parse("PageA");
     WikiPagePath b = PathParser.parse("PageB");

     assertTrue(a.compareTo(a) == 0);    // a == a
     assertTrue(a.compareTo(b) != 0);    // a != b
   }
```

#### Warning of consequences

Explain the logic behind decision-making.  
```java
public static SimpleDateFormat makeStandardHttpDateFormat() {
     //SimpleDateFormat is not thread safe,
     //so we need to create each instance independently.
     SimpleDateFormat df = new SimpleDateFormat(”EEE, dd MMM  yyyy HH:mm:ss z”);
     df.setTimeZone(TimeZone.getTimeZone(”GMT”));
     return df;
   }
```

#### TODO

Don't leave TODO comments too long. Put the task into backlog in project management.

#### Amplification

A comment may be used to amplify the importance of something that may otherwise seem inconsequential.  
```java
    String listItemContent = match.group(3).trim();
   // the trim is real important.  It removes the starting
   // spaces that could cause the item to be recognized
   // as another list.
   new ListItemWidget(this, listItemContent, this.level + 1);
   return buildList(text.substring(match.end()));
```

#### Javadoc

Only for public API.

### Bad Comments

#### Mumbling

```java
   public void loadProperties() {
     try {
      String propertiesPath = propertiesLocation + ”/” + PROPERTIES_FILE;
      FileInputStream propertiesStream = new FileInputStream(propertiesPath);
      loadedProperties.load(propertiesStream);
     } catch(IOException e) {
       // No properties files means all defaults are loaded
     }
   }
```  
What the comment mean?

#### Redundant

The comment not more informative than the code itself. Usually we copy-paste part of code as comment.

#### Misleading

```java
    // Utility method that returns when this.closed is true. Throws an exception
   // if the timeout is reached.
   public synchronized void waitForClose(final long timeoutMillis) throws Exception {
      if(!closed){
         wait(timeoutMillis);
         if(!closed)
           throw new Exception("MockResponseSender could not be closed");
      }
   }
```  
The comment not (wrong) explain the code. The code said "if this close", but the comment said "when this close".

#### Mandated

Not all code need comments!  
But all code must expressive!

#### Journal

Sometimes people add a comment to the start of a module every time they edit it. Don't do this. It's source control job.

#### Noise

Comments that are nothing but noise. Think before you write a comment.  
```java
/**
     * Returns the day of the month.
     *
     * @return the day of the month.
     */
    public int getDayOfMonth() {
      return dayOfMonth;
   }
```

#### Copy-paste

Copy-paste? Big No!  
```java
    /** The version. */
   private String version;
   /** The version. */
   private String info;
```

#### Don’t Use a Comment When You Can Use a Function or a Variable

```java
    // does the module from the global list <mod> depend on the
   // subsystem we are part of?
   if (smodule.getDependSubsystems().contains(subSysMod.getSubSystem()))
```  
vs
```java
        ArrayList moduleDependees = smodule.getDependSubsystems();
        String ourSubSystem = subSysMod.getSubSystem();
        if (moduleDependees.contains(ourSubSystem))
```

#### Position markers

```text
// start //////////////////////////////////
```  
A banner is startling and obvious if you don't see banners very often. So use them very sparingly, and only when the benefit is significant.

##### Closing brace

If your code is big enough, maybe you will see this  
```java
try {
          while ((line = in.readLine()) != null) {
            // the logic
          } //while
          // the logic
       } // try
```  
We place comment to mark closing brace.  
Don't do this.  
Remember function must small!

##### Attribution and bylines

```text
/* Added by Fulan */
```  
It's source control job.

##### Commented-out code

Why we need to keep old code? It's source control job to track code changes.  
Remember "the boy scout rule"!

##### HTML

Do we need HTML format in our comment? No. It's not readable. If we want the comment to show up in web pages, let other tool handle it to us.

##### Non local information

Don't put something not related to the context.  
```java
    /**
    * Port on which fitnesse would run. Defaults to 8082.
    *
    * @param fitnessePort
    */
   public void setFitnessePort(int fitnessePort) {
     this.fitnessePort = fitnessePort;
   }
```  
The default port can change and the information not relevant to function.

##### Too much information

Don’t put interesting historical discussions or irrelevant descriptions of details into your comments. Comments are only intended for related code or context.  
It's better to our code can express it.

##### Inobvious connection

Code and comment must be tight coupled.

##### Function header

Do we need this to explain our function? It's better to have:  
1. Small function.  
2. Descriptive function name.  
3. SRP class.

##### Javadoc for private API