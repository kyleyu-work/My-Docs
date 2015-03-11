# Background
<br>
#### Dependency Inversion Principle
Dependency inversion principle is a software design principle which provides us the guidelines to write loosely coupled classes. According to the definition of Dependency inversion principle:
* **High-level** modules should **NOT** depend on **low-level** modules. Both should depend on `abstractions`.
* Abstractions should not depend upon details. Details should depend upon abstractions.

###### Example:
```java
class EventLogWriter
{
    public void Write(string message)
    {
        //Write to event log here
    }
}

class AppPoolWatcher
{
    // Handle to EventLog writer to write to the logs
    EventLogWriter writer = null;

    // This function will be called when the app pool has problem
    public void Notify(string message)
    {
        if (writer == null)
        {
            writer = new EventLogWriter();
        }
        writer.Write(message);
    }
}
```
If the requirement changes, e.g. For some specific set of errors, we will send an e-mail to the administrator, then we have to change the AppPoolWatcher class code. If we have more selective actions, then this situation gets worse. The code above violates the Dependency Inversion Principle because the high level class AppPoolWatcher depends on the concreate class EventLogWriter, not on an abstraction.

# Inversion of Control (IcC)
<br>
So, we use the method of **Inversion of Control (IoC)** to refractor our code
```java
public interface INotificationAction
{
    public void ActOnNotification(string message);
}

class EventLogWriter extends INotificationAction
{   
    public void ActOnNotification(string message)
    {
        // Write to event log here
    }
}

class EmailSender extends INotificationAction
{
    public void ActOnNotification(string message)
    {
        // Send email from here
    }
}

class SMSSender extends INotificationAction
{
    public void ActOnNotification(string message)
    {
        // Send SMS from here
    }
}

class AppPoolWatcher
{
    // Handle to EventLog writer to write to the logs
    INotificationAction action = null;

    // This function will be called when the app pool has problem
    public void Notify(string message)
    {
        if (action == null)
        {
            // Here we will map the abstraction i.e. interface to concrete class 
        }
        action.ActOnNotification(message);
    }
}
```
The next step for us is to bind the concreate class we want to the abstraction. We cannot use `new` directly since it will bring us back to the original point. We will use **Dependency Injection** technique.
<br>
<br>
# Dependency Injection
<br>
Dependency Injection is mainly for injecting the concrete implementation into a class that is using abstraction i.e. interface inside. The main idea of dependency injection is to reduce the coupling between classes and move the binding of abstraction and concrete implementation out of the dependent class.

Dependency injection can be done in three ways:

* Constructor injection
* Method injection
* Property injection

<br>
#### Constructor Injection

```java
class AppPoolWatcher
{
    // Handle to EventLog writer to write to the logs
    INotificationAction action = null;

    public AppPoolWatcher(INotificationAction concreteImplementation)
    {
        this.action = concreteImplementation;
    }

    // This function will be called when the app pool has problem
    public void Notify(string message)
    {   
        action.ActOnNotification(message);
    }
}

// On client side
EventLogWriter writer = new EventLogWriter();
AppPoolWatcher watcher = new AppPoolWatcher(writer);
watcher.Notify("Sample message to log");
```
This Constructor Injection will let the dependent class use one concrete class the entire life time. It is less flexible but the good point is: When you want to use this dependent class, you are always sure that the concrete class binding is all set up.

<br>

#### Method Injection
```java
class AppPoolWatcher
{
    // Handle to EventLog writer to write to the logs
    INotificationAction action = null;

    // This function will be called when the app pool has problem
    public void Notify(INotificationAction concreteAction, string message)
    {
        this.action = concreteAction;
        action.ActOnNotification(message);
    }
}

EventLogWriter writer = new EventLogWriter();
AppPoolWatcher watcher = new AppPoolWatcher();
watcher.Notify(writer, "Sample message to log");
```
It is much more flexible now. But the binding step and the method invocation step must be together.

<br>

#### Property Injection (Setter Injection)

```java
class AppPoolWatcher
{
    // Handle to EventLog writer to write to the logs
    INotificationAction action = null;

    public void setAction(INotification action) {
      this.action = action;
    }
    
    public INotification getAction() {
      return this.action;
    }

    // This function will be called when the app pool has problem
    public void Notify(string message)
    {   
        action.ActOnNotification(message);
    }
}

EventLogWriter writer = new EventLogWriter();
AppPoolWatcher watcher = new AppPoolWatcher();
// This can be done in some class
watcher.setAction = writer;

// This can be done in some other class
watcher.Notify("Sample message to log");
```
The binding process and the method invocation can be done in different classes.

<br>

# Note:

1. Constructor injection is the mostly used approach when it comes to implementing the dependency injection. 
2. If we need to pass different dependencies on every method call then we use method injection. 
3. Property injection is used less frequently.






