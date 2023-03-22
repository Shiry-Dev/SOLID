SOLID é o acronimo para cinco príncipios de design que promovem manutenibilidade, escalabilidade reusabilidade de software.
Os príncipios do SOLID são:
- Single responsibility principle (SRP)
- Open/closed principle (OCP)
- Liskov substitution principle (LSP)
- Interface segregation principle (ISP)
- Dependency inversion principle (DIP)

---
1- Single responsibilty principle (SRP)
A classe tem que ter uma única função/razão/funcionalidade
```
//bad design
public class Employee {
	public void calculateSalary() {
		//calculate salary
	}

	public void saveEmployee() {
		//save employee data to db
	}
}

//good design
public class Employee {
	public void calculateSalary() {
		//calculate salary
	}
}

public class EmployeeRepository {
	public void saveEmployee(Employee employee) {
		//save employee data to db
	}
}
```
2- Open/closed principle (OCP)
Classe/Modulos/Funções/etc devem estar abertas para extensão, mas fechadas para modificações
```
// Bad design
public class Circle {
    public double radius;
}

public class Square {
    public double side;
}

public class AreaCalculator {
    public double calculate(Object shape) {
        if (shape instanceof Circle) {
            return Math.PI * ((Circle) shape).radius * ((Circle) shape).radius;
        } else if (shape instanceof Square) {
            return ((Square) shape).side * ((Square) shape).side;
        }
        return 0;
    }
}

// Good design
public interface Shape {
    double calculateArea();
}

public class Circle implements Shape {
    public double radius;

    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}

public class Square implements Shape {
    public double side;

    @Override
    public double calculateArea() {
        return side * side;
    }
}

public class AreaCalculator {
    public double calculate(Shape shape) {
        return shape.calculateArea();
    }
}
```
3- Liskov substitution principle (LSP)
subtypes must be substitutable for their base types wihout affecting the correctness of the program
```
// Bad design
public class Bird {
    public void fly() {
        // Implementation
    }
}

public class Penguin extends Bird {
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Penguins can't fly.");
    }
}

// Good design
public interface FlyingBird {
    void fly();
}

public class Bird {}

public class Eagle extends Bird implements FlyingBird {
    @Override
    public void fly() {
        // Implementation
    }
}

public class Penguin extends Bird {
    // No fly method
}
```
4- Interface segregation principle (ISP)
Classes não devem depender de interfaces que não devem usar
```
// Bad design
public interface Worker {
    void work();
    void eat();
}

public class HumanWorker implements Worker {
    @Override
    public void work() {
        // Implementation
    }

    @Override
    public void eat() {
        // Implementation
    }
}

public class RobotWorker implements Worker {
    @Override
    public void work() {
        // Implementation
    }

    @Override
    public void eat() {
        throw new UnsupportedOperationException("Robots don't eat.");
    }
}

// Good design
public interface Worker {
    void work();
}

public interface Eater
```
5- Dependency inversion principle (DIP)
Desacoplar a responsabilidade de um objeto em uma classe
> definir uma interface MessageService
```
public interface MessageService {
    void sendMessage(String message, String recipient);
}
```
> definir duas classes EmailService e SMSService que implemente a interface
```
public class EmailService implements MessageService {
    @Override
    public void sendMessage(String message, String recipient) {
        System.out.println("Sending email to " + recipient + " with message: " + message);
    }
}

public class SMSService implements MessageService {
    @Override
    public void sendMessage(String message, String recipient) {
        System.out.println("Sending SMS to " + recipient + " with message: " + message);
    }
}
```
> definir uma classe MessageClient que recebe via construtor a interface MessageService
```
public class MessageClient {
    private final MessageService messageService;

    public MessageClient(MessageService messageService) {
        this.messageService = messageService;
    }

    public void processMessage(String message, String recipient) {
        messageService.sendMessage(message, recipient);
    }
}
```
> user um MessageClient com implementações diferentes de MessageService
```
public class Main {
    public static void main(String[] args) {
        MessageService emailService = new EmailService();
        MessageService smsService = new SMSService();

        MessageClient emailClient = new MessageClient(emailService);
        MessageClient smsClient = new MessageClient(smsService);

        emailClient.processMessage("Hello, Email!", "email@example.com");
        smsClient.processMessage("Hello, SMS!", "123-456-7890");
    }
}
```
neste exemplo, a classe "MessageClient" não depende diretamente de nenhuma implementação expecífica da interface "MessageService". Ela recebe a implementação via construtor.
