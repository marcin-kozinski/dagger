## A new command

Let's add a new command for logging in to the ATM:

```java
final class LoginCommand extends SingleArgCommand {
  private final Outputter outputter;

  @Inject
  LoginCommand(Outputter outputter) {
    this.outputter = outputter;
  }

  @Override
  public String key() {
    return "login";
  }

  @Override
  public Status handleArg(String username) {
    outputter.output(username + " is logged in.");
    return Status.HANDLED;
  }
}
```

(The abstract `SingleArgCommand` we're using for simplicity is defined
[here][SingleArgCommand]).

We can create a `LoginCommandModule` like our `HelloWorldModule` to bind
`LoginCommand` as a `Command`:

```java
@Module
abstract class LoginCommandModule {
  @Binds
  abstract Command loginCommand(LoginCommand command);
}
```

To start using the `LoginCommand` in `CommandRouter`, replace `HelloWorldModule`
in the `@Component` annotation with `LoginCommandModule`. Run the application
and try to log in.

This begins to show some of the benefits of using Dagger. With a one line
_declarative_ change, we were able to change what `Command` was received by
`CommandRouter`. `CommandRouter` had no changes, it just worked. Using this
approach, you can write many different versions of your application and reuse
code without massive changes.

[Previous](05-abstraction-for-output) · [Next](07-two-for-the-price-of-one)
{@paragraph style="text-align: center"}

[SingleArgCommand]: https://github.com/google/dagger/tree/master/java/dagger/example/atm/SingleArgCommand.java