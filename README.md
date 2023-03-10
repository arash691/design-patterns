# Design Patterns In Action

In this tutorial, I provide a clear and concise explanation of each design pattern, including its purpose, components, and benefits. I also provide examples of how each pattern can be used in real-world scenarios, so that developers can see how they might apply these patterns to their own projects.

## Adaptor Design Pattern

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

Another example is the Spring _HandlerAdapter_ interface, which is used in the Spring MVC (Model-View-Controller) web framework. The _HandlerAdapter_ interface defines a common interface for all controllers in the framework, allowing the framework to adapt different types of controllers to a common interface. This makes it easier to write new controllers and add them to the framework without modifying the existing code.
