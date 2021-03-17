![Formatting Mind Mao](https://drive.google.com/uc?export=view&id=1SONM0Fs-2_ZZonNvb8HGJBaeRgpdOxoJ)

### The purpose of formatting

It's all about _communication_. Different people have different coding styles, they have their own rules. Therefore, we need to make an effort to read other people's code.  
We want everyone who reads our code to have such a good impression that they think the people writing this code are professionals.

“getting it working” not the most important thing for us, a professional developer. The functionality that you create today has a good chance of changing in the next release, but the readability of your code will have a profound effect on all the changes that will ever be made.  
Coding style + readability have an effect on _maintainability_ and _extensibility_ long after the original code has been changed beyond recognition.

### Vertical formatting

#### Like reading a newspaper

We would like a source file to be like a newspaper article.  
* Source file name should be simple but explanatory.  
* The most important concept come first, provide the high-level concepts and algorithms. Then low level details come last.

#### Blank lines separate different concepts

#### Same concept close together

```java
public class ReporterConfig {
     /**
      * The class name of the reporter listener
      */
     private String m_className;
     /**
      * The properties of the reporter listener
      */
     private List<Property> m_properties = new ArrayList<Property>();
     public void addProperty(Property property) {
       m_properties.add(property);
     }
```  
vs  
```java
public class ReporterConfig {
     private String m_className;
     private List<Property> m_properties = new ArrayList<Property>(); 
     public void addProperty(Property property) {
       m_properties.add(property);
     }
```

Beware of comments between lines!  
The tighter the concept the closer the distance. Their vertical separation should be a measure of how important each is to the understandability of the other.

#### Concepts that are closely related should be kept vertically close to each other

Closely related concepts shouldn't be separated into different files. There is no place for protected variables.  
We want to avoid forcing our readers to hop around through our source files and classes.

#### Variable declaration must be close to where it' called

```java
private static void readPreferences() {
     InputStream is= null;
     try {
       is= new FileInputStream(getPreferencesFile());
       // other code
  }
```

#### Instance variables declaration at the top of class

#### Function declaration must be close to where it's called

This gives the program a natural flow. Functions that have direct dependencies are assembled and read from top to bottom.

```java
public class WikiPageResponder implements SecureResponder {

     protected WikiPage page;
     protected PageData pageData;
     protected String pageTitle;
     protected Request request;
     protected PageCrawler crawler;
     
     public Response makeResponse(FitNesseContext context, Request request) throws Exception {
       String pageName = getPageNameOrDefault(request, “FrontPage”);
       loadPage(pageName, context);
       if (page == null)
         return notFoundResponse(context, request);
       else
         return makePageResponse(context);
     }
     
     private String getPageNameOrDefault(Request request, String defaultPageName) {
       //
     }
     
     protected void loadPage(String resource, FitNesseContext context) throws Exception {
       //
     }
     
     private Response notFoundResponse(FitNesseContext context, Request request)  throws Exception {
       //
     }
     
     private SimpleResponse makePageResponse(FitNesseContext context)  throws Exception {
       //
     }
   …
```  
Also applied to function that have similar operation.  
```java
public class Assert {

     static public void assertTrue(String message, boolean condition) {
       if (!condition)
         fail(message);
     }

     static public void assertTrue(boolean condition) {
       assertTrue(null, condition);
     }

     static public void assertFalse(String message, boolean condition) {
       assertTrue(message, !condition);
     }

     static public void assertFalse(boolean condition) {
       assertFalse(null, condition);
     }
   …
```

### Horizontal formatting

#### My personal rule is 80 chars width

#### Use of horizontal spacing

###### Associate things that are strongly related

```java
    private void measureLine(String line)
```

###### Disassociate things that are more weakly related

```java
    int lineSize = line.length();
```

###### Highlight the precedence of operators

```java
    return b*b - 4*a*c;
```

#### No need horizontal alignment

Code in one line read from left to right, not top to bottom.

#### Indentation

A source file is a hierarchy rather like an outline. There is information that pertains to the file as a whole, to the individual classes within the file, to the methods within the classes, to the blocks within the methods, and recursively to the blocks within the blocks.  
To make this hierarchy of scopes visible, we indent the lines of source code in proportion to their position in the hierarchy.

### Team Rules

Every programmer has his own favorite formatting rules, but if he works in a team, then the team rules.  
A good software system is composed of a set of documents that read nicely. They need to have a consistent and smooth style. The reader needs to be able to trust that the formatting gestures he or she has seen in one source file will mean the same thing in others.