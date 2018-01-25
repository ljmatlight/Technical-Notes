
If you want those listeners to be registered automatically regardless of the way the application is
created you can add a `META-INF/spring.factories` file to your project and reference your
listener(s) using the `org.springframework.context.ApplicationListener` key

> spring.factories

```factories

org.springframework.context.ApplicationListener=com.example.project.MyListener

```

Application events are sent in the following order, as your application runs:
1. An `ApplicationStartingEvent` is sent at the start of a run, but before any processing except
the registration of listeners and initializers.
2. An `ApplicationEnvironmentPreparedEvent` is sent when the Environment to be used in the
context is known, but before the context is created.
3. An `ApplicationPreparedEvent` is sent just before the refresh is started, but after bean definitions
have been loaded.
4. An `ApplicationReadyEvent` is sent after the refresh and any related callbacks have been
processed to indicate the application is ready to service requests.
5. An `ApplicationFailedEvent` is sent if there is an exception on startup.
