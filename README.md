# Abstract Class vs Interface in C# --- Real-World Payment System Example

This document explains when to use an abstract class vs an interface
using a real enterprise .NET payment architecture example.

------------------------------------------------------------------------

## 📌 Core Idea

-   Interface → defines capability/contract
-   Abstract class → provides shared behavior + base implementation

In real-world applications, both are often used together.

------------------------------------------------------------------------

# 1️⃣ Using Interface for Payment Processor

Use when: - multiple implementations - dependency injection - loose
coupling

``` csharp
public interface IPaymentProcessor
{
    Task ProcessPayment(decimal amount);
}
```

### Implementations

``` csharp
public class RazorpayProcessor : IPaymentProcessor
{
    public Task ProcessPayment(decimal amount)
    {
        // Razorpay payment logic
        return Task.CompletedTask;
    }
}

public class StripeProcessor : IPaymentProcessor
{
    public Task ProcessPayment(decimal amount)
    {
        // Stripe payment logic
        return Task.CompletedTask;
    }
}
```

Why interface? - Swap providers easily - Test with mocks - Works with DI
container

------------------------------------------------------------------------

# 2️⃣ Using Abstract Class for Shared Logic

Use when: - common validation - logging - retry logic - shared fields

``` csharp
public abstract class BasePaymentProcessor : IPaymentProcessor
{
    protected void Validate(decimal amount)
    {
        if (amount <= 0)
            throw new Exception("Invalid amount");
    }

    public abstract Task ProcessPayment(decimal amount);
}
```

------------------------------------------------------------------------

# 3️⃣ Implementation Using Abstract Base

``` csharp
public class RazorpayProcessor : BasePaymentProcessor
{
    public override Task ProcessPayment(decimal amount)
    {
        Validate(amount);
        // Razorpay API logic
        return Task.CompletedTask;
    }
}
```

------------------------------------------------------------------------

# 🧠 Real Enterprise Decision Rule

  Scenario                             Use Interface   Use Abstract Class
  ------------------------------------ --------------- --------------------
  Define contract                      ✅              ❌
  Shared logic                         ❌              ✅
  Dependency injection                 ✅              ⚠️
  Code reuse                           ❌              ✅
  Multiple unrelated implementations   ✅              ❌
  Same family of classes               ❌              ✅

------------------------------------------------------------------------

# 🎯 Interview-Ready Answer

Yes, payment processor can be created using interface.

But when common logic like logging, validation, retry, and configuration
is required, abstract class is used to avoid duplication.

In real .NET systems, both interface and abstract class are used
together: - Interface → flexibility & DI - Abstract → code reuse & base
behavior - Implementation → actual business logic

------------------------------------------------------------------------
# 60-second interview answer (Interface vs Abstract Class): English Version
Both interface and abstract class are used for abstraction in C#, but they serve different purposes.
An interface defines a contract — what a class should do. It contains no implementation and is mainly used for dependency injection, loose coupling, and multiple implementations in .NET applications.

An abstract class is used when related classes need to share common logic, fields, or base behavior. It can contain both abstract methods and implemented methods, which derived classes can reuse and extend.

In real projects, we often use both together — interfaces for flexibility and architecture, and abstract classes for code reuse and shared behavior.

For example, in a payment system, an IPaymentProcessor interface defines the contract, while a BasePaymentProcessor abstract class handles common logic like logging, validation, and retries.

------------------------------------------------------------------------

# 60-second interview answer (Interface vs Abstract Class): Hinglish Version
Interface aur abstract class dono abstraction ke liye use hote hain, but purpose alag hota hai.
Interface ek contract define karta hai — kya karna hai. Isme implementation nahi hoti, aur multiple classes isko implement kar sakti hain. Ye DI, loose coupling, aur testing me heavily use hota hai .NET me.

Abstract class tab use hoti hai jab related classes me common logic, fields, ya base behavior share karna ho. Isme partial implementation hoti hai aur derived classes usko extend karti hain.

Real projects me usually dono saath use hote hain — interface flexibility ke liye aur abstract class code reuse ke liye.

Example: Payment system me IPaymentProcessor interface contract define karta hai, aur BasePaymentProcessor abstract class logging, validation, retry jaise common logic handle karti hai.

------------------------------------------------------------------------

# ⭐ Senior-Level Takeaway

Interface defines WHAT system can do.
Abstract class defines HOW common behavior is shared.
Concrete classes define ACTUAL business logic.
