# WPZylos Events

[![PHP Version](https://img.shields.io/badge/php-%5E8.0-blue)](https://php.net)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![GitHub](https://img.shields.io/badge/GitHub-WPDiggerStudio-181717?logo=github)](https://github.com/WPDiggerStudio/wpzylos-events)

PSR-14 compliant event dispatcher for WPZylos framework.

📖 **[Full Documentation](https://wpzylos.com)** | 🐛 **[Report Issues](https://github.com/WPDiggerStudio/wpzylos-events/issues)**

---

## ✨ Features

- **PSR-14 Compliant** — Standard event dispatcher interface
- **Event Classes** — Type-safe event objects
- **Listeners** — Single event handlers
- **Subscribers** — Multi-event handlers
- **Stoppable Events** — Control event propagation
- **Queued Events** — Async event processing

---

## 📋 Requirements

| Requirement | Version |
| ----------- | ------- |
| PHP         | ^8.0    |

---

## 🚀 Installation

```bash
composer require wpdiggerstudio/wpzylos-events
```

---

## 📖 Quick Start

```php
use WPZylos\Framework\Events\EventDispatcher;

$dispatcher = new EventDispatcher();

// Register listener
$dispatcher->listen(UserCreated::class, function (UserCreated $event) {
    // Handle event
});

// Dispatch event
$dispatcher->dispatch(new UserCreated($user));
```

---

## 🏗️ Core Features

### Event Classes

```php
class UserCreated
{
    public function __construct(
        public readonly User $user
    ) {}
}

class OrderPlaced
{
    public function __construct(
        public readonly Order $order,
        public readonly User $customer
    ) {}
}
```

### Listeners

```php
// Closure listener
$dispatcher->listen(UserCreated::class, function (UserCreated $event) {
    mail($event->user->email, 'Welcome!', 'Thanks for signing up.');
});

// Class listener
$dispatcher->listen(UserCreated::class, [SendWelcomeEmail::class, 'handle']);
```

### Subscribers

```php
class UserEventSubscriber implements EventSubscriberInterface
{
    public static function getSubscribedEvents(): array
    {
        return [
            UserCreated::class => 'onUserCreated',
            UserDeleted::class => 'onUserDeleted',
        ];
    }

    public function onUserCreated(UserCreated $event): void
    {
        // Handle creation
    }

    public function onUserDeleted(UserDeleted $event): void
    {
        // Handle deletion
    }
}

$dispatcher->addSubscriber(new UserEventSubscriber());
```

### Stoppable Events

```php
class ValidatableEvent implements StoppableEventInterface
{
    private bool $propagationStopped = false;

    public function stopPropagation(): void
    {
        $this->propagationStopped = true;
    }

    public function isPropagationStopped(): bool
    {
        return $this->propagationStopped;
    }
}
```

---

## 📦 Related Packages

| Package                                                                | Description            |
| ---------------------------------------------------------------------- | ---------------------- |
| [wpzylos-core](https://github.com/WPDiggerStudio/wpzylos-core)         | Application foundation |
| [wpzylos-hooks](https://github.com/WPDiggerStudio/wpzylos-hooks)       | WordPress hooks        |
| [wpzylos-scaffold](https://github.com/WPDiggerStudio/wpzylos-scaffold) | Plugin template        |

---

## 📖 Documentation

For comprehensive documentation, tutorials, and API reference, visit **[wpzylos.com](https://wpzylos.com)**.

---

## ☕ Support the Project

If you find this package helpful, consider buying me a coffee! Your support helps maintain and improve the WPZylos ecosystem.

<a href="https://www.paypal.com/donate/?hosted_button_id=66U4L3HG4TLCC" target="_blank">
  <img src="https://img.shields.io/badge/Donate-PayPal-blue.svg?style=for-the-badge&logo=paypal" alt="Donate with PayPal" />
</a>

---

## 📄 License

MIT License. See [LICENSE](LICENSE) for details.

---

## 🤝 Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

---

**Made with ❤️ by [WPDiggerStudio](https://github.com/WPDiggerStudio)**
