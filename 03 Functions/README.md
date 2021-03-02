![Meaningful Names mind map](https://drive.google.com/uc?export=view&id=1d4K0ktZwhiu2hNsAHiMsL_HfjE-dYpBA)

##### What is a function?

1. Do something.  
2. Answer something.  
3. Not allowed to do both in one function.

Look at this code  
```java
public boolean set(String attribute, String value);
```  
What if we use like this  
```java
if (set(”username”, ”unclebob”))…
```  
Is there anything weird?

##### Function should do one thing!

"one thing" mean one level of abstraction. You can keep your function small if you strict with this rule.  
Look at this code  
```java
public void sendMailToClients(string[] clients) {
    for (String client : clients) {
        // find client record in db (query)
        if (clientRecord.IsActive())
        {
            // sent mail
        }
    }
}
```  
We can explain what the function does like this "TO sendMailToClients, we iterate clients. In each iteration we query to db to get client record. Then we check if client is active, if true we sent the mail".  
Clearly this function talk too much (query data and check status). Compare with this  
```java
public void sendMailToClients(string[] clients) {
    List<Client> activeClients = getActiveClients(clients);
    for (Client client : activeClients) {
        // sent mail
    }
}

public List<Client> getActiveClients(string[] clients) {
    // find client record in db which status is active
}
```  
We can explain what the function does like this "TO sendMailToClients we get all active client, iterate it. In each iteration we sent the mail".  
The "thing" or abstraction is sent the mail.

If a function does only those steps that are one level below the stated name of the function, then the function is doing one thing.

How we can know if the function doing more than "one thing"? You can extract another function from it with a name that is not merely a restatement of its implementation.

Read the code from top to bottom. Top level is the abstraction, infer it from the name. Read down, and we can see one level (only one) decrease of abstraction. Sample from the book  
```text
To include the setups and teardowns, we include setups, then we include the test page content, and then we include the teardowns.
To include the setups, we include the suite setup if this is a suite, then we include the regular setup.
To include the suite setup, we search the parent hierarchy for the “SuiteSetUp” page and add an include statement with the path of that page.
To search the parent…
```