# Design Patterns In Action

In this tutorial, I provide a clear and concise explanation of each design pattern, including its purpose, components, and benefits. I also provide examples of how each pattern can be used in real-world scenarios, so that developers can see how they might apply these patterns to their own projects.

## Adapter Pattern

Adapter Pattern used to convert the interface of an existing class into another interface the client expects. This allows classes with incompatible interfaces to work together by wrapping the incompatible interface with an adapter. The adapter acts as a bridge between the two interfaces, enabling them to communicate and work together.

Adapter Design Pattern is particularly useful when integrating third-party libraries or services, adapting legacy code to meet new requirements, and implementing compatibility between incompatible interfaces. By using Adapter Design Pattern, developers can save time and effort, and avoid complex and error-prone code.

The Adapter Pattern consists of three main components:

- Target interface: The interface that the client expects to interact with.
- Adaptee: The existing class or interface that needs to be adapted to the target interface.
- Adapter: The class that adapts the Adaptee to the Target interface.

Let's take an example of an e-commerce application that needs to integrate with different payment gateways to process payments. The application has a PaymentGateway interface that defines the contract for processing payments:

```java
public interface PaymentGateway {
    void processPayment(String paymentType, double amount);
}
```
The application initially supports payments through a Adyen gateway, which implements the PaymentGateway interface:

```java
public class AdyenGateway implements PaymentGateway {
    public void processPayment(String paymentType, double amount) {
        // process payment using Adyen 
    }
}
```
Later, the application needs to integrate with a new payment gateway called Stripe which has a different interface than the PaymentGateway interface. 
To integrate Stripe, we need to use the Adapter Pattern.

We can create a StripePaymentGatewayAdapter class that adapts the Stripe interface to the PaymentGateway interface:

```java
public class StripePaymentGatewayAdapter implements PaymentGateway {
    private StripePaymentGateway stripeGateway;

    public StripePaymentGatewayAdapter(StripePaymentGateway stripeGateway) {
        this.stripeGateway = stripeGateway;
    }

    public void processPayment(String paymentType, double amount) {
        // convert the parameters to the Stripe gateway format
        String stripePaymentType = convertPaymentType(paymentType);
        int stripeAmount = (int) (amount * 100);

        // call the Stripe gateway to process the payment
        stripeGateway.processStripePayment(stripePaymentType, stripeAmount);
    }

    private String convertPaymentType(String paymentType) {
        // convert the payment type to Stripe format
        // ...
    }
}
```
In this example, the StripePaymentGatewayAdapter class adapts the StripePaymentGateway interface to the PaymentGateway interface. The adapter takes a StripePaymentGateway object as a constructor argument and uses it to process the payment. The adapter also converts the payment type and amount to the format expected by the StripePaymentGateway.

This implementation respects the SOLID principles because:

- Single Responsibility Principle: The StripePaymentGatewayAdapter has only one responsibility, which is to adapt the StripePaymentGateway to the PaymentGateway interface.

- Open/Closed Principle: The PaymentGateway interface is closed for modification, but open for extension. The application can easily support new payment gateways by creating new adapters that implement the PaymentGateway interface.

- Liskov Substitution Principle: The StripePaymentGatewayAdapter can be used wherever a PaymentGateway object is expected, without affecting the behavior of the application.

- Interface Segregation Principle: The PaymentGateway interface defines a single method that encapsulates the behavior of processing payments. Clients that use the PaymentGateway interface are not affected by the internal implementation of the payment gateway.

- Dependency Inversion Principle: The StripePaymentGatewayAdapter depends on abstractions (the PaymentGateway interface) rather than on concretions (the StripePaymentGateway). This makes the adapter more flexible and easier to test.

### Template Method Pattern

Template Method Pattern is a behavioral design pattern that defines the skeleton of an algorithm in a superclass but allows subclasses to override specific steps of the algorithm without changing its structure. It is used when we have an algorithm that has several steps, some of which may be common to different subclasses, and we want to avoid duplicating code.

Let's take an example in the context of a Bank application. 
Suppose we have a Bank class with a method called "processLoan". The processLoan method performs a series of steps to approve or reject a loan application. These steps include verifying the applicant's credit score, income, and employment status. However, the specific details of how these verifications are performed may vary depending on the type of loan and the bank's policies.

To implement the Template Method Pattern, we would create an abstract class called "LoanProcessor" that defines the skeleton of the loan processing algorithm. This abstract class would have a method called "processLoan" that calls several other abstract methods, such as "checkCreditScore", "checkIncome", and "checkEmploymentStatus". These abstract methods would be implemented by concrete subclasses of "LoanProcessor" for different types of loans and different bank policies.

Here's an example implementation:

```java
public abstract class LoanProcessor {
    
    public final boolean processLoan(LoanApplication loanApplication) {
        if (!checkEligibility(loanApplication)) {
            return false;
        }
        
        if (!checkCreditScore(loanApplication)) {
            return false;
        }
        
        if (!checkIncome(loanApplication)) {
            return false;
        }
        
        if (!checkEmploymentStatus(loanApplication)) {
            return false;
        }
        
        return true;
    }
    
    protected abstract boolean checkEligibility(LoanApplication loanApplication);
    
    protected abstract boolean checkCreditScore(LoanApplication loanApplication);
    
    protected abstract boolean checkIncome(LoanApplication loanApplication);
    
    protected abstract boolean checkEmploymentStatus(LoanApplication loanApplication);
}
```
In this example, the LoanProcessor class defines the template method "processLoan" and several abstract methods that must be implemented by concrete subclasses. The concrete subclasses, such as PersonalLoanProcessor and HomeLoanProcessor, implement the abstract methods to perform the specific checks required for each loan type:

```java
public class PersonalLoanProcessor extends LoanProcessor {
    
    @Override
    protected boolean checkEligibility(LoanApplication loanApplication) {
        // implementation specific to personal loans
    }
    
    @Override
    protected boolean checkCreditScore(LoanApplication loanApplication) {
        // implementation specific to personal loans
    }
    
    @Override
    protected boolean checkIncome(LoanApplication loanApplication) {
        // implementation specific to personal loans
    }
    
    @Override
    protected boolean checkEmploymentStatus(LoanApplication loanApplication) {
        // implementation specific to personal loans
    }
}

public class HomeLoanProcessor extends LoanProcessor {
    
    @Override
    protected boolean checkEligibility(LoanApplication loanApplication) {
        // implementation specific to home loans
    }
    
    @Override
    protected boolean checkCreditScore(LoanApplication loanApplication) {
        // implementation specific to home loans
    }
    
    @Override
    protected boolean checkIncome(LoanApplication loanApplication) {
        // implementation specific to home loans
    }
    
    @Override
    protected boolean checkEmploymentStatus(LoanApplication loanApplication) {
        // implementation specific to home loans
    }
}
```

The Template Method Pattern respects the SOLID principle of the Open/Closed Principle, as it allows us to add new loan types or new verification steps without modifying the existing loan processing algorithm. We can simply create new subclasses of LoanProcessor and override the necessary methods. It also promotes code reuse and modularity by separating the common loan processing steps from the specific loan type or verification step implementations.
